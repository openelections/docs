---
layout: default
title: Cookbook
permalink: /guide/cookbook/
---

## Cookbook

Elections data varies widely between different states and between elections in the same state.  As a result, it's not always obvious how to represent this data with the OpenElections models or how to organize code in a state module as it becomes more complex.  This documentation provides some examples of potentially confusing situations and stategies to addres them.

### Loading data

#### Multiple loader classes

In an ideal world, all results files for a state would have the same structure, or have small enough variations to implement [loader classes](/guide/#load) with a minimum amount of branching.  In reality, you'll probably need to implement multiple loader classes corresponding to the different file types or structures.  However our API defines a single entry point for loading, a ``LoadResults`` class.  You can make ``LoadResults.run()`` delegate to other loader classes based on values in the mapping.

{% highlight python %}
class LoadResults(object):
    """Entry point for data loading.

    Determines appropriate loader for file and triggers load process.

    """

    def run(self, mapping):
        election_id = mapping['election']
        if '2002' in election_id:
            loader = MDLoader2002()
        elif '2000' in election_id and 'primary' in election_id:
            loader = MDLoader2000Primary()
        elif '2008' in election_id and 'special' in election_id:
            loader = MDLoader2008Special()
        else:
            loader = MDLoader()
        loader.run(mapping)
{% endhighlight %}

#### Pseudo-jurisdictions

Consider these rows from a precinct-level results file for Iowa's Adair County:

Race | County | Precinct | ROXANNE CONLIN | THOMAS L. FIEGEN | BOB KRAUSE | WRITE-IN | Over Votes | Under Votes | Precinct Totals | Final Data?
---- | ------ | -------- | -------------- | ---------------- | ---------- | -------- | ---------- | ----------- | --------------- | -----------
U.S. SENATOR - DEMOCRATIC PARTY | Adair | 1 NW | 30.0 | 6.0 | 4.0 |  |  | 1.0 | 41.0 | Y
U.S. SENATOR - DEMOCRATIC PARTY | Adair | 2 NE | 24.0 | 4.0 | 4.0 |  |  | 1.0 | 33.0 | Y
U.S. SENATOR - DEMOCRATIC PARTY | Adair | 3 SW | 28.0 | 5.0 | 4.0 |  |  | 1.0 | 38.0 | Y
U.S. SENATOR - DEMOCRATIC PARTY | Adair | 4 SE | 10.0 | 1.0 |  |  |  | 1.0 | 12.0 | Y
U.S. SENATOR - DEMOCRATIC PARTY | Adair | 5 GF | 24.0 | 1.0 | 5.0 |  |  | 3.0 | 33.0 | Y
U.S. SENATOR - DEMOCRATIC PARTY | Adair | ABSENTEE | 16.0 |  | 8.0 |  | 1.0 | 1.0 | 26.0 | Y
Grand Totals |  |  | 132.0 | 17.0 | 25.0 |  | 1.0 | 8.0 | 183.0 |
{: .table-striped }

Since this is a precinct-level file, ``RawResult`` records created from these rows will have a ``reporting_level`` value of ``precinct``. However, there is a row where the ``Precinct`` column has a value of "ABSENTEE".  This means that absentee votes from all the precincts in the county have been aggregated together.

For rows like this:

* Create ``RawResult`` instances and set the ``reporting_level`` field to ``county``.
* Set the ``jurisdiction`` field to ``Adair``.
* Set the ``votes_type`` field to ``absentee``.

#### Multiple reporting levels in one file

Consider these rows from a precinct-level results file for Iowa's Adair County:

Race | County | Precinct | ROXANNE CONLIN | THOMAS L. FIEGEN | BOB KRAUSE | WRITE-IN | Over Votes | Under Votes | Precinct Totals | Final Data?
---- | ------ | -------- | -------------- | ---------------- | ---------- | -------- | ---------- | ----------- | --------------- | -----------
U.S. SENATOR - DEMOCRATIC PARTY | Adair | 1 NW | 30.0 | 6.0 | 4.0 |  |  | 1.0 | 41.0 | Y
U.S. SENATOR - DEMOCRATIC PARTY | Adair | 2 NE | 24.0 | 4.0 | 4.0 |  |  | 1.0 | 33.0 | Y
U.S. SENATOR - DEMOCRATIC PARTY | Adair | 3 SW | 28.0 | 5.0 | 4.0 |  |  | 1.0 | 38.0 | Y
U.S. SENATOR - DEMOCRATIC PARTY | Adair | 4 SE | 10.0 | 1.0 |  |  |  | 1.0 | 12.0 | Y
U.S. SENATOR - DEMOCRATIC PARTY | Adair | 5 GF | 24.0 | 1.0 | 5.0 |  |  | 3.0 | 33.0 | Y
U.S. SENATOR - DEMOCRATIC PARTY | Adair | ABSENTEE | 16.0 |  | 8.0 |  | 1.0 | 1.0 | 26.0 | Y
Grand Totals |  |  | 132.0 | 17.0 | 25.0 |  | 1.0 | 8.0 | 183.0 |
{: .table-striped }

Most rows will result in ``RawResult`` records with a ``reporting_level`` field set to ``precinct``.  However, this result file also includes county-level totals for each race, as designated by the row with "Grand Totals" in the ``Race`` column.

For rows like this:

* Create ``RawResult`` instances and set the ``reporting_level`` to ``county``.
* Set the ``jurisdiction`` field to ``Adair``.

#### Pseudo-candidates

Consider these rows from a precinct-level results file for Iowa's Adair County:

Race | County | Precinct | ROXANNE CONLIN | THOMAS L. FIEGEN | BOB KRAUSE | WRITE-IN | Over Votes | Under Votes | Precinct Totals | Final Data?
---- | ------ | -------- | -------------- | ---------------- | ---------- | -------- | ---------- | ----------- | --------------- | -----------
U.S. SENATOR - DEMOCRATIC PARTY | Adair | 1 NW | 30.0 | 6.0 | 4.0 |  |  | 1.0 | 41.0 | Y
U.S. SENATOR - DEMOCRATIC PARTY | Adair | 2 NE | 24.0 | 4.0 | 4.0 |  |  | 1.0 | 33.0 | Y
U.S. SENATOR - DEMOCRATIC PARTY | Adair | 3 SW | 28.0 | 5.0 | 4.0 |  |  | 1.0 | 38.0 | Y
U.S. SENATOR - DEMOCRATIC PARTY | Adair | 4 SE | 10.0 | 1.0 |  |  |  | 1.0 | 12.0 | Y
U.S. SENATOR - DEMOCRATIC PARTY | Adair | 5 GF | 24.0 | 1.0 | 5.0 |  |  | 3.0 | 33.0 | Y
U.S. SENATOR - DEMOCRATIC PARTY | Adair | ABSENTEE | 16.0 |  | 8.0 |  | 1.0 | 1.0 | 26.0 | Y
Grand Totals |  |  | 132.0 | 17.0 | 25.0 |  | 1.0 | 8.0 | 183.0 |
{: .table-striped }

Candidates names are represented as columns, so for each row, you will create a ``RawResult`` record for each candidate.  There are also columns for over votes, under votes, all write-in candidates and the total votes for all candidates in a county.  Create ``RawResult`` instances for each of these columns and put the column name, (e.g. "WRITE_IN") in the ``full_name`` field.

In your transforms where you create clean ``Candidate`` records, set the ``flags`` field to specify that the candidate isn't a person and instead represents over votes, under votes or votes for all write-in candidates.

#### Vote breakdowns in columns

Consider these rows from a precinct-level results file from Iowa's Lee County:

Precinct | Race | Candidate | Election Day | Absentee | Total Votes
-------- | ---- | --------- | ------------ | -------- | -----------
FM1 | U S SENATOR | Roxanne Conlin | 109.0 | 115.0 | 224.0
FM1 | U S SENATOR | Chuck Grassley | 167.0 | 93.0 | 260.0
FM1 | U S SENATOR | John Heiderscheit | 10.0 | 4.0 | 14.0
FM1 | U S SENATOR | WRITE-IN | 1.0 |  | 1.0
FM1 | U S SENATOR | OVER VOTES |  |  |
FM1 | U S SENATOR | UNDER VOTES | 5.0 | 3.0 | 8.0
{: .table-striped }

Each row has a separate column for election day, absentee and total results.  This makes it straightforward to set the ``vote_breakdowns`` field of the ``RawResult`` record.

Create a single ``RawResult`` record for each row and set the ``votes`` field to ``row['Total Votes']``.

Set the ``vote_breakdowns`` field to a dictionary like this:

{% highlight python %}
{
    'absentee': row['Absentee'],
    'election_day': row['Election Day'],
}
{% endhighlight %}

#### Vote breakdowns in rows

Consider these rows from a precicnt-level results file from Iowa's Union County:
Precinct | Race | Candidate |  | Total Votes
-------- | ---- | --------- |  | -----------
Union  | U.S. Senator  | Roxanne Conlin  | Polling  | 125.0
Union  | U.S. Senator  | Roxanne Conlin  | Absentee  | 47.0
Union  | U.S. Senator  | Roxanne Conlin  | Total  | 172.0
{: .table-striped }

While some result files have the vote breakdowns in separate columns, others, like this example, have a row for each type of vote for a candidate.

While you could implement logic to combine multiple rows into a single ``RawResult`` record with a ``vote_breakdowns`` field, we want to keep our loader logic as simple as possible and have our ``RawResult`` records reflect the data in the source file.  Create one ``RawResult`` record for each of the rows.  For the first row, set the ``votes_type`` field to ``election_day``, for the second, set it to ``absentee`` and for the third, set it to ``None``.

When creating ``Result`` records in a transform, collect the ``RawResult`` records with ``votes_type`` set to ``absentee`` or ``election_day`` and use them to set the ``vote_breakdowns`` field.
