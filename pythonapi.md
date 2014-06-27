---
layout: default
title: Python API
permalink: /guide/python-api/
---

## openelex.base.datasource

### class BaseDatasource([state=''])

Wrapper for interacting with source data.

Its primary use is serving as an interface to a state's source data,
such as URLs of raw data files, and for standardizing names of result
files.

It should be subclassed in state-specific modules as
``openelex.us.{state_abbrev}.datasource.Datasource``.

### BaseDatasource.elections([year=None])

Retrieve election metadata for this state.

**Args:**

* year: Only return metadata for elections from the specified year,
provided as an integer.  Defaults to returning elections
for all years.

**Returns:**

A dictionary, keyed by year.  Each value is a list of dictonariess, 
each representing an election and its metadata for that year.

The election dictionaries match the output of the
[Metadata API](http://docs.openelections.net/metadata-api/).

The election dictionaries have an additional ``slug`` key that
can be used as an election identifier.

### BaseDatasource.mappings([year=None])

Retrieve source URL, standardized filename and other metadata for a results file.

This must be implemented in the state-specific class.

**Args:**

* year: Only return mappings for elections from the specified year,
        provided as an integer.  Defaults to returning mappings for
        all elections.

**Returns:**

A list of dicts, each containing source URL and standardized
filenames for a results file, along with other pieces of metadata.

For a state with one results file per election, the returned list
will contain only a single mapping dictionary.  For states that split results
across multiple data files by county, congressional district or
precinct, there will be one dictionary per result file.

The return dictionary should include the following keys:

* **ocd_id**: The Open Civic Data identifier for the jurisdiction
    that the data file covers.
* **election**: An identifier string for the election.  You
    should use the ``slug`` value from the dictionaries returned by
    the ``elections()`` method.
* **raw_url**: The full URL to the raw data file.
* **generated_filename**: The standardized filename that will be
    used for the locally cached copy of the data.  For more on
    filename standardization, see
    http://docs.openelections.net/archive-standardization/
* **name**: The name of the jurisdiction that the data file covers.

**Example mapping dictionary:**


{% highlight python %}
{
  "ocd_id": "ocd-division/country:us/state:md/place:baltimore",
  "election": "md-2012-04-03-primary",
  "raw_url": "http://www.elections.state.md.us/elections/2012/election_data/Baltimore_City_By_Precinct_Republican_2012_Primary.csv",
  "generated_name": "20120403__md__republican__primary__baltimore_city__precinct.csv",
  "name": "Baltimore City"
}
{% endhighlight %}

The ``ocd_id`` and ``name`` values in the returned dictionary
should reflect the jurisdiction that the data covers.  For
example, a file containing results for all of Delaware would have
an ocd\_id of ``ocd-division/country:us/state:de`` and a name of
"Delaware".

A file containing results for Worcester County, Maryland would
have an ocd\_id of 
``ocd-division/country:us/state:md/county:worcester`` and a name
of "Worcester".


### BaseDatasource.target\_urls([year=None])

Retrieve source data URLs.

This must be implemented in the state-specific class.

**Args:**

* year: Only return URLs for elections from the specified year,
    provided as an integer.  Default is to return URLs for all
    elections.

**Returns:**

A list of source data URLs.

### BaseDatasource.filename\_url\_pairs([year=None])

Retrieve standardized filename, source url pairs.

This must be implemented in the state-specific class.

**Args:**

* year: Only return URLs for elections from the specified year.
Default is to return URL and filename pairs for all elections.

**Returns:**

A list of standardized filename, source url pairs.

### BaseDatasource.unprocessed\_filename\_url\_pairs([year=None])

Retrieve standardized filename, source URL pairs for unprocessed files.

This should be implemented in some state-specific classes.

It is only needed for states where we have to preprocess the raw data
from the state, for example, to convert PDFs to CSV.  In most cases,
you won't have to implement this.

**Args:**

* year: Only return URLs for elections from the specified year.
Default is to return URL and filename pairs for all elections.

**Returns:**

An array of tuples of standardized filename, source URL pairs.


### BaseDatasource.jurisdiction\_mappings([filename=None])

Retrieve jurisdictional mappings based on OCD IDs.

**Args:**

* filename: Filename of the CSV file containing jurisdictional
mappings.  Default is 
openelex/us/{state_abbrev}/mappings/{state_abbrev}.csv.

**Returns:**

A list of dictionaries containing jurisdiction Open Civic Data
identifiers, jurisdiction names and other metadata about
the jurisdiction.  The return dictionaries include a
value for each column in the input CSV.

Example jurisdiction mapping dictionary:

{% highlight python %}
{
    'ocd_id': 'ocd-division/country:us/state:ar/county:washington',
    'fips': '05143',
    'name': 'Washington'
}
{% endhighlight %}

### BaseDatasource._counties

Retrieve jurisdictional mappings for a state's counties.

**Returns:**

A list of dictionaries containing jurisdiction metadata, as
returned by ``jurisdictional_mappings()``.

### BaseDatasource.\_election\_slug(election)

Generate a slug for an election.

**Args:**

* election: Dictionary of election attributes as returned by the
metadata API.

**Returns:**

A string containing a unique identifier for an election.  For
example, "ar-2012-05-22-primary".

### BaseDatasource.\_url\_paths([filename=None])

Load URL metadata from a CSV file.

The CSV file should follow the conventsions described at
http://docs.openelections.net/guide/#populating-urlpathscsv

**Args:**

* filename: Path to a URL paths CSV file.  Default is
openelex/{state\_abbrev}/mappings/url\_paths.csv

**Returns:**

A list of dictionaries, with each dict corresponding to a row
in the CSV file.

### BaseDatasource.\_url\_paths\_for\_election(election, [filename=None])

Retrieve URL metadata entries for a single election.

**Args:**

* election: Election metadata dictionary as returned by the
elections() method or string containing an election slug.

**Returns:**

A list of dictionaries, like the return value of
``_url_paths()``.

### BaseDatasource.\_standardized\_filename(election, [bits=None[, \*\*kwargs]])

Standardize a result filename for an election.

For more on filename standardization conventsions, see
http://docs.openelections.net/archive-standardization/.

**Args:**

* election: Dictionary containing election metadata as returned by
the elections API. Required.
* bits: List of filename elements.  These will be prepended to the
filename.  List items will be separated by "\_\_".
* reporting_level: Slug string representing the reporting level of
the data file.  This could be something like 'county' or
'precinct'.
* jurisdiction: String representing the jurisdiction of the data
covered in the file.
* office: String representing the office if results are for a single
office.
* office_district: String representing the office district numver if
the results in the file are for a single office.
* extension: Filename extension, including the leading '.'. 
Defaults to extension of first file in election's
``direct_links``.

**Returns:**

A string representing the standardized filename for
an election's data file.

### A simple state datasource module

This is an example state datasource module for the most simple possible case, a state where there is a single data file for each election.  Your state will likely need more logic, but this should give an idea of which methods in ``BaseDatasource`` you need to override.

This module would be defined in the file ``openelex/us/{state\_abbrev}/datasource.py``.

{% highlight python %}
from openelex.base.datasource import BaseDatasource

class Datasource(BaseDatasource):
    def mappings(self, year=None):
        mappings = []
        for yr, elecs in self.elections(year).items():
            mappings.extend(self._build_metadata(yr, elecs))

        return mappings

    def target_urls(self, year=None):
        return [item['raw_url'] for item in self.mappings(year)]

    def filename_url_pairs(self, year=None):
        return [(item['generated_filename'], item['raw_url']) 
                for item in self.mappings(year)]

    def _build_metadata(self, year, elections):
        meta_entries = []
        for election in elections:
            meta_entries.append({
                "generated_filename": self._standardized_filename(election),
                "raw_url": election['direct_links'][0],
                "ocd_id": "ocd-division/country:us/state:{}".format(self.state),
                "name": self.state, 
                "election": election['slug']
            })
        return meta_entries

{% endhighlight %} 

## openelex.lib

### openelex.lib.build\_github\_url(generated\_filename)

Generate a URL to a preprocessed result file hosted on GitHub

Args:
    generated_filename: Standardized filename of an election result file.

Returns:
    String containing a URL to the preprocessed result file on GitHub.
