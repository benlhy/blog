---
title: "How to Remove/Exclude ANY SITE from your Google Searches PERMANENTLY"
date: 2018-08-13T01:23:31+00:00
slug: "how-to-remove-exclude-any-site-from-your-google-searches-permanently"
draft: false
description: "Remove/Exclude Any Site (Pinterest/Research Gate) from your Google search permanently without any addons!"
cover:
  image: "/images/2018/08/pin2.png"
  relative: false
  hiddenInList: false
tag: ["Google", "Images", "search", "without", "permanent", "remove", "Pinterest", "automatic", "exclude", "Researchgate", "Chegg", "no install", "no addon"]
---

*Edit: A typo corrected by a very kind suggestion from Adam Flynn.*

This method requires no installation of any addons nor does it require you to prepend any operators manually each time you search. **This method modifies the built-in search bar of Chrome** and automatically prepends a search operator that removes any domains associated with Pinterest, Chegg, Quora or any website that you want to remove from your searches.

This can be very useful if you are searching for information but the top results are choked with websites that require a subscription to use. It is also more convenient than prepending an operator each time you want to perform a search, especially if there are multiple websites you want to exclude from your search each time.

## How to do it

In Chrome, go to `Settings>Manage Search Engines`. Click on the default Google search, copy the field of *URL with %s in place of query*.

![editsearch](/images/2018/10/editsearch.JPG)

If it is greyed out, you can still double click on the field to highlight the information, and use Ctrl-C to copy the field. Exit out and create a new entry using the add button.

![addbutton](/images/2018/10/addbutton.JPG)

I called mine **Google Without Pinterest**. Paste your copied information into the same field and add `+-site:pinterest.*+` after `?q=`. The field should look like this:

`{google:baseURL}search?q=+-site:pinterest.*+%s&{google:RLZ}{google:originalQueryForSuggestion}{google:assistedQueryStats}{google:searchFieldtrialParameter}{google:iOSSearchLanguage}{google:searchClient}{google:sourceId}{google:contextualSearchVersion}ie={inputEncoding}`

For every site you want to exclude from your google search bar, add in `+-site:**site_name**+`. The `*` operator after pinterest removes all associated domains from the search, so not only .com is blocked, but also .net, .org, .io.

![search_engine](/images/2018/10/search_engine.JPG)

Exit out and **set it as the default search**. Now every search you make will be devoid of Pinterest or any other website you chose to block. This result takes you to the actual websites hosting the content instead of Pinterest.

## Rationale

I understand the utility of Pinterest. It is an extremely easy way for users to collect and group ideas. However, my issue is with the aggressive signup policy of Pinterest. Pinterest has a signup page that blocks any further navigation.

![pin1](/images/2018/08/pin1.JPG)

Now this would be fine if Pinterest and I never crossed paths. The problem is that Pinterest occupies a lot of Google Image options, and when you click through, it the offending signup page pops up again. As someone who does not use the site, this is very frustrating.

![ami](/images/2018/08/ami.JPG)

It is disruptive as it is another layer that I have to go through before I get to the page I want with the content.

Clicking on the images is also non-intuitive as it brings you to other images instead of the page hosting the actual content. The actual link is below the image.

![clicking](/images/2018/08/clicking.JPG)

Sometimes it leads back to Google Images. This leaves you in a bind because clicking you will lead to the google page, which points to the Pinterest, pointing to Google... you get the idea.

![google](/images/2018/08/google.JPG)

There are some fixes to this problem. One which includes the option of appending a operator to the Google Search: `-site:pinterest.*`. I did not want to do this every time I searched, so instead I modified my default search engine to automatically prepend the operator to remove the search result permanently from my Google searches. This even works with Google Images. This is the least disruptive and most automated way of removing Pinterest search results without installing any addons or software.
