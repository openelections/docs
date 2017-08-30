---
layout: default
title: Converting Raw Results to Data
permalink: /guide/converting
---

## Converting Raw Results to Data

Some states provide results data in a way that is not easily loaded using the data pipleline.  This could be because the data is provided in a format that isn't easily machine readable, such as an image PDF, or a format that isn't easy to inspect or load incrementally, such as a database dump stored in monolithic files.  Also, results may not be publically accessible and may have been obtained on physical media or by email.

In these cases, the data needs to be converted to CSV and placed in a separate repository that is accessible over HTTP.  Thes [datasource](/guide/#datasource) should be implemented so that it points to the files in the repository.

Examples of states with data repositories include:

* [Mississippi](http://github.com/openelections/openelections-data-ms)
* [Arkansas](http://github.com/openelections/openelections-data-ar)

## Task description

The inputs to this task are usually PDFs and sometimes database files provided by state-level and county-level officials (i.e. county clerks).

The outputs to this task are CSVs whose rows look like this: `county,precinct,office,district,candidate,party,votes`

## Where to get PDFs to convert

You should look through [the OpenElections github](https://github.com/openelections) for repos with names that start with `openelections-data`. We're working on writing issues that have easy PDFs for everyone to take on.

## How to complete this task

The two most common ways of performing this task are by data entry and by using [Tabula](http://www.tabula.technology), a Java-based software that extracts data from compuer-generated PDFs. Both are great methods for non-technical contributors, so we recommend both depending on the size of the PDFs.

Generally, results with less than 10 precincts can easily be done by data entry. Anything more than that should be done by Tabula, unless the PDFs are image-based PDFs (someone scanned in a page vs. a computer generated the PDF directly).

If you are looking at doing data entry:

1. Download and open the raw results PDF
2. In Excel or OpenOffice Calc, set your first row as `county,precinct,office,district,candidate,party,votes`
3. In the results PDF, count the number of precincts in the county
4. For each precinct, write a row that fills in the first two cells. Example: `Harney,Burns 21`, `Harney,Hines 25`
5. In the results PDF, count the total number of candidates that appear in the races. A couple of things to note here. First, we're only looking at federal (e.g. U.S. Senate, President) and state (e.g. Attorney General, Secretary of State, State House) races. Second, you should count write-ins, over votes and under votes each as a candidate. So, an uncontested race will really have four candidates: the candidate, write-ins, over votes and under votes.
6. Copy and paste the set of precinct rows for each candidate so that each precinct-candidate combination will have a row (use `CTRL+C`, `CTRL+V` here)
7. Next, go through each race and label the appropriate number of rows with the office and district. Example: a race for the State Senate in the 5th district will have the labels `State Senate` and `5` for office and district (use `CTRL+D` to label once and copy all the way down)
8. Fill in each race with the candidate names (use `CTRL+D` to label once and copy all the way down)
9. Finally, fill in the vote counts for each row (resize windows so that PDF and CSV are open at the same time)

Useful shortcuts for this method:

* `CTRL+Arrow key` - move to the last cell in the direction you chose that is filled with content
* `CTRL+C`, `CTRL+V` - copy and paste
* `SHIFT+Arrow key` - expand the selection in the direction you chose. Combine with `CTRL+Arrow key` to select all cells from current to the last one that is filled
* `CTRL+D` - copy value/formula down in the current selection

If you are looking at using Tabula, use [this tutorial video](https://www.youtube.com/watch?v=of9680dgqIc) to learn how to use Tabula. The output will still require a lot of wrangling in Excel or OpenOffice Calc.
These sorts of outputs require things like Text to Column, Find and Replace, LEFT() MID() RIGHT(), TRIM(), UPPER() LOWER(), INDEX() MATCH(), etc. to properly wrangle the data. There is no standard way to wrangle the data - the best you can do is use as many Excel/Calc shortcuts and tools as you can to make this go as quickly as possible.

No matter which method you used, save the CSV using the format `YYYYMMDD__stateabrev__electiontype__countyname__doctype.csv` Most of the time, converting raw results is at the precinct level, so you'd use something like this: `20121106__or__general__harney__precinct.csv`. Usually, the Github issue that you are helping with will have the CSV file name to use. Otherwise, email us for help.

## Office names

We standardize office names as the following:

* President
* U.S. Senate
* U.S. House
* State Senate
* State House
* Governor
* Attorney General
* State Treasurer
* Secretary of State

## Need help?

You should [email us](mailto:openelections@gmail.com), ask in the [Google Group](https://groups.google.com/forum/?fromgroups#!forum/openelections), or use [completed docs](https://github.com/openelections/openelections-data-or/tree/master/2010) as reference.



