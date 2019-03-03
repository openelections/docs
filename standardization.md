---
layout: default
title: Standardization
permalink: /standardization/
---

## Getting Involved  – Standardization

After we collect and convert election results data, often there's some cleanup to be done. In many cases, that means standardizing office, party and candidate names. If we convert county-by-county, it also could mean generating a statewide precinct file with a minimum set of columns. We'll walk through standardization tasks here and provide some opportunities to contribute.

## Standardizing Office Names

For our purposes, we include some pseudo-offices in this column. They include:

* Registered Voters
* Ballots Cast
* Straight Party

For any election, the minimum set of offices we collect results for are federal, statewide and state legislative offices, excluding judicial positions. For federal offices we standardize names as follows:

* President
* U.S. Senate
* U.S. House

The goal for statewide and state legislative office names is to keep them standard within a state, allowing for some variation across states. Here are some state office names we use:

* Governor
* Lieutenant Governor
* Attorney General
* Secretary of State
* State Treasurer
* State Auditor
* State Comptroller
* Commissioner of Insurance
* Agriculture Commissioner
* State Senate
* State House
* State Representative
* State Assembly

Some states have fairly unique offices; Texas has a `Commissioner of the General Land Office` and for Nebraska's unicameral legislative body we use `State Legislature`. The rule to follow is to look at what office names we've used previously or have specified in the GitHub issues. We also don't list multiple offices that run on the same ticket, so we use `Governor` instead of `Governor and Lt. Governor`.

How you standardize office names is up to you. It could range from simple find-and-replace commands in a text editor to programmatic approaches.

Please note: when we standardize legislative offices with districts, we put the district value in the `district` column.

#### Office Standardization Tasks for 2018

* [California](https://github.com/openelections/openelections-data-ca/issues/138)
* [New York](https://github.com/openelections/openelections-data-ny/issues/61)
* [New Jersey](https://github.com/openelections/openelections-data-nj/issues/39)

## Standardizing Candidate Names

We don't require this, mostly because it's a lot of work depending on the state. But if it's a small set of candidates or if the variations are plentiful (as happens in presidential races), we welcome standardized candidate names. Our preferred style is `First Name Last Name` rather than last name first. As a default, we use the name on the results source unless it is spelled incorrectly or only consists of a last name.

## Generating Statewide Precinct Files

For states where we collect and convert each county's results, we'll want to stitch together a statewide precinct file. The complication is that different counties report different kinds of results. For example, not every county in Texas reports early voting as a separate category. This step should be done after standardizing office and candidate names to make the statewide file more useful. [Here's an example](https://github.com/openelections/openelections-data-ms/blob/master/statewide_generator.py) of a simple Python script with functions to generate a statewide file that could be adapted to other states. To do so, you'd need to make sure that you had the correct offices and vote types included.

#### States Needing 2018 Statewide Precinct Files

* [Utah](https://github.com/openelections/openelections-data-ut/tree/master/2018) (General)
* [Georgia](https://github.com/openelections/openelections-data-ga/tree/master/2018) (General, Primary, Primary Runoff)
* [Iowa](https://github.com/openelections/openelections-data-ia/tree/master/2018) (Primary)
