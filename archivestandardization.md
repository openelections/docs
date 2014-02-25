---
layout: default
title: Datasource Archive and Standardization
permalink: /archive-standardization/
---

## Datasource Archive and Standardization

The goal is to archive the raw version of data that seeds our data processing pipeline for each state.  These raw files will vary in granularity and type:

* HTML
* CSV/Excel files
* PDFs
* Database dumps
* Manually keyed data

The datasource.py for each state provides the link between raw data and year-by-year breakdowns of that data, regardless of its original form. This might mean, for example, that in the case of a single large database dump, we pre-process the dump and store yearly files on S3 in addition to the raw dump; or if we outsource to MechanicalTurk, we chunk out the processing in a similar year-by-year fashion that datasource.py can interface with.

Our filename standardization conventions for the “common” case should be distilled from [Github](https://github.com/openelections/core/issues/5) for reference.

The basic format for a generated filename is as follows:

date__state__{party}__election_type__{special}__{jurisdiction}{office}__{reporting_level}.format

So, for example:

	20121106__md__general__queen_annes__precinct.csv

Which contains precinct-level results from the Nov. 6, 2012 Maryland general election in Queen Anne’s county. A primary election might look like this:

	20120403__md__republican__primary__prince_georges__precinct.csv

Not every state has individual files for a jurisdiction, however. Ohio has precinct-level results for an entire election:

	20041102__oh__general__precincts.xls

Or a single file for a special general election for a single office:

	20071211__oh__general__special__house__5.csv
