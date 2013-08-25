---
layout: default
title: Metadata API
permalink: /metadata-api/
---

## Introduction

The election Metadata API provides programmatic access to information gathered as part of the [OpenElections Metadata process](http://docs.openelections.net/getting-involved/), including details about the types of elections held and the scope of results available. This API is likely to change, although not substantially, as the project proceeds.

## Response Formats

Currently the API returns JSON, although CSV will be on the way and XML is likely. You can follow along on work on our [wiki](https://github.com/openelections/specs).

## Responses

The API currently has a single response, but others are possible, and the elections response is subject to change.

### Elections

This response returns a list of elections for a given state and year. The state parameter is a valid postal code for the 50 states and the year parameter is a four-digit year from 2000 to 2012 inclusive. A sample url looks like this:

  `http://dashboard.openelections.net/api/state/il/year/2004/`
  
A state and year for which no elections have occurred will return a 404 response from the server.