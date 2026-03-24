---
title: "Cross Origin Resource Sharing Woes (CORS)"
date: 2019-12-07T14:57:34+00:00
slug: "cors-and-how-to-fix"
draft: false
description: "How to fix CORS errors when trying to GET a HTTP endpoint API."
cover:
  image: "/images/2019/12/cors.png"
  relative: false
  hiddenInList: false
tags: ["api", "webserver", "javascript"]
---

Ever come across this situation? You want to pull an HTTP API into service on your website. You test it happily on your local machine with Python or Postman, but then when you convert the code into javascript and try to use it in the browser... boom. You get hit with error messages that look like this:

`preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.`

or

`origin 'null' has been blocked by CORS policy`

CORS exists to stop cross-site request forgeries.

There are a few fixes you can attempt, the first of which is to try setting `dataType: "jsonp"`, this will solve the issue if the server allows requests from anywhere, ie, there is no access control. However if the server does implement access controls and disallows requests, then what you might end up facing is simply a `net::ERR_ABORTED 400 (BAD REQUEST)` error. This is because the browser itself checks the headers and blocks any GET requests that violates the CORS policy.

Unfortunately, this is server dependent. If you are using someone else's API then the workaround would be to use a proxy that forwards your request to the HTTP endpoint, and then responds with the proper CORS headers to the browser.

Luckily for us, some of the hard work has already been done with: [CORS Anywhere](https://github.com/Rob--W/cors-anywhere/). So if you are building a site that uses some external API in the javascript, this might be the best solution.
