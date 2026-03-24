---
title: "Review: Udacity Data Engineer Nanodegree"
date: 2021-04-15T15:45:34+00:00
slug: "review-udacity-data-engineer"
draft: false
description: "Disappointing. The features of Udacity that I care about aren't present here. Don't waste your money or your time."
cover:
  image: "/images/2021/04/udacity-data.png"
  relative: false
  hiddenInList: false
tags: ["2021", "review", "Data Engineering", "data science", "content", "Nanodegree", "Udacity", "Python"]
---

Tldr; more content, worse support. If you are looking into this Nanodegree to learn data engineering I would strongly advise you to look elsewhere.

# Old content that has not been updated

This is the biggest issue for me. Technology does not stand still, especially for cloud technologies. If you decide to create videos, I minimally expect that the content is kept up to date because videos become impossible to follow if the GUI on the instructor's screen does not match the current state of the GUI. For this reason, I don't like follow-along videos for GUI interactions because it is simply not easy to follow what someone is clicking around in a GUI as compared to coding. The interface for AWS has not changed much, but a few options that were laid out in the video are hidden in dropdown menus now, and it took me awhile to find it.

The content on Udacity was made in 2018. This is not an issue for general concepts such as databases and how they are organized, but links to external content should be updated. I'm sure that the best blog posts on the Internet on databases were not all written concentrated in a short period in 2018.

One particularly egregious example is a link to the Airflow Apache macro. It has not been updated - [https://airflow.apache.org/macros.html](https://airflow.apache.org/macros.html). As of this time of writing, it points to a 404 page. This is a problem because you need to reference this document for the exercise and project. I can find it myself on Google, but if the course is unable to simply update its link to documentation that other people maintain, it is not a good look for the course.

## Content that does not match the videos

Around the Spark course, the content in the video starts to diverge from the instructions.

{{< figure src="/images/2021/04/Screenshot-2021-04-06-at-5.25.01-PM.png" alt="" caption="Instructions for lesson 4" >}}

I have so many questions

- Upload a file: which file? Any file? Any s3 location? This is not answered in the video.
- What are we doing with cities.csv? Note this is the first and last time I heard of this dataset.
- What is <filename>.py? We didn't make anything with Python before this
- What is the s3 bucket for, what are we doing with the EMR instance?

I thought I missed something and skipped back a few lessons, thinking I would find a thread I lost. Nope. Browsing into the Github link provided, there is no Python script or even a template. It felt as if some content was spliced out, and it was very confusing.

After puzzling around for awhile, I searched the forum and found the following:

{{< figure src="/images/2021/04/Screenshot-2021-04-06-at-5.26.21-PM.png" alt="" caption="Below this are complaints of students not wasting hours trying to understand the problem" >}}

If they aren't aligned, shouldn't they be fixed since 9 months ago? Mind you, this is not a free course on YouTube. This is a course that Udacity is charging you \$500 a month for. To put that in context, a course of similar length and quality on Udemy is \$100 for the course.

## Overall, the content was substiantial

I liked that you got to see live coding in the data pipelines section. This is the best way to learn as opposed to watching the presenter run blocks of code or having blocks of code presented to you in a powerpoint because you actually get a sense of how another programmer thinks. Watching can be so instructional because you picking up small hints and tips on how a professional works. Also, it is a a easy way to tell within five minutes of watching someone working if they know their stuff or not.

The content was fairly substiantial. I liked this because I felt that the Robotics Nanodegree was too short. There were plenty of videos explaining concepts and quite some material to work through. There were relatively large number of demos and exercises to reinforce the concepts during the lessons which I appreciated, but this was marred by repetition.

# Repetitive Exercises

The part I enjoyed the least was Lesson 2 was we had to manually enter data into the table using SQL statements. This was busywork since we had already completed the SQL statements from earlier. A better way to approach this would have been to vary the query so that you get to think about how the SQL statements are crafted.

{{< figure src="/images/2021/03/image-4.png" alt="" caption="Repetitive work" >}}

I understand the value of repetition because if implemented correctly it builds recall and speed, but if you are going to spend your time on mindless repetition, you might as well do something else. A better kind of repetition would be to vary the frequency and task to test the same concept in new ways.

Happily enough, the number of repetitive exercises decreases later in the course. I felt this was done pretty well in the Airflow section were the instructor already provided the SQL statements, focusing instead on the aspects specific to getting Airflow running smoothly. I personally liked this section the best.

# Disappointing projects

The projects are a big disappointment. Project A: Do this in Redshift, Project B: Do the exact same thing, but in Spark! And you use the exact same SQL commands for each. In fact, all the projects in the course are using the same database. It gets boring after awhile if you are writing the same SQL statements over and over again.

These projects were not well designed and there was minimal effort put into them to showcase architectures. Despite going over the benefits of various architectures during the lesson, the projects merely ask you to do a very basic, same Extract-Transform-Load process, which I felt was a shame.

One of the reasons I would chose Udacity is being able to learn interesting concepts through creative projects that help to set the stage and tie concepts together. If all I am getting is a Unix environment with some preinstalled packages and a default dataset (Udacity uses Sparkify's dataset), then it is not what I expect. It feels like being served Campbell's canned soup while paying three star Michelin prices.

At least the mentors will be the saving grace right? Oh dear...

# Poor Mentor Help

Unfortunately, the mentors in this course were of a much poorer quality than those from the Robotics Nanodegree. The answers were frequently canned responses with minimal follow-up or were simply not helpful at all.

I present exhibit A: In response to a problem that students faced when following along on the course, specifically as to why an option was not appearing on a dropdown list despite following every step as presented:

![](/images/2021/04/Screenshot-2021-04-06-at-7.57.09-PM.png)

And I quote:

> **Some problems have been left on purpose **

**What?** This was in response to an issue in *following* along on the course. It wasn't even a poorly posed question. "*It was all a test to see if you were paying attention"* is a cringe-worthy response to someone pointing out a mistake you made. It is an unacceptable response to a bug for an online course that you have paid hundreds of dollars for and that has been reported multiple times for a year.

Most mentors regurgitate the same variations "did you create it in the same region" when there is a problem, so often that students have started to preface their questions with statements that they did indeed create their instances and clusters in the same region (even then the mentors still ask).

Other mentors mention remedies for later steps instead of the current one the student is on. When a student tries to engage the mentor to figure out the problem, I rarely see any follow up.

I am very disappointed by the state of the mentors in this program and given that this is actually one of Udacity's unique points, I wouldn't recommend this course.

# Conclusion

The value of this course is about \$40, maybe \$80 if you are feeling generous. Note that this is the amount I would pay for the whole course, not in a month. Note that Udemy courses, which are better quality, cost $20 ~ $40 dollars (some of them go higher undiscounted but I've never seen them undiscounted). And Udemy courses are updated.

Part of my valuation is due to the sheer amount of content it provides. Not all of it is bad and there are plenty of exercises in each lesson. However, poor mentors and the laziness of the project offerings really hold back a lot of the value.

It is definitely not worth \$445 a month. Even a hefty discount of 75% still puts it above what I value it.

If you want to learn about data engineering, you would do well to check out Quiklabs or look into getting a cloud certification from any of the big providers because managing online platforms is a big part of this. At least the tutorials will be up to date and won't be half-baked. If you are a big organization looking for an educational partner, I would advise looking elsewhere for better training.

In conclusion, I'm fairly disappointed with the provided content and support and **I do not recommend** Udacity's Data Engineering Nanodegree.
