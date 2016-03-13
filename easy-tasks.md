---
layout: default
title: Easy Tasks
permalink: /easy-tasks/
---

## Getting Involved  – Easy Tasks

We know that as a volunteer, your time is a gift to us. We want to make it easy for you to contribute in ways large and small. Got 10 minutes to do some data entry? Great! Maybe you can write a one-off scraper? We'll take it. Adopt an entire state? Awesome. We love any and all contributors.

Here is why easy tasks are important - we need to collect election results from all 50 states (plus D.C.!) and that process can't be fully automated. Volunteers completing easy tasks make it easier for us to focus on solving more complex issues and fill in crucial gaps in our data.

### Quick and Easy Ways to Help OpenElections

Look in `openelections-{state}-data` repositories for Github issues labeled as "easy task" - these are small jobs that could involve data entry, a quick scrape or conversion from a PDF file. Here is an example from [Mississippi](https://github.com/openelections/openelections-data-ms/labels/easy%20task) and here are others from [Ohio](https://github.com/openelections/openelections-data-oh/labels/easy%20task).

### Turning PDFs into CSVs

For PDFs that are electronic (that you could copy and paste results from), there are several options for turning the results into CSV files. We recommend using [Tabula](http://tabula.technology/) to extract tables from electronic PDFs, and we have a short video that shows how we use it:

[![Using Tabula for Extracting Data from PDFs](http://img.youtube.com/vi/of9680dgqIc/0.jpg)](https://www.youtube.com/watch?v=of9680dgqIc)

You could also use PDF readers that can extract text, or the command-line tool [xPDF](http://www.foolabs.com/xpdf/) if you're comfortable with the command line. You can automate as little or as much of the process as you want; if copying extracted text into a spreadsheet and then arranging it is what you are able to do, that's great.

### Turning HTML into CSVs

Web pages that have tables in them can be even easier to work with - in most cases you can copy the table directly into a spreadsheet and then re-arrange the data to fit our layout. Don't worry about formatting - saving the file as a CSV removes any text formatting.

### Getting Started

We have PDF extraction or HTML conversion tasks in the following states:

* [Mississippi](https://github.com/openelections/openelections-data-ms/issues)
* [D.C.](https://github.com/openelections/openelections-data-dc/labels/easy%20task)
* [Georgia](https://github.com/openelections/openelections-data-ga/labels/easy%20task)
* [Tennessee](https://github.com/openelections/openelections-data-tn/labels/easy%20task)
* [Utah](https://github.com/openelections/openelections-data-ut/issues?q=is%3Aissue+is%3Aopen+label%3A%22easy+task%22)
* [South Carolina](https://github.com/openelections/openelections-data-sc/issues?q=is%3Aissue+is%3Aopen+label%3A%22easy+task%22)

If you do contribute code, take a look at [how we do it](http://docs.openelections.net/guide/preprocessing/) and please don't hesitate to ask us any questions. All CSV output files should go in year-specific folders in the specific `openelections-{state}-data` repository, but if you're not familiar with Git or Github, that's fine - you can email us the finished CSVs at openelections@gmail.com. Thanks!
