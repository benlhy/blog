---
title: "My experience taking the Google  Associate Cloud Engineer exam"
date: 2021-06-02T13:52:36+00:00
slug: "google-cloud-associate-cloud-engineer-notes"
draft: false
description: "How I prepared and passed the Google Cloud Associate Cloud Engineer exam. What I learned, what tools I used, and some tips when it comes to answering questions"
cover:
  image: "/images/2021/06/gcp-1.png"
  relative: false
  hiddenInList: false
tag: ["GCP", "Google", "2021", "cloud", "Associate Cloud Engineer"]
---

I recently passed my Google Cloud ACE examination!

{{< figure src="/images/2021/05/image.png" alt="" caption="Yay!" >}}

The exam itself was very challenging and it certainly pushed my cloud knowledge as well as my understanding of networking to the limit. It can be quite nitpicky, all the way down to four similar looking commands with minor differences in order to selecting a suite of services based on a given scenario.

I used the Official ACE examination study guide as well as Qwiklabs. I thought Qwiklabs were the most useful in terms of getting me familiar with Google Cloud in a risk free environment since they create a custom lab each time you start a lesson. You can get a free month by taking part in their challenges each month here: [https://inthecloud.withgoogle.com/google-cloud-skills/register.html](https://inthecloud.withgoogle.com/google-cloud-skills/register.html)

Complete the first lab with the complementary credits and you will be automatically assigned a free month. Do note that you need to enroll in the code first before signing into Qwiklabs in order for the free month to apply.

I won't go through the content of the ACE examination because Google provides an adaquate summary of the topics that you need to know. Instead, I will provide some intuition with how the questions are structured that I got after quite a bit practice and review.

# Top Ten Intuitions

- Never write anything custom.
- Never write a custom script. There is almost no scenario where a custom script is necessary to do something in Google Cloud. Metrics = Cloud Logging. Messaging =PubSub
- Anything that requires delays  = PubSub
- Short snippet = Cloud Functions
- Cost is mentioned = Preemptible instances/nodes
- IAM and people = always group them, then apply permissions
- There can only be one App Engine in a project
- Understand the layers for Kubernetes as well as App Engine. Kubernetes - Pods, Deployments, Services. App Engine - Instance, Version, Service, Application
- Load balancers have only one ip address
- Naming convention of compute and IAM permissions

Onward to the Professional Cloud Architect!
