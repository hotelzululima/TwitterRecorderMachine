# Twitter Recorder Machine

The Twitter Recorder Machine, hereafter TRM, is a collection of five dozen small python scripts that are used to support a team of individuals doing large scale monitoring of Twitter.

This code is meant for use on a Linux host and you'll need the following in place to fully exercise it:

* one shell account per Twitter observer account
* one cron job per account every sixty seconds
* system assumes ~/Dropbox is dedicated to TRM duty
* Twitter accounts need secret trm- list for each group to observer
* tw-auth output goes in ~/.twitter

A quad processor VPS with four gig of memory can support sixteen shell account/Twitter account pairs watching over a total of 3,200 accounts. Capacity may be increased with little loss of resolution by dividing accounts into even/odd minute execution groups. The only hard limit on accounts tracked is disk space, but there is a soft limit in that each account will only be accessed  1/(total count of accounts) minutes.

Normal practice involves collectors with specific interests watching less than ten lists with a total of one to two hundred subjects, often overlapping between lists. Analysts typically harvest all the accounts being watched by collectors and they run broader lists of up to five hundred members for background.

Output from TRM comes in a variety of formats which include:

* tweet log file with data, time, tweet ID, and other metadata along with the raw tweet text. Suitable for batch loading into Splunk.
* raw tweet file with all metadata eliminated and URLs removed as well. This is used with the Java Graphical Authorship Attribution Program, a simple cross platform stylometry tool.
* CSV files derived from the tweet log, which contain things like mentions, device usage, and other metadata useful for large scale social network analysis using Gephi.
* XLS files derived from the tweet log, which contain information on account usage time. A comparison of Euclidean distance taken from a 7x24 matrix of tweet activity normalized between 0% and 100% provides a sense of concurrency of use from a pair of accounts.