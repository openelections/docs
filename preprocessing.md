---
layout: default
title: Preprocessing Data
permalink: /guide/preprocessing/
---

## Preprocessing results

Some states provide results data in a way that is not easily loaded using the data pipeline.  This could be because the data is provided in a format that isn't easily machine readable, such as an image PDF, or a format that isn't easy to inspect or load incrementally, such as a database dump stored in monolithic files.  Also, results may not be publicly accessible and may have been obtained on physical media or by email.

In these cases, the data needs to be converted to CSV and placed in a separate repository that is accessible over HTTP. This could be done in a variety of ways, from one-off scripts to more customizable libraries that can handle multiple results files. You might need to perform [OCR](https://en.wikipedia.org/wiki/Optical_character_recognition) on images of results before converting them to CSV files. (For those records that have to be manually typed in, see our [data entry](http://docs.openelections.net/data-entry/) page.)

This is very important work because many states don't keep their results in a single format, which means converting a state's results into a single format (CSV) is crucial to our efforts.

## Data repositories

Repositories of preprocessed data should be named ``openelections-data-{state_abbreviation}``.  For example, Mississippi's repository is named ``openelections-data-ms``.

Preprocessed results go into state-specific data repositories, which is where you'll find tasks to work on. Some examples of state data repositories include:

* [Mississippi](http://github.com/openelections/openelections-data-ms)
* [Oregon](http://github.com/openelections/openelections-data-or)

## Examples of preprocessing tasks

Take [Oregon's 2008 general election county-level results](http://sos.oregon.gov/elections/Documents/results/results-11-2008.pdf), which are in an electronic PDF. Using Tabula or xPDF, we can manually extract the tabular data inside, copy it to a spreadsheet and convert it to a CSV file using OpenElections' conventions: the result is [here](https://github.com/openelections/openelections-data-or/blob/master/2008/20081104__or__general.csv). Here are the steps, using Tabula (and assuming you have it installed and running):

1. Save the PDF file to your desktop or downloads folder.
2. Using [Tabula's browser interface](http://127.0.0.1:34555/), import the PDF file.
3. Using Tabula's interface, draw a box around each table of results (it's best to draw one race at a time).
4. Copy the extracted data and paste it into a spreadsheet.
5. Re-arrange the data to fit our format of one candidate result per row.
6. Add other columns (county, office, district) as needed.
7. Repeat for all federal and state offices. Save often.

Tasks involving pre-processing of electronic PDFs often are tagged with our `easy task` label ([here](https://github.com/openelections/openelections-data-or/labels/easy%20task), for Oregon).

A similar approach could be used for a fixed-width text file using a text editor, although in that case most of the parsing would be done in the text editor.

Another approach could use a programming language (preferably Python) to read and parse a text or HTML results file and create a CSV file. An example is the [Python parser for results in Josephine County, Oregon](https://github.com/openelections/openelections-data-or/blob/master/src/parsers/josephine_parser.py).


## Using OCR

For image-based PDF files ([here's an example](https://github.com/openelections/openelections-sources-or/blob/master/Lincoln/G1172000.PDF) from Lincoln County, Oregon), performing OCR can make it possible to avoid data entry and parse the results as data. There are several options for OCR, but they usually require purchasing software. If you have the professional version of Acrobat, able2Extract or similar software, you can take PDF files and do OCR on their contents. If you don't have access to any of those, please contact us at openelections@gmail.com and we can figure out a way to make it work.

Because OCR is a tricky process that isn't always accurate, you'll need to review the results to ensure that they are correct - in particular, check rows in which the votes are 0 or contain 5, 6 or 8.

## Supporting code

In some cases, scraping and processing the raw data requires Python code.  Processing scripts should be placed in a ``bin`` directory and any common supporting code should go in a packaged named ``openelexdata.us.{state_abbreviation}}``, but we're happy to accept one-off scrapers and the output. We prefer Python, but are most concerned about the quality of the data, so if Ruby or Node is more your thing, contact us on [GitHub](https://github.com/openelections), [Twitter](https://twitter.com/openelex) or by email at openelections@gmail.com and we'll figure it out.

### Example of Python code structure

An example directory structure for Iowa looks like this:

{% highlight bash %}
.
├── 20000104__ia__special__general__state_house__53.csv
├── 20000606__ia__primary__county.csv
├── 20001107__ia__general__precinct.csv
├── 20001107__ia__general__president__county.csv
├── bin
│   ├── parse_2000_general_county_president.py
│   ├── parse_2000_general_precinct.py
│   └── parse_2000_primary.py
├── openelexdata
│   ├── __init__.py
│   └── us
│       ├── ia
│       │   ├── __init__.py
│       │   ├── parser.py
│       │   ├── util.py
│       ├── __init__.py
├── README.md
├── tests
│   ├── test_util.py
{% endhighlight %}

It is important to note that:

* The converted data files live at the top-level of the repository.
* There is a ``README.md`` file that describes the conversion process, the role of the supporting code and any manual processes.
* Scripts used to process the raw data are placed in the ``bin`` directory.
* Supporting library code is in the ``openelexdata`` package directory.

### Namespace packages

The ``openelexdata.us.{state_abbreviation}`` should be implemented as a [Namespace package](http://legacy.python.org/dev/peps/pep-0420/) so that the different state packages can live in separate repositories but all be imported under ``openelexdata.us``.  The easiest way to make this work is to use the [pkgutil](https://docs.python.org/2/library/pkgutil.html) package and place the following code in ``openelexdata/__init__.py`` and ``openelexdata/us/__init__.py``:

```
from pkgutil import extend_path
__path__ = extend_path(__path__, __name__)
```
