---
layout: default
title: Getting Involved
permalink: /getting-involved/
---

## Gathering Metadata

Election data is hard. We need your help to make it easy. And free (as in speech and beer).

The OpenElections Project is a nationwide effort to gather election results for federal and gubernatorial races in all 50 states. Election data is useful, but it's much more interesting when linked to campaign cash, legislation and demographic data. With your help, we'll gather election results and link it to other public data sets so that anyone — journalist, civic hacker, academic, curious citizen — can have access to a robust and useful set of election data.

But first we need the data. That's where you come in.

For each state, we want you to look beyond what's posted on election agency websites and identify the best available data sources. By developing relationships with election agencies and other organizations and experts, you'll help us identify the best sources of results for your state.

Gathering metadata – the data about the data – from those sources sets the stage for the second phase: collecting election results. Whether you're a student, a journalist, a developer or a civic-minded retiree, we want your help with getting the data. To contribute, you can adopt a state, gather some metadata about its elections and submit CSVs that meet our data spec.

### Defining the Important Bits

As much as we'd like to have the most comprehensive election results data in human history, we have to make some decisions on scope. Here is what is important to this project.

OpenElections is focused on official results for federal and most statewide elections (meaning gubernatorial and other state officials), plus state legislative elections where they are easy to obtain. We are not asking for data pertaining to county or municipal races (not yet, at least). The statewide offices we're most interested in include governor, lieutenant governor, attorney general and treasurer, although many states have different offices and we'll take what we can get.

The time frame for OpenElections, at least for now, is from 2000-on. Again, we won't turn away official results from prior to 2000, but the focus is on building up a consistent dataset for the past 12-13 years. While our preference is for official, certified results, if those aren't available we'll accept unofficial results until we can update them.

Finally, we care about all elections that fit the first two criteria, meaning primary, runoff, general and special elections. That's the preamble. Now let's get the data!

### Metadata Gathering

So you adopted a state. Now what? Make a phone call. Maybe several phone calls. Yes, election results are online. But never assume an agency website has the best data available.

There could be a rich, clean database of results lurking behind those spreadsheets posted on the web. Step one is to call the state election agency and ask about their results data. Do they have a results database? Do we need to file a formal records request to get it? Are we stuck with whatever is posted online? Do we have to call county agencies to get the data we want?

We've created a data hub admin site that lets you track conversations with data sources and their offerings. In most cases, you'll start with the official state agency that handles elections.

Once you've identified the best source of data, tell us about it on our admin site.

### The Metadata Process

There are many different kinds of elections, but we want to know the same basic information about all of them. The metadata process begins with an election's date, and includes the types of offices on the ballot, the range of results available and more. We'll go over these in more detail, but here is a brief overview of the information we need.

Elections are mostly regularly-scheduled affairs, so we track them by date. Usually that means many races will be held on the same day, but there are also special elections that can be held throughout the year, and there is a special process for tracking those described below. So let's start with the date of the election.

From there, we'll need some basic information, including relevant URLs from the source, what type of results ("Certified" or "Unofficial" – all results are unofficial until they are certified) and the format(s) that are available.

### Using the Data Hub Admin

If you haven't already received login credentials for the data admin, you should contact the OpenElections team at&nbsp;[openelections@gmail.com][14].

The Data Hub includes several forms for data entry. The primary interface is the States section, which you can click to navigate to the data entry page for your state.

![Data Hub][15]

### State Data Entry Portal

Each state page contains a note for general information about election results in the state. The page also contains sub-forms for entering election metadata and logging the history of correspondence with election officials.

You'll spend most of your time in the Election subforms, which can be added by clicking the "%2B" button to create a new record. Election Metadata

Each election metadata entry form has five sections which will be explained in detail below:

  * Data Source
  * Election Meta
  * Offices Covered
  * Reporting Levels
  * Notes

### Data Sources

![Data Source][16]

