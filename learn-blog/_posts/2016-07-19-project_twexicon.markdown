---
layout: post
title:  "Project: Twexicon"
date:   2016-07-19 23:15:00 +0000
---

Well, that was fun.

<h2>Project</h2>
<p>Twexicon is a CLI data gem I built as my capstone project for the Object Oriented Ruby unit. It prompts the user for a Twitter handle; checks that the given handle is an active, public Twitter account; and, assuming all checks pass, catalogs the account's twenty or so most recent tweets in a large, nested hash. While cataloging tweets, Twexicon also parses the text of each tweet with a series of regular expressions, collecting any matched strings into arrays nested within the parent hash.</p>
<p><pre>[14:08:28] ~
// ♥ gem install twexicon
Fetching: twexicon-0.1.7.1.gem (100%)
Successfully installed twexicon-0.1.7.1
1 gem installed

[14:08:33] ~
// ♥ twexicon
Welcome to Twexicon.

Please enter a valid Twitter handle:
realdonaldtrump

Indexed the 20 most recent tweet(s) for @realDonaldTrump.</pre></p>

<p>After the indexing process completes, Twexicon provides a series of prompts to help the user navigate through the culled data.</p>
<p><pre>What would you like to do next? For options, type 'help'.
help

To retrieve one of @realDonaldTrump's 20 tweet(s), type 'GET'.
To view @realDonaldTrump's ten favorite words, type 'WORDS'.
For @realDonaldTrump's favorite things to scream about, type 'SHOUTS'.
To see the #hashtags that consume @realDonaldTrump's thoughts, type 'HASHTAGS'.
For some Twitter handles that @realDonaldTrump has tweeted at, type 'USERNAMES'.
To see links for embedded pictures and/or videos, type 'PIX'.
To view any other links @realDonaldTrump deemed worthy of sharing, type 'LINKS'.
For some numbers that @realDonaldTrump mentioned, type 'NUMBERS'.
To see which short acronyms @realDonaldTrump is fond of, type 'ACRONYMS'.
To look up a different Twitter handle or to exit the program, type 'EXIT'.
Short commands, in order: 'g', 'w', 's', 'ht', 'u', 'p', 'l', 'n', 'a', 'e', and 'h' for 'help'.

What would you like to do next? For options, type 'help'.
get

Which tweet would you like to view?
Please specify a number between 1 and 20.
17

@realDonaldTrump said: .@FoxNews is much better, and far more truthful, than @CNN, which is all negative. Guests are stacked for Crooked Hillary! I don't watch.

What would you like to do next? For options, type 'help'.
words

@realDonaldTrump's current favorite word(s):
04x People
04x Looking
04x Divided
04x Crooked
03x Very
03x Tonight
03x Ratings
03x Hillary
03x Country
03x Bad

What would you like to do next? For options, type 'help'.
shouts

@realDonaldTrump shouted about the following things:
TRUMP
VAST MAJORITY
STILL AHEAD
LAW AND ORDER
TRYING
ISIS

What would you like to do next? For options, type 'help'.
hashtags

@realDonaldTrump ameliorated the lack of Thought Leadership on these topics:
#GOPConvention
#RNCinCLE
#MakeAmericaSafeAgain
#60Minutes

What would you like to do next? For options, type 'help'.
usernames

@realDonaldTrump thinkfluenced the following users:
@RoxaneTancredi
@OreillyFactor
@FoxNews
@CNN
@realDonaldTrump
@60Minutes
@Mike_Pence

What would you like to do next? For options, type 'help'.
pix

@realDonaldTrump shared the following risky clicks:
pic.twitter.com/Kvq6r6WkQ1
pic.twitter.com/iFdqRvaL3q
pic.twitter.com/TzGbDs88b3

What would you like to do next? For options, type 'help'.
links

@realDonaldTrump directed traffic to the following sites:
https://www.facebook.com/DonaldTrump/posts/10157331528640725
https://www.facebook.com/DonaldTrump/posts/10157329790520725
http://cbsn.ws/29QEyKD
https://www.facebook.com/DonaldTrump/posts/10157324504140725

