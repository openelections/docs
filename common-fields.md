---
layout: default
title: Common Fields
permalink: /common-fields/
---

# Common Fields in results files

### Introduction

Each election date represents a directory containing CSV and JSON files covering elections to specific offices. For example, Maryland's Nov. 6, 2012 general election results would be stored in a /md/2012-11-06/ directory, and the filenames would represent offices such as "president", "us-senate" and "us-house". Each office file would contain one record for each candidate listed in the results. The result records consist of at least these fields.

[Example JSON results file](https://gist.github.com/dwillis/5666920)

[Example CSV results file](https://gist.github.com/dwillis/5666870)

### Fields

| Field | Type | Description |
|---|---|---|
| election_id | string | OpenElections-created slug for election |
| office_name | string | name of office sought |
| office_district | string | district number or designation |
| division | string | political jurisdiction using [Open Civic Data Division Identifiers](https://github.com/opencivicdata/ocd-division-ids) - acts as reporting level |
| given_name | string | parsed given name of the candidate|
| additional_name | string | parsed middle name or initial of the candidate|
| family_name | string | parsed family name of the candidate |
| suffix | string | parsed suffix of the candidate |
| name | string | "raw" full name of the candidate from the results, if present |
| other_names | array | array of additional names with `note` label |
| party | string | "raw" party name or abbreviation from the results (`None` for non-partisan) |
| winner | boolean | true for the winning candidate(s) within this division; all other candidates are marked as false |
| votes | integer | number of votes received by the candidate within this division|
| pct | decimal | percentage of votes received by the candidate within this division|
| write_in | boolean | true if the candidate is a write-in candidate |

### Questions

Join us on [the Google Group](https://groups.google.com/forum/?fromgroups=#!forum/openelections) to discuss these specs, or you can [file an issue](https://github.com/openelections/docs/issues/new) on this Github repository, too.

1. Should the results data have a `results_type` attribute in the event that both Unofficial and Certified data is available? Or does that information belong in the Elections data, perhaps in the `election_id`?
2. For states like Maryland that have an Other Write-Ins value, should that go in `family_name` or just `name`?
3. Should the results data have incumbency status?
4. Should the office fields be collapsed into a single one, perhaps based on the OCD division IDs?
5. How should the `other_names` array be represented in CSV?