The Data Source section allows us to track the source of a set of election results, and provide information about the formats.

**Organization**&nbsp;– Select the source agency. If the organization is not yet in the system, use the Organization pop-up form to enter it's information.

**Portal Link**&nbsp;- Link to the page that contains the direct link to a data set, or possibly to a web form that allows you to request it (may not always be available).

**Direct Links**&nbsp;- Direct URL of the data set, if available (it won't always be).

**Result type**&nbsp;- Is this data set unofficial or certified? The latter represents the final vote tally and often includes counts of provisional and absentee ballots.

**Formats**&nbsp;- Election results come in many file formats, but we have identified a small collection that are most commonly used. These range from HTML (usually in tables) to CSV (comma separated values) to PDF, with some others. In some cases, it may be necessary to open the file and assess the format. If the state you are working on has a format that is not present in the admin, let us know and we'll figure out whether to add a new record or to place it within an existing format.

### Election Meta

![Election Meta][17]

The Election Meta section tracks general information about races on a particular election date.&nbsp;**For a given state, we want to create a separate Election record for every unique combination of election date, race type, and special election status.**

For example, let's say an election date in November included the following races:

  * a US Senate and several US House elections on their normal election dates
  * two runoffs for U.S. House
  * a special general election to fill a vacant state senate seat
  * a recall election for governor

To capture the above in our data admin, you would create four separate Election entries: One for the general, one for the U.S. House runoffs, one for the special election, and a final entry for the gubernatorial recall. You would then select the appropriate offices in the Offices Covered section (see below).

Below are more details on each field in this section.

**Start date**&nbsp;- The first date of the election. Most U.S. elections take place on a single day, so normally you'll only fill in this field (and not "end date").

**End date**&nbsp;- Some elections, such as the Wyoming and Maine presidential primaries, span multiple days. In such cases, enter the end date of the election. Otherwise leave it blank.

**Race type**&nbsp;- Select the relevant race type (Primary, General, Primary Runoff, etc.)

**Special**&nbsp;- Check this box if the race is a special election. A special election is one that takes place outside of the normally scheduled election cycle for a given office. Think of it as a modifier of the options in&nbsp;**race type**.

**Primary type** – If it's a primary, select whether it's _open_, _closed_, _semi-closed_, _semi-open_, _blanket_, or _other_. _Other_ should be used for edge cases such as states where parties have different primary types. See below note on primaries for more details.

**Primary note&nbsp;**\- Explain any idiosyncrasies about primaries here. If you select&nbsp;_Other_ for the&nbsp;**primary type** field, you must supply a note explaining the edge case. For example, Kansas and a number of other states have _semi-closed_ primaries.

**Absentee and Provisional**&nbsp;- Check this if the results include tallies for absentee and provisional ballots.

#### A Note on Primaries

Most primary elections are closed, meaning that voters can only vote in
partisan primaries for the party with which they're registered. A number
of states, however, have open, semi-open, semi-closed, or blanket primaries (see [Wikipedia for a good write-up on the differences).][18]

While the type of primary is rarely indicated in results data, this information is valuable to OpenElections because it will help us validate results down the road. Some states change primary types over time, such as California, which switched to an open primary system in 2012. In order to verify primary type over time, volunteers should check with state election officials to get a sense of the primary system's history. This is a great question to ask during the [initial data interview][19] with state officials.

### Offices Covered

![Offices Meta][20]

This section is used to note which offices had races on a given election date. A data set for general and primary elections typically includes results for numerous offices, whereas special elections usually involve a single office.

The Florida 2012 general election, for example, included races for all categories but Governor, whereas the state also had a number of special elections for state legislative seats in 2011.

The general election would involve creating a single Election entry and flagging all the offices except for Governor. The special elections would each require a separate Election entry with only the State Legislative box checked.

NOTE: The OpenElections project is primarily focused on results for federal and major state-level offices. Therefore, this section does NOT include ways to track referendums or county and municipal races. We may integrate support for these election types in the future.

### Results Breakdowns

![Results Meta][21]

This section allows us to track the levels at which results are broken down.

Election agencies often provide a mix of breakdowns. The common case is "racewide" results, which implies different geographic areas depending on the type of race. Racewide results imply a statewide tally for offices such as U.S. Senate or Governor, whereas the "racewide" category implies district-wide totals for a U.S. House race. Racewide simply means the highest level of results aggregation for each race.

Agencies also often provide results at lower levels of tabulation, such as county- and precinct-level breakdowns.

These reporting levels are the primary tabulations we're interested in tracking. One or more of these boxes must always be checked for a given result set.

The second row of tabulation levels — Congressional District and State Legislaive — should only be flagged when there are result breakdowns at those levels for unrelated offices. In other words, flag the Congressional District box if there are results for the presidential race at the congressional district level. Do NOT check the box to denote results for a U.S. House race (these are covered by the "Racewide" checkbox).

### Notes

![Notes Meta][22]

**Notes** General notes about a particular data set that would be useful to know. These would include quirks about the data that may not be otherwise tracked by the data admin (e.g., the fact that D.C. has ward-level vote breakdowns) or differences in what is included at various reporting levels (Maryland, for instance, does not include absentee or provisional tallies in precinct-level results).

**Needs Review&nbsp;** Explanation of possible data issues and how to follow up.

### FOIA Logs

![FOIA Log][23]

The FOIA contact logs should be used to track the history of important conversations with contacts at data source agencies. The types of things you should track in this section include:

  * Initial phone call to an agency to inquire if they have a database of election results
  * Detailed conversations about the technical aspects of the results system (extended conversations can be recorded in a Google Doc and linked to from the FOIA Log record – be sure to make the Google Doc accessible to anyone who has the link).
  * Formal public records requests – please keep copies of any records requests submitted.



   [1]: http://openelections.files.wordpress.com/2013/02/bannerlogofinal.jpg
   [2]: http://blog.openelections.net/
   [3]: http://blog.openelections.net/19-2/ (About)
   [4]: http://blog.openelections.net/people/ (People)
   [5]: http://blog.openelections.net/get-involved/ (Get Involved)
   [6]: http://blog.openelections.net/faqs/ (FAQs)
   [7]: https://github.com/openelections (View on Github)
   [8]: http://blog.openelections.net/openelections-glossary/ (Glossary)
   [9]: http://s0.wp.com/wp-content/themes/premium/standard/images/social/small/twitter.png?m=1351293018g
   [10]: http://s0.wp.com/wp-content/themes/premium/standard/images/social/small/vimeo.png?m=1351293018g
   [11]: http://s0.wp.com/wp-content/themes/premium/standard/images/social/small/email.png?m=1354256288g
   [12]: http://s0.wp.com/wp-content/themes/premium/standard/images/social/small/rss.png?m=1351293018g
   [13]: http://blog.openelections.net
   [14]: mailto:openelections%40gmail.com
   [15]: https://s3.amazonaws.com/openelex-static/docs/admin_main.png
   [16]: https://s3.amazonaws.com/openelex-static/docs/data_source2.png
   [17]: https://s3.amazonaws.com/openelex-static/docs/election_meta2.png
   [18]: http://en.wikipedia.org/wiki/Primary_election
   [19]: https://docs.google.com/document/d/1nt6A0Ek1n3IAXyHQcYT0Q0JRejD43ZsfnH7_9SVN8V4/pub
   [20]: https://s3.amazonaws.com/openelex-static/docs/offices_meta.png
   [21]: https://s3.amazonaws.com/openelex-static/docs/results_meta.png
   [22]: https://s3.amazonaws.com/openelex-static/docs/notes_meta2.png
   [23]: https://s3.amazonaws.com/openelex-static/docs/foia_log.png