What would you like to do next? For options, type 'help'.
numbers

@realDonaldTrump loves them some digits:
8:30
7
7:00

What would you like to do next? For options, type 'help'.
acronyms

@realDonaldTrump was too lazy to type out the following acronyms:
CBS
P.M.
USA
CNN
V.P.</pre></p>

<p>When the user finishes reviewing the data for one Twitter account, they can either exit or specify a new @username to analyze.</p>
<p><pre>What would you like to do next? For options, type 'help'.
exit
Would you like to look up tweets from another Twitter handle? [Y/N]
y

Please enter a valid Twitter handle:
hillaryclinton

Indexed the 20 most recent tweet(s) for @HillaryClinton.

What would you like to do next? For options, type 'help'.
words

@HillaryClinton's current favorite word(s):
09x Trump
03x Going
02x World
02x Win
02x Wall
02x Trump's
02x Street
02x Small
02x Republicans
02x Chip

What would you like to do next? For options, type 'help'.
exit
Would you like to look up tweets from another Twitter handle? [Y/N]
n
Thank you –– come again!</pre></p>

<h2>Process</h2>
<p>My first few ideas for a project weren't feasible, so it took me a while to get the ball rolling on this one. Finally, I had the idea to scrape and analyze tweets. No one had ever created <a href="https://rubygems.org/search?utf8=%E2%9C%93&query=twitter">a gem for Twitter</a> before, but I set out boldly, breaking new ground left and right.</p>

<h3>Validator</h3>
<p>First, I created <a href="https://github.com/gj/twexicon-cli-gem/blob/master/lib/twexicon/validator.rb">the Validator class</a>, which asks the user to provide a Twitter handle and then checks to see whether it is valid with the following method:</p>
<p><pre>def is_valid?
  username.match(/^\w{1,15}$/) && !TWITTER_RESERVED_WORDS.include?(username.downcase)
end</pre></p>

<p>All Twitter names are comprised of between 1 and 15 Latin alphabet characters, digits, and underscores, which are all conveniently covered by the RegEx word character class (<code>\w</code>). I also had to make sure that Twexicon didn't try to parse certain non-account pages that are housed at <code>https://twitter.com/*</code>, such as <code>/settings</code>, <code>/download</code>, and <code>/privacy</code>. As an added touch, I decided to use Nokogiri to grab the properly formatted @UsErNaMe from the site, so, even if the user enters <code>realdonaldtrump</code>, the <code>@username</code> variable will be stored as <code>realDonaldTrump</code>:</p>
<p><pre>@username = Nokogiri::HTML(open("https://twitter.com/#{username}")).css("span.u-linkComplex-target").first.text</pre></p>

<h3>Scraper</h3>
<p>Next, I worked on <a href="https://github.com/gj/twexicon-cli-gem/blob/master/lib/twexicon/scraper.rb">the Scraper class</a>, which scrapes the HTML from <code>https://twitter.com/#{username}</code> into a large hash that's nested as follows:</p>
<p><pre>tweets {
  1 => {
    "Text of tweet #1" => {
      :pix => [],
      :links => [],
      :hashtags => [],
      :usernames => [],
      :numbers => [],
      :acronyms => [],
      :shouts => [],
      :words => []
    }
  }
  2 => {
    "Text of tweet #2" => {
      ...
    }
  }
}</pre></p>

