---
layout: default
title: Code Contribution Guide
permalink: /guide/
---

## Contributing Code

Every state presents unique challenges when it comes to data. Some results will require manual clean-up or data entry, steps that are hard to capture in code. Others will be perfectly structured CSVs, ready for any mere mortal to download and use.

Whether you're facing a Sisyphean challenge or handling the friendliest of data, the OpenElections team will work with you to bring raw results into our world (a much cleaner, saner world, we hope!).

At the 30,000 foot level, here's what our "world" looks like:

1. Acquire raw results data from the state
2. Standardize the name of raw data files
3. Load raw data into our backend data store (mongo, if you're wondering)
4. Standardize results data in a step-by-step fashion, for example, by writing separate bits of code to parse a name into separate fields, to standardize party and office names, etc.
5. Check data accuracy
6. Bake out beautifully structured, easy-to-use CSVs and JSON. Mere mortals rejoice!

We use Python (2.7 for now) to accomplish most of these tasks.

Here's a high-level snapshot of how a state directory's code is organized, using Maryland as an example:

	openelex/us/md/
	    process.md - Markdown-formatted description of non-standard data gathering process, if not fully automated.
	    corrections.csv - CSV containing info on corrections to be applied to raw results.
	    datasource.py - maps raw data sources to standardized filenames and metadata
	    mappings/ - misc metadata files for data processing pipeline
	    load/ | load.py - data loaders from local cache into mongo
	    parse/  - (optional) break out parsers if you have lots of them
	    transform/ | transform.py - step-wise data clean ups
	    validate/ | validate.py - data integrity tests
	    bake/ | bake.py

As a contributor, we appreciate any part of the data process you can help us with. Let's dive in!

## Bootstrapping

Before you can do any coding, you have to [bootstrap your environment](https://github.com/openelections/core). Once that's done, you're ready to start feeding data into our pipeline.

## Data Pipeline

#### Overview

OpenElections uses [invoke](http://docs.pyinvoke.org/en/latest/), a task execution framework, to manage the data processing pipeline. You don't have to write any invoke tasks of your own, but you can use invoke to make local development easier and help us process the data you've gathered.

Invoke provides command-line tasks to:

* Inspect data source urls and associated metadata
* Download raw data and save it locally under a standardized file name
* Inspect and clear files downloaded to a local cache
* Load data from cache into the data store
* Apply transformations to stored data
* Run validations to ensure data integrity
* Bake standardized results out to CSVs and JSON

The OpenElections team will help you wire up your code so that it "just works" using the invoke framework.

Here's a quick primer on how to get started.

Pre-flight check:

* Have you [bootstrapped your environment](https://github.com/openelections/core)?
* Have you navigated on the command line to the core/openelections directory? Note, you can only use invoke tasks from this directory. Errors will be thrown if you try to use invoke from core or a subdirectory such as core/openelex/us.

You can learn more about available invoke tasks by dropping to the command line:

```
cd /path/to/openelex-core/openelex
invoke --help
```

List available invoke tasks.

```
invoke --list
```

View help for a specific task.

```
invoke --help cache.clear
```

#### Datasource

Our data processing pipeline starts with ``datasource.py``, which handles the process of taking source files and organizing them for our system. This module encapsulates all the information needed to download data files from source agencies, save the files locally under a standardized name, and make them available for parsing and loading into our data store.

Like the other parts of the process, a state-specific datasource.py subclasses a `BaseDatasource` that includes some generic functions for the most common tasks. Inside the state-specific datasource.py you'll write functions that handle parts of the process particular to that state: setting up scrapers, handling unusual file paths or a mix of formats.

[Most states involve more than a simple web scrape](archive-standardization). You might have one big database dump for all election years, or unparseable PDFs that require outsourcing to Mechanical Turk for data entry.  More likely, you have a combination of easy-to-use data and older files that are harder to work with. The OpenElections team will work with you to come up with a solution that feeds into our data pipeline. It can be a heavy lift, but once complete, the datasource.py step makes downstream processing much easier.

A lot depends on how the results data is organized and stored by the original source. It may come as single spreadsheet files, individual HTML pages or PDF documents. The job for the datasource file is to provide an interface to that data, rename the files using OpenElections conventions and provide some basic political geographic elements. For example, here is the representation of a single results file from Baltimore City for the 2012 primary, mapping it to a OpenElections generated filename:

{% highlight json %}
{
  "ocd_id": "ocd-division/country:us/state:md/place:baltimore",
  "election": "md-2012-04-03-primary",
  "raw_url": "http://www.elections.state.md.us/elections/2012/election_data/Baltimore_City_By_Precinct_Republican_2012_Primary.csv",
  "generated_name": "20120403__md__republican__primary__baltimore_city__precinct.csv",
  "name": "Baltimore City"
}
{% endhighlight %}

Ideally, the generated files correspond directly to the original source files so that even if the original file is a PDF and the generated file CSV, they both cover the same contests and results. In the case of most PDFs, the `generated_name` will be a CSV file.

The best-case scenario is that all election results data for a state is available in the same file format, which means that you'd likely only need a small handful of functions to create the mappings. At the other end of the spectrum are states that use a combination of formats and naming conventions over time.

To help construct these mappings, you should store helper files in a {state}/mappings directory. The first is ``{state}.csv``, which lists political jurisdictions (usually counties, but can also include cities and other types of divisions that are reporting levels) and connects them to the [OCD_ID](https://github.com/opencivicdata/ocd-division-ids). In states where it is necessary to directly map URLs to `generated_name`, a ``url_paths.csv`` file can be used (see [Ohio](https://github.com/openelections/core/blob/dev/openelex/us/oh/mappings/url_paths.csv) for an example).

Once you've crafted `datasource.py`, you can query it for information about a state's data sources from the command line.

NOTE: Below code snippets use Maryland as an example.

List election metadata for all years (from our [Metadata API](http://blog.openelections.net/2013/10/23/an-improved-metadata-api/); warning: output is very verbose).

```
invoke datasource.elections --state md
```

List metadata for one year.

```
invoke datasource.elections --state md --datefilter 2012
```

List urls for raw data sources.

```
invoke datasource.target_urls --state md --datefilter 2012
```

List the mapping between raw data source urls and standardized file names (saved to the local cache).

List all years.

```
invoke datasource.filename_url_pairs --state md
```

List one year.

```
invoke datasource.filename_url_pairs --state md --datefilter 2012
```

And most importantly, the datasource.mappings task provides a year-by-year breakdown of metadata (raw url, standardized file name, OCD ID, and election ID). These bits of metadata are critical to the subsequent data loading step. The [OCD_ID](https://github.com/opencivicdata/ocd-division-ids) is a standardized way to refer to specific political geographies (states, counties, congressional districts, etc.) that makes it easier for OpenElections data to be shared across projects.

List all mappings for state.

```
invoke datasource.mappings --state md
```

List mappings for one year.

```
invoke datasource.mappings --state md --datefilter 2012
```

You can pipe the mappings to a filenames.json, which is a snapshot of the mappings for use in the downstream load process.

```
invoke datasource.mappings --state md > us/md/mappings/filenames.json
```

#### Fetch/Cache

The datasource.py file sets up the basic process, but it doesn't actually grab the raw result files. That's the job of ``fetch.py``. You can use the fetch task to download raw result files for your state all at once, or year by year. Downloaded files are stored under their standardized names in a {state}/cache directory (e.g. ``md/cache``). An important point: the {state}/cache directory is excluded from version control.

We use the useful `requests` library to make HTTP requests and `BeautifulSoup` for HTML parsing.

Download all files for the state.

```
invoke fetch --state md
```

Download files for one year.

```
invoke fetch --state md --datefilter 2012
```

List the contents of your local cache.

```
invoke cache.files --state md
```

List cached files for one year.

```
invoke cache.files --state md --datefilter 2012
```

#### Load

The load.py file is responsible for taking the raw results and putting them into a MongoDB database. The goal is to keep the data loader simple, and defer any data cleaning and transformations to downstream steps in the process. That said, this is where the process begins to place results data into our defined models, including `Candidate`, `Result` and `Contest`.

The `load` task imports raw results from locally cached files into the data store. This assumes you've written the data loader for your state, of course! Depending on how complex a state's data is - for example, if different years come in different formats or contain different types of data - the load.py could contain multiple functions to handle this process.

We use [unicodecsv](https://github.com/jdunck/python-unicodecsv) to read CSV files.


Load all raw results in local cache.

```
invoke load.run --state md
```

Load results for one year.

```
invoke load.run --state md --datefilter 2012
```

#### Transform

Transforms are functions that update data after they've been loaded into our [data store](http://docs.mongodb.org/manual/). They must be registered in a ``{state}\transforms.py`` file. A typical update might be parsing candidate names from a single field into name components.

List available transforms for state.

```
invoke transform.list --state md
```

MD transforms, in order of execution:

* parse_candidate_names
* standardize_office_names

Transforms are run in the order that they are registered, so ORDERING IS IMPORTANT. You can select one or more transforms to run using the *--include* flag with a comma-separated list of function names. Or you can run all but selected transforms using the *--exclude* flag. Also note that you can register validators with a transform. Validators, covered in the next section, are functions that check data after a transformation to ensure the result is correct.

Run all transforms for state.

```
invoke transform.run --state md
```

Only run name parsing transform.

```
invoke transform.run --state md --include parse_candidate_names
```

Run all transforms except name parsing.

```
invoke transform.run --state md --exclude parse_candidte_names
```

#### Validate

Validations help ensure data integrity by running queries against results in the data store, typically after a particular transformation is run (You can link one or more validators with a transformation; see the previous section). They exist in a ``{state}/validate.py`` file.

All validations should be functions prefixed with "validate_" and importable from openelex.us.state.validate (e.g. from openelex.us.md.validate import validate_unique_contests).

Similar to transformations, you can include/exclude validations.

List available validations.

```
invoke validate.list --state md
```

Run all validations for a state.

```
invoke validate.run --state md
```

Only run validate_unique_contests.

```
invoke validate.run --state md --include validate_unique_contests
```

Run all validations except for validate_unique_contests.

```
invoke validate.run --state md --exclude validate_unique_contests
```

#### Bake

This is the long-awaited final step, where we bake out raw results after applying transforms (and validations, of course!) into flat files using one or more user-friendly formats. Regardless of format, results should conform to our [standardized results spec](https://github.com/openelections/specs/wiki/Results-Data-Spec-Version-2).

Bake all results for a state.

```
invoke bake.state_file --state md
```

Bake results for one year.

```
invoke bake.state_file --state md --datefilter 2012
```

Bake results to a specific directory.  The default is OPENELEX_ROOT/us/bakery/.

```
invoke bake.state_file --state md  --datefilter 2012 --outputdir ~/data/
```

Bake results to a JSON file instead of CSV.

```
invoke bake.state_file --state md --datefilter 2012 --format json
```

## Manual Processes

#### Data extraction

Some data sources are just too unwieldy to parse using a fully automated process. Think landscape-oriented, image PDFs with angled header columns and 8-point font.  In such cases, hand-keying data will be the only option, or perhaps a combination of hand-keying and an automated process such as Tabula.

Any data extraction process that is not fully automated (and integrated into the data processing pipeline) should be documented in a process.md file. The description should be minimal, just enough info to help recreate your process. Think bullet-points for the uninitiated.

If a state requires a manual process, that process is documented in ``{state}/process.md``, along with any particular issues you might have come across during the process. These may include results that are incomplete or missing, or results which may require further investigation/follow-up with the source agency.

#### Corrections

You'll occasionally run into raw result files that contain incorrect data -- a candidate name misspelled, or candidate parties swapped. Ideally, such inaccuracies should be reported to the source agency so they can fix their data. Of course, to ensure accuracy in the meantime, a record of the correction should be stored a corrections.csv file so that during the Transform process the raw data can be fixed.

This file should live at at the root of the state directory (``core/openelex/us/<state>/corrections.csv``) and contain four columns:

* source - standardized name of raw results file that has incorrect data. From Datasource.
* description - Description of error and change that is necessary
* fixed? - [true|false]
* transform - Name of transform that applies correction. Blank until transform is written.

#### Metadata

Metadata for offices and parties are loaded from the CSV files ``us/fixtures/office.csv`` and ``us/fixtures/party.csv``.

As you encounter new offices or political parties in your state data, you should add them to these files.

The updated data  can be loaded into the database using the ``load_metadata.run`` task.  For example, to load new offices, use the command:

```
invoke load_metadata.run --collection=office
```

## En Fin

Time to celebrate! You've helped bring transparency to American democracy! And in the process, you got a sweet OpenElections T-shirt that proves to the world you helped FREE THE DATA! (You did get a shirt, right?)

But really, we owe you a hearty thanks, whichever part of this process you pitched in on. Our "modest" goal is to bring some sanity to U.S. election results, and there's always more work to be done.

We welcome your ideas on new ways to add value and generally make the raw output of democracy more accessible to all.

The OpenElections Crew

* Geoffrey Hing
* Sara Schnadt
* Serdar Tumgoren
* Derek Willis
