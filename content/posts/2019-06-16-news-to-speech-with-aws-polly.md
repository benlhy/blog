---
title: "News To Speech with AWS Polly"
date: 2019-06-16T04:36:35+00:00
slug: "news-to-speech-with-aws-polly"
draft: false
description: "Convert any digital content to audio for listening with AWS Polly! In this piece we also fix utf-8 formatting issues."
cover:
  image: "/images/2019/06/polly.JPG"
  relative: false
  hiddenInList: false
tag: ["aws", "text to speech", "Polly", "utf-8", "news", "read", "listen", "audiobooks", "decoding"]
---

I love audiobooks. They allow me to enjoy books while allowing me to other tasks. However, audio content is not always available. Small snippets of news or articles are great for listening to as they don't require much attention, but because of their transient nature, they are very rarely available in audio form. This could also benefit those who might find articles too tiring to read because of the screen or other issues that prevents them from reading digital content.

Let's say we have dear Grandma, who we all love and she likes getting news from a particular website, but she's getting on in years and her eyesight isn't doing too well. As her dutiful grandchild, you would love to read these articles to her, but for reasons, you can't do it all the time. So you set up this script that turns hard-to-read text into speech for her, and now your Grandma's life's improved because she doesn't have to struggle with reading the screen and can even do other tasks around the house while she listens to the news.

# How To

- Sign up for an AWS account.
- Set up an S3 bucket, no public access is fine.
- Download/install the aws command line.
- Set up an admin user on the IAM through the AWS Management Console. Get the Access Key ID and Secret Access Key.
- `C:\aws configure` set up the access ID
- Create a text file and use the command: `aws polly start-speech-synthesis-task --region us-east-2 --endpoint-url "https://polly.us-east-2.amazonaws.com/" --output-format mp3 --output-s3-bucket-name [bucket name] --voice-id Joanna --text file://[path-to-file]` Note that `[]` indicates areas where you fill in content yourself.
- `aws s3 cp s3://[bucket name]/[TaskId].mp3 [mp3 name].mp3`

Congratulations! Now you have mp3s that you can play on your phone or any device you want.

# Next Steps

You can write a script that fetches the news or information automatically, convert it, and download to a local device. You could even link it with Alexa and listen to your content, always updated without having to lift a finger.

## UTF-8 Errors

If your text is not properly encoded, and you see the following error:

> Error parsing parameter '--text': Unable to load paramfile (C:\Users\Ben\sm.txt), text contents could not be decoded.'

Use [iconv](http://gnuwin32.sourceforge.net/packages/libiconv.htm) for Windows and use the following command: `iconv -f utf8 -t utf8 -c broken.txt > fixed.txt`. [Online UTF Tools](https://onlineutf8tools.com/validate-utf8) was very useful in checking for errors.

## Bash on Windows

If you use Bash on Windows, you might have some trouble installing aws. It might report that the file does not exist and try to search your C drive despite not having access. Use the following to add the `bin` to the `PATH` in Bash

`export PATH=~/.local/bin:$PATH`
