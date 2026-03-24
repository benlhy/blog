---
title: "Some lessons on writing web-interface scripts"
date: 2018-09-13T10:10:12+00:00
slug: "some-impressions-on-writing-web-interface-scripts"
draft: false
cover:
  image: "/images/2018/09/programming.JPG"
  relative: false
  hiddenInList: false
tag: ["Python", "script", "webserver"]
---

These are some impressions and lessons that I learned from writing a few scripts that access web-interfaces over the last few weeks.

- **Automated login forms are hard. Use cookies.** Not all websites provide their login interfaces with a webpage or an easily accessible form. Some use overlays and this can make it hard to access automatically. Luckily, many of them allow you to *Remember Me* which then allows you to use cookies to access the webpage once you have logged in manually once. For Firefox, access to the profile would give you the cookie, and in Selenium, the code would be:`path = "C:\\Users\\[username]\\AppData\\Roaming\\Mozilla\\Firefox\\Profiles\\[profile]"fox_profile = webdriver.FirefoxProfile(path)``driver = webdriver.Firefox(fox_profile)`This will load the profile that you would normally use with Firefox.
- **Learn your CSS selectors.** This is extremely helpful if you are trying to pick out an element for testing. Some elements are buried under a whole mess of  `<divs>` that make selection via the absolute path really tedious. The best method is to look out for classes and use CSS selectors to pick out the precise tag that you wanted.
- **Installing geckodriver is annoying.**`wget [https://github.com/mozilla/geckodriver/releases/download/v0.18.0/geckodriver-v0.18.0-linux64.tar.gz](https://github.com/mozilla/geckodriver/releases/download/v0.18.0/geckodriver-v0.18.0-linux64.tar.gz)tar -xvzf geckodriver*chmod +x geckodriver`In the .bashrc file:`export PATH=$PATH:/path-to-extracted-file/geckodriver`
