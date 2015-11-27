---
layout: default
title: Preprocessing Data
permalink: /guide/preprocessing/
---

## Preprocessing results

Some states provide results data in a way that is not easily loaded using the data pipleline.  This could be because the data is provided in a format that isn't easily machine readable, such as an image PDF, or a format that isn't easy to inspect or load incrementally, such as a database dump stored in monolithic files.  Also, results may not be publically accessible and may have been obtained on physical media or by email.

In these cases, the data needs to be converted to CSV and placed in a separate repository that is accessible over HTTP.  Thes [datasource](/guide/#datasource) should be implemented so that it points to the files in the repository.

Examples of states with data repositories include:

* [Mississippi](http://github.com/openelections/openelections-data-ms)
* [Oregon](http://github.com/openelections/openelections-data-or)

## Data repositories

Repositories of preprocessed data should be named ``openelections-data-{state_abbreviation}``.  For example, Mississippi's repository is named ``openelections-data-ms``.

## Data entry

For those records that have to be manually typed in, see our [data entry](http://docs.openelections.net/data-entry/) page.

## Supporting code

In many cases, scraping and processing the raw data requires Python code.  Processing scripts should be placed in a ``bin`` directory and any common supporting code should go in a packaged named ``openelexdata.us.{state_abbreviation}}``, but we're happy to accept one-off scrapers and the output. We prefer Python, but are most concerned about the quality of the data, so if Ruby or Node is more your thing, contact us on [Github](https://github.com/openelections), [Twitter](https://twitter.com/openelex) or by email at openelections@gmail.com and we'll figure it out.

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
