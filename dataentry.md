---
layout: default
title: Data Entry
permalink: /data-entry/
---

## Data Entry

We won't sugar-coat this: data entry is hard work. But in some cases, having a person type in election results is the most accurate way to get the information we need into the format we need it in. This is usually the case when a state or county stores its election results in an image-based PDF that can't be parsed properly using programmatic tools. Something that looks like this:

![Mississippi results](https://github.com/openelections/openelections-data-ms/raw/master/ms_county_example.png)

or maybe even like this:

![Oregon results](https://raw.githubusercontent.com/openelections/docs/master/assets/img/oregon_county_example.png)

File names match the [generated_name standard](http://docs.openelections.net/archive-standardization/) described in our docs. So the CSV file to match the first example would be 20121106__ms__general__chickasaw__precinct.csv. The OpenElections CSV layout approach is to mirror the results file as much as possible, with one exception: we try to have a single result on each line, rather than multiple candidates or precincts.

![CSV example](https://github.com/openelections/openelections-data-ms/raw/master/ms_county_csv_example.png)

Where totals are included, leave the precinct column blank and mark the overall winner in each race in the winner column, which takes a boolean value of TRUE for winning candidates:

![CSV example 2](https://github.com/openelections/openelections-data-ms/raw/master/ms_county_csv_example_total.png)

For elections that have only county-level results, total rows will leave the county blank for races that involve more than one county:

![CSV example 3](https://github.com/openelections/openelections-data-ms/raw/master/ms_multi_county_csv_example_total.png)

For elections in which two candidates advance to a runoff, both candidates are marked as `winner`.

### Data Entry Guidelines

* We're only interested in federal, statewide and state legislative offices; no local offices.
* Our office titles are: President, U.S. Senate, U.S. House, Governor (and other statewide officers), State Senate and State House (see more details [here](http://docs.openelections.net/archive-standardization/)).
* Don't include "County" in the county name.
* Don't include "Precinct" in the name of the precinct.
* For precincts with numbers in the name, remove any leading zeros (005 becomes 5).
* There is no set order for candidates/races: even within the same election, the order of candidates can differ between counties.

### Instructions

1. Using Excel or Google spreadsheets, create a file to hold the results you'll be typing in.
2. Use the following row headers: `county`, `precinct`, `office`, `district`, `party`, `candidate`, `votes`. Only include a column for `winner` if the winner is clearly indicated in the original data.
3. Type in the results, using our office titles listed above.
4. Don't include commas in vote totals.
5. Enter the candidate name just as it appears.
6. Save the file as a CSV file using our [naming conventions](http://docs.openelections.net/archive-standardization/). If you're not sure what to name it, give it the most descriptive and logical name you can and we'll figure it out.
7. Email us the file at openelections@gmail.com or, if you know Git, fork the `openelections-data-{state abbrev}` repository for the state you're working on and create a pull request.

### Where to Start

We have data entry tasks in the following states:

* [Oregon](https://github.com/openelections/openelections-data-or/labels/data%20entry)
