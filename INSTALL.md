#Installation

The Twitter Recorder Machine has been kept as simple as possible in terms of dependencies. All one needs to install it are a few python libraries and write access to /usr/local/bin. These are four external libraries required:

*tweepy - basic Twitter library

*simpleconfigparser - used to read ~/.twitter credentials

*pyklout - library from Klout for accessing their service

*xlwt - used to read/write XLS files

Once four libraries are loaded the scripts can simply be copied to /usr/local/bin. They do expect to live there and they make explicit full path calls to each others.

#Configuation

Once the scripts are installed you will need to take the following steps:

*Obtain CONSUMER and CONSUMER_SECRET API tokens by registering a Twitter application
*Edit tw-auth to reflect your API tokens.
*Run tw-auth for each account, place output in ~/.twitter
*Create ~/Dropbox, this is the working space for the scripts.
*Manually add Klout API key

Klout API usage isn't very common and the API is forgiving - one account, one API key, a dozen collectors, and we never ran into any sort of limit.

#Cron jobs

Each Twitter account gets a shell account, each shell account gets a cron job that calls N-queue. Memory is the first limit you'll hit, processors are the second. The system has a random sleep of 0 - 30 seconds per each invocation, which serves to smooth the load. N-queue execution time on accounts which are current is on the order of three to five seconds.

If your system starts getting into swap this typically progresses pretty quickly into a full on thrash and then a freeze. This should no longer happen due to concurrent jobs thanks to the queuelock feature but if a system had a large number of shell accounts and two or more analysts created new trm- lists at the same time, it's possible to reproduce this error. The system needs and upgrade, sensing free memory and refusing to run in low memory conditions.

A load average between 1.0 and 2.0 isn't unusual on a dual processor system, more processors obviously help and scale up linearly. Load averages above 1.5 will leave you with a system that keeps running, but for which response via ssh is somewhere between miserable and unusuable.

Collectors generally keep two accounts - one that's fast, for shifty targets who know they are under observation, and one that is thorough. The fast accounts need to run every minute or two, so maybe you split them into even/odd minute execution, maybe you move the thorough accounts to a much slower clock. Maybe the tw-randwait script could be modified to use a period between 60 - 120 seconds, which would introduce the stochastic equivalent of even/odd groups without the configuration complexity.

The htop utility is much more useful than plain ol' top. If you need help with a performance problem install this first so we're using the same measuring stick.

#Dropbox

We started using Dropbox as the file store, but the constant churn made notifications a nuisance. Making Dropbox run from a unix shell account is simple, but if you beat it nonstop dropboxd tends to just give up without warning. Collectors want to see the consolidated file for text search, the mentions CSV for social network analysis, and the XLS files for temporal signatures. The rest of the derived content is used only very occasionally by analysts. We moved to a scheme involving an rsync and dropboxd restart every thirty minutes, then we decided Dropbox was not all that trustworthy, and just stopped using it, but the system still expects to work in ~/Dropbox.

#Splunk

Once the full text search is done with Splunk, collectors are still going to want to see social networks, device usage, and temporal signatures. A trend of such functionality being rendered directly within Splunk rather than duing the collection code execution is something we expect to happen, but not until we have someone around with a more than cursory knowledge of Splunk.

The 500 meg limit for the free version of Splunk is fine, you'd only exceed it during an initial load of a large corpus of data, and since that's a one day event you just get a single over capacity warning. More problematic is the single login security model. Serious service providers will want to purchase the first license, not for the volume, but for the enterprise grade security model.

Splunk should never be exposed to the wild. We wanted to offer the service without revealing the system location, but Splunk will not behave when directly used as a Tor hidden service. Performance is tolerable if accessed via an ssh port forward running over Tor, but that exposes the system location. Maybe the right thing is an sshd that only talks to keys it already knows and which only permits port forwards and not login shells. If Maltego integration is pursued that would be much simpler with keyed ssh logins rather than adding the cost and complexity of a transform server, but again this exposes a tempermental beta grade integration job to curious users.


