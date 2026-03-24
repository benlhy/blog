---
title: "Review: Udacity Robotics Software Engineer Nanodegree"
date: 2021-03-15T13:09:28+00:00
slug: "udacity-robotics-review-2021"
draft: false
description: "Great for busy professionals, pass otherwise. Low value per dollar, high value overall."
cover:
  image: "/images/2021/03/udacity-logo-1.png"
  relative: false
  hiddenInList: false
tags: ["review", "online", "learning", "Udacity", "Robot", "ROS", "2021"]
---

**Great for busy professionals, pass otherwise.**

Tldr; I enjoyed the novel aspects of the program, but ultimately I thought it fell short of the price it was asking for self-funded learners.

---

I recently had the opportunity to do a Udacity course. Arguably, amongst all the providers I've tried, the Udacity curriculum is most tailored towards a working professional:

- It designed around the assumption that learning is not your full time job. Correspondingly, the content is broken down into small bite-sized chunks that you can start and stop at any time.
- It provides you all the tools you need to complete the program right in your browser. No more figuring out installations and setups!

With that in mind, I set out to finish the Udacity Robotics Software Engineer Nanodegree. Below, I break down my experience of each part of the program, and hopefully, it can help you to decide if Udacity Nanodegree is right for you.

---

# Content

I thought the content was quite well done in the early parts of the course. The lecturer broke down big concepts and I was able to follow along at quite a comfortable pace. The instructions to follow along in ROS were also broken up at regular intervals so that I could always go back if I missed something.

{{< figure src="/images/2021/03/Screenshot-2021-03-15-at-7.21.22-PM.png" alt="" caption="Content" >}}

There were explanations at each line of code and there wasn't fat - content that doesn't need to be there.

However, I felt that the video content was a little paltry; it was even perhaps too concise. The course is broken up into five sections with five major projects. A section might have 3 to 4 lessons. Each lesson has 10 ~ 20 concepts, for those that have videos, the videos can range from 30 seconds to 4 minutes. There were some concepts that did not even have a video at all and instead felt like a transcript was provided from the way the lesson was written. While I read faster than I can listen, it helps to have animated diagrams to explain concepts, especially for the harder ones.

Towards the end of the course they relied more on technical explanations when introducing new concepts. I found these sections difficult to work through because there wasn't a lot of buildup. The instructions for the projects also became more vague and there were a few times I got lost; even the mentors acknowledged that the wording could be improved! I appreciate that it is a way to remember old concepts, but a link to the relevant concept would have been nice.

Overall, content was challenging, but not overwhelming. I did enjoy getting to learn what was provided. Before starting the course, I was familiar with C & C++, and I did robotics courses back in school, but I wasn't familiar with ROS or autonomous navigation. I estimate a 50/50 split between learning how to use ROS (stringing together packages/adapting them), and the ideas behind the algorithms. You get some problems to code in C++, in a nice in-browser environment to get the basics of the concept, and then you get to implement a fully-fledged one in ROS.

Despite some flaws, this is still one of the more comprehensive introductions to ROS with good software support.

# Support

Previously Udacity provided 1-to-1 mentor support, but that has ended. Instead you get a Piazza-like Q&A space where you can ask questions and mentors answer these questions. The variety of responses I’ve seen range from incredibly helpful where the mentor took time to break down where was going wrong in the student's solution to unhelpful snarky remarks of the sort you might find on a forum. Granted, to Udacity's credit, I've seen more positive responses than negative ones, but given that you are paying quite a significant amount for the course, I feel like it should always be of the latter sort.

What I really liked is the project feedback. A real human verifies your work and the feedback I’ve received has always been very helpful, with some reviewers going as far as to provide useful screenshots to show the problem and how I could fix it.

{{< figure src="/images/2021/03/Screenshot-2021-03-15-at-7.09.10-PM.png" alt="" caption="Helpful suggestions from the reviewer!" >}}

{{< figure src="/images/2021/03/Screenshot-2021-03-15-at-7.10.43-PM.png" alt="" caption="Even captured GIFs to show the problem" >}}

This was surprisingly a big plus for me. Having someone to identify what might be wrong with your submission, or even providing encouragement, was quite helpful in motivating me for the next project.

# Workspace

Each time you need to run ROS in the course, you are provided with a workspace. This is a VM that is automatically set up for you when you reach the concept and you can interact with it through a shell/filesystem, or launch the desktop (VNC). A GPU is automatically connected, and the appropriate packages are already loaded into the VM, so there isn’t much configuration you need to do to get it working. The workspace takes about 1 to 3 seconds to launch.

{{< figure src="/images/2021/03/image.png" alt="" caption="Workspace" >}}

The workspace is a big convenience that cannot be overstated. It provides a stable base to revert to and you don’t have to figure out all the dependencies due to the differences between your system’s and the teachers. This allows you to focus on the task at hand: learning ROS, instead of figuring out how best to connect the GPU to a VM.

{{< figure src="/images/2021/03/image-1.png" alt="" caption="ROS in the workspace" >}}

For simulation intensive courses, this is actually quite important because it sets up a GPU workspace to run ROS without assuming any hardware or software on the user's side. This user-dependency can be a bit tricky since users don’t always have the right hardware to run specialised and computationally heavy workloads. ROS is one such workload: it is designed to be run in linux, which most people don’t have installed natively, and it requires a GPU to run at a decent speed. I accidentally launched the workspace without the GPU once, and it subsequently slowed down to 1fps. Given that ROS is quite GUI heavy at the beginning stages of learning, dragging a ball around at 1fps (due to no GPU support) almost broke me down into tears. Getting GPU support in a virtualized workspace working over VNC is no easy task, so kudos to the team at Udacity.

One annoying bit about the workspace is that it is set to automatically close after an hour and you have to click a prompt to continue working. I get that this is a way to conserve valuable GPU time, but if you are looking up some documentation and miss the prompt, the workspace will be shut down. While it is persistent, any files you did not save are lost.

There were also some problems in the workspace that were not fixed. While I understand that because these courses are built on evolving frameworks, things are bound to break sometimes, but I’ve seen workspace problems that prevented progress for multiple students lingering around twenty days or more on the Q&A section. Some of these fixes were quite simple: some students changed a value, or used a configuration file/workspace from earlier in the course, but this should have been covered in the content or fixed by the technical team instead of students sharing their own fixes. Given how central the workspace is to this content, it did not inspire a lot of confidence that these problems are not fixed for long periods of time.

# Cost and Recommendation

The cost is quite significant: for the Robotics Software Engineer course I took, it was \$1876 for 4 months of access. The time is an estimate provided by Udacity at 10 ~ 15 hours of learning a week. Udacity regularly provides discounts from 50% ~ 75%, so the real cost is close to \$200 ~ \$100 a month.

{{< figure src="/images/2021/03/image-2.png" alt="" caption="Udacity discounts" >}}

If the content were just judged on just the teaching material, this would definitely be a hard pass for me, especially considering that there are other offerings out there. I can’t speak for other Nanodegree courses, but where it might just be worth \$100/month is the workspace offering as well as the review portion of the Nanodegree.

If someone else is paying, go for it! If you are paying out of pocket, and you have your heart set on this, I recommend seeing if you can set aside a month and try finishing it within the month. If you are still on the fence, check out other providers like Udemy.