<p>I created a series of regular expressions to further parse each tweet. Each looks for certain patterns within the tweet text, adds any matches to the correct array within <code>{tweets}</code>, and then removes those matches from the tweet text string. This last step is to ensure that each character in a tweet is only matched once, an easy solution to prevent something like a fragment identifier at the end of a URL (e.g., <code>https://en.wikipedia.org/wiki/Fragment_identifier#Examples</code>) being mistakenly matched as a hashtag (e.g., <code>#Examples</code>). Each tweet is forced to run the following gauntlet:
<ol>
<li>Match embedded pictures and videos: <code>/pic.twitter.com\/\w{10}/</code></li>
<li>Match links that begin with 'http' or 'https': <code>/https?:\/\/[\w\.\?\=\&\-\/\#\:]+/</code></li>
<li>Match word character strings that begin with <code>#</code> (hashtags): <code>/#\w+/</code></li>
<li>Match word character strings that begin with <code>@</code> (usernames): <code>/@\w+/</code></li>
<li>Match one or more digits that might be separated by colons, periods, or spaces (numbers): <code>/(\d+[:\.\ ]?\d*)+/</code></li>
<li>Match two or three capital letters separated by periods or spaces (acronyms): <code>/(\b[A-Z][\.\ ][A-Z][\.\ ][A-Z][\.\ ]|\b[A-Z][\.\ ][A-Z][\.\ ])/</code></li>
<li>Match two or more all-caps strings in a row or one long all-caps string on its own (shouts): <code>/(([A-Z]+[\s\,\&\:\-]+){2,}|[A-Z]{4,}\W)/</code></li>
<li>Match any remaining two- or three-character all-caps strings (acronyms):<code>/\b[A-Z]{2,3}\b/</code></li>
<li>Match any remaining words: <code>/\w+['\/]?\w*/</code></li>
</ol></p>

<h3>Analyzer</h3>
<p>Once <code>{tweets}</code> is fully populated, it's time to turn it over to <a href="https://github.com/gj/twexicon-cli-gem/blob/master/lib/twexicon/analyzer.rb">an Analyzer object</a> in order to allow the user to retrieve data. Analyzer asks until it receives recognizable input, then executes the method that corresponds to the user's command. Most of its retrieval methods have the same basic structure: grab data from the correct arrays nested within <code>{tweets}</code>; if nothing is found, spit out a rejoinder; if one or more strings are found, print them out for the user to read.</p>

<h2>The Biggest Hurdle</h2>
<p>The project progressed rather swimmingly until it came time to build my gem. I attempted to follow <a href="http://guides.rubygems.org/make-your-own-gem/">the RubyGems guide</a> but apparently missed a step somewhere along the way. My gem would function normally when executed within the same directory in which I'd built it, but calling it from anywhere else would result in failure. I suspected that I didn't have my gem's files linked together via the correct <code>require</code> / <code>require_relative</code> statements, but I couldn't figure out the correct combination. So, I did what any sane person would have in that situation: I copied the syntax from a working gem.</p>

<p>My victim of choice was <a href="https://github.com/becky000/sharks_ahead">SharksAhead</a> by <a href="https://github.com/becky000">becky000</a>, who made the mistake of responding to a Slack question I lobbed while knee-deep in the trenches of build failure hell. After reorganizing my project files to mirror the structure of SharksAhead, I tried to build the gem once again. Complete and utter red-texted failure.</p>
<h6><code>uninitialized constant Twexicon</code></h6>
<h5><code>uninitialized constant Twexicon</code></h5>
<h4><code>uninitialized constant Twexicon</code></h4>
<h3><code>uninitialized constant Twexicon</code></h3>
<h2><code>uninitialized constant Twexicon</code></h2>
<h1><code>uninitialized constant Twexicon</code></h1>

![Why must life be so hard?](http://i.imgur.com/SCwXk0k.gif)

<p>For a couple hours, I tried and tried to figure out where the heck <code>Twexicon</code> was supposed to be initialized and why it wasn't properly initializing. I rearranged <code>require</code> statements, read gem creation guides, and tried everything else short of ripping out my motherboard and hard-coding the gem instructions into the soft copper.</p>

![Expand my brain, learning juice!](http://i.imgur.com/VC0tXxp.gif)

<p>And then, like a bolt of lightning, it hit me. I'd become so used to looking at becky000's gem that the fact that my own <code>lib/twexicon/version.rb</code> file looked like this didn't faze me in the slightest:</p>
<p><pre>module SharksAhead
  VERSION = "0.1.2"
end</pre></p>

<p><a href="https://github.com/gj/twexicon-cli-gem/commit/520383c76d662aa89367e7122674552648558d58#diff-ddbb94ba3ad87243697f2680fea70b73">I am an idiot</a>.</p>
