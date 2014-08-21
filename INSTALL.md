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
*Run tw-auth, place output in ~/.twitter
*Create ~/Dropbox, this is the working space for the scripts.