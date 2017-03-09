---
layout: default
title: Common Fields
permalink: /common-fields/
---

# Common Fields in results files

### Introduction

Each election year represents a directory containing data files covering elections occurring in that year. Each election held during that year contains one record for each candidate listed in the results. The result records consist of at least these fields:

[Example CSV results file](https://github.com/openelections/openelections-results-fl/blob/master/raw/20000905__fl__primary__county__raw.csv)

### Fields

| Field | Type | Description |
|---|---|---|
| updated_at | | |
| id | string | OpenElections-created slug for election |
| start_date | string | |
| end_date | string | |
| election_type | string | |
| result_type | string | |
| special | string | |
| office | string | name of office sought |
| district | string | district number or designation |
| name_raw | string | "raw" full name of the candidate from the results, if present |
| last_name | string | parsed family name of the candidate |
| first_name | string | parsed given name of the candidate|
| suffix | string | parsed suffix of the candidate |
| middle_name | string | parsed middle name or initial of the candidate|
| party | string | "raw" party name or abbreviation from the results (`None` for non-partisan) |
| jurisdiction | string | |
| division | string | political jurisdiction using [Open Civic Data Division Identifiers](https://github.com/opencivicdata/ocd-division-ids) - acts as reporting level |
| votes | integer | number of votes received by the candidate within this division|
| vote_type | | |
| total_votes | | |
| winner | boolean | true for the winning candidate(s) within this division; all other candidates are marked as false |
| write_in | boolean | true if the candidate is a write-in candidate |
| year | integer | |

### Questions

Join us on [the Google Group](https://groups.google.com/forum/?fromgroups=#!forum/openelections) to discuss these specs, or you can [file an issue](https://github.com/openelections/docs/issues/new) on this Github repository, too.
