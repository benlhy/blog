---
title: "Obtaining the OSCP in 2022"
date: 2022-08-15T15:50:41+00:00
slug: "obtaining-the-oscp-in-2022"
draft: false
description: "Reflections on working for and earning the OSCP certificate!"
cover:
  image: "/images/2022/08/oscp_earned.jpg"
  relative: false
  hiddenInList: false
tags: ["offensive security", "2022", "oscp", "passed", "exam", "security", "pentesting", "penetration testing", "course"]
---

On August 2022, I received an email from Offensive Security certifying that I had earned my **Offensive Security Certified Professional certificate**.

> [PEN-200] introduces penetration testing tools and techniques via hands-on experience. PEN-200 trains not only the skills, but also the mindset required to be a successful penetration tester. Students who complete the course and pass the exam earn the coveted Offensive Security Certified Professional (OSCP) certification.

This was one of the most meaningful certificates I have earned to date, and I'm really proud to have earned it. I learned a lot throughout this journey, and I tried to consolidate my thoughts below in hopes it will be useful to others. I think I also offer the unique perspective of going through the course twice over a number of years.

---

I started my OSCP journey in early 2015 when I became interested in 'hacking'. That interest eventually coalesced towards penetration testing, and all my research pointed to one place to get started: Offensive Security.

I liked the name, and I liked the concept even more: test your knowledge against live systems and prove your mastery of the material. Offensive Security heavily emphasizes the practical aspects of penetration testing, and in terms of industry recognition, even in 2015 it was already known to be one of the premier penetration testing certifications that tested practical knowledge over memorization of material.

Armed with this knowledge, and solely this knowledge, I signed up for a 60 day course and was promptly destroyed. It was an unmitigated disaster.

---

## Why I failed the first time

### In retrospect

I jumped into OSCP in 2015 barely knowing any programming languages, a basic understanding of networking, and little to no experience of using a Linux system. I remember struggling to do basic things like copy and pasting from the command line. This extended to basic pentesting techniques like enumeration and exploit modification.

## Lack of familiarity

In 2015, my understanding of how to manage Linux based systems was rudimentary at best, and I barely had any programming knowledge. Penetration testing revolves around spotting things that might be out of place, but because I was unfamiliar with a lot of computing concepts, I wasn't able to tell the difference between what was normal and what was exploitable. I was also struggling to navigate around a Linux system and having to search for instructions on basic operations really slowed me down.

When I attempted the exam again, I had the benefit of knowing C from programming embedded systems, Python from script automation, SQL and Javascript from building small web services, Linux familiarity from using a variety of Linux-based distros... etc. With this background I was able to focus on learning penetration testing techniques.

## No community

When I started, I was the only one I knew who was doing the OSCP and knew nobody who . While there was a online chat through IRC, most of the replies were to try harder. It was challenging to find someone to bounce ideas off on.

Now with Discord and a bigger community for the OSCP, it is easier to find someone to discuss ideas. I also benefitted from knowing OSCP cert holders in person, who advised and encouraged me throughout my learning journey.

## Few alternative resources

In 2015, supplementary resources like Try Hack Me (THM) and Hack The Box were not mature yet, so it was challenging to get additional practice and familiarization on different angles when it came to pentesting.

THM was incredibly useful for preparing me in buffer overflows and being able to work though multiple variations of a problem helped to consolidate my methodology and boosted my confidence. Hack The Box was an additional practice set on top of the labs provided and had the benefit of publicly available writeups that taught me alternative approaches and tools to solving a problem.

## Registering for the course

### PEN-200

# Pre-Labs

I signed up for Hack The Box's (HTB) premium subscription about a month before I started my labs. In retrospect, with the large number of boxes available, I would have been better served starting even earlier.

I started with easy Hack The Box boxes and walkthroughs with Ippsec to understand how I should approach boxes. It was useful to understand why certain tools are used and how to interpret their output. I've seen people say that HTB is very CTF-like and therefore not worth your time, but I think it was a fantastic resource to build familiarity with the tools and techniques that would form the core of my methodology. Think more along the lines of testing out various options in `nmap` and `dirbuster` than actually developing the methodology.

About the time I started my exam, I also registered for Proving Grounds and that provided me with another resource to tap on. I ended up dropping HTB but I kept Proving Grounds until I was done with the exam.

# Labs

I think the most egregious part of the OSCP experience is the learning material and lab exercises. The material is old, and when I compared it against the course material that I purchased in 2015, I was disappointed to find out that there were large portions that are exactly the same and don't work in the modern version of Kali. This was easily the most frustrating and demoralizing part of the course, especially because for the cost and prestige, I expected that the material would have been kept up to date.

I personally opted to do the Lab Report for the bonus points, and it was a trudge. I did find doing it in the last few weeks to be useful because the concepts were fresh in my mind. I did learn new techniques while working through the report that I would otherwise not have explored, but the vague questions often left me wondering if I had included enough detail in my reports. Hopefully I will be the last generation of students that have that option as Offsec is phasing it out in favor of CTF-like challenges in their online portal that are directly related to the content. I did a few of these exercises and I really liked the immediate feedback that they provided. Some sections I learned a lot from were:

- Port Redirection and Tunneling
- Active Directory Attacks
- Client Side Attacks

Sections that I would have avoided:

- Powershell Empire - out of date
- Metasploit Framework - not useful for the exam
- Vulnerability Scanning - the learning objectives were unclear

While there are some new machines in the labs, a lot of the machines are old and have not been updated in awhile. I personally would have liked to see some one-shot machines being updated to introduce more privilege escalation concepts.

I spent around 3-5 hours each day working on OSCP, with 6 hours a day on weekends. I aimed to root at least two machines a day, and if I managed to root two, I could stop early giving me extra motivation when tacking boxes.

I scheduled my exam about one month out, slightly before the end date of my lab access. On exam day, I had approximately 75+ machines completed from the labs, PG Practice, and HTB. That was when I felt ready to tackle the exam, i.e. I didn't want to wait any more to find out how prepared I was. I think it was important to set the exam date so that I had a deadline to work towards.

# Exam

I booked the exam for Thursday, and in the days leading up to the exam, I progressively slowed down the pace which I was doing boxes and started to consolidate my material. I think that was a good strategy for me as I started the exam 'fresh' instead of thinking about the last box I did.

The setup process went smoothly and my advice is to login at least 30 minutes before the scheduled start time to set things up with your proctor. I did not experience any lag or crashes throughout my exam and everything went smoothly until the VPN was closed. I spent the next day writing my report and submitted it. Within 24 hours I was notified that I had passed the exam via email.

I completed the exam with 90 + 10 points and it took me approximately 5 ~ 6 hours to get to the passing grade, and 8 hours to get to 90 points. I spent the remainder of the exam attempting to get the last 10 points but I didn't manage to get it. A lot of people comment that taking breaks is important because it can lead to breakthroughs and that was true for me. I drank plenty of water throughout my exam, which turned out to be great for hydration and for remembering to take breaks.

When I approached the boxes in the labs, I documented them as if they were one of my exam boxes, so taking screenshots and organizing my findings became muscle memory. As I progressed during the exam, I just did whatever I used to do for other boxes, and when I started to prepare my report while the VPN connection was still active, I found that I only had to go back to grab one or two screenshots.

I used my own simple Markdown template that was modelled after how Offsec structured the template report and I don't think I was penalized for it. As long it is properly formatted and professionally written, I don't think the exact format matters much.

---

I spent the next day finishing up the report and the day after that refreshing my email. Even though I managed to get the flags and knew where I stood with regards to the points, I was still really nervous that I had messed up some part of my report and failed the exam. I was only able to finally relax when I received the email from Offensive Security notifying me that I passed!

## Reflections

### Some thoughts about what I experienced

**Active Directory. **I found the Active Directory networks in the labs as well as the sandbox to be great practice for the level of competency in AD that Offsec expects you to have walking into the exam.

**Privilege Escalation. **I found the privilege escalation courses by Tiberius on Udemy to be very comprehensive for expected methods to privilege escalate in OSCP. I regret is not doing it much earlier because then I would have gotten more out of the material. I did it five days before my exam, and I did not have enough time to consolidate and apply the material.

**Other materials. **PG Practice by Offsec was a great resource for training. Most of the machines in the lab are old and don't reflect the current standards. Many times, what should have been a straightforward exploit instead turned into a multi-hour trifecta of searching the forums, faffing around with my Kali installation, and asking other students because of outdated boxes. As the PG Practice boxes are newer, I got to focus on refining my pentesting skills instead of doing things like downgrading python to get an old library in order to run an out-of-date tool.

My recommendation is to focus on the boxes made by Offsec in PG Practice as I found the process for boxes made by them to be more methodical in the way that you enumerate and exploit the box. I thought the community ratings are quite accurate representation in measuring how hard I would find the boxes so I would also recommend working upwords starting from the easier machines as rated by the community.

**Being stuck. **I gave myself anywhere from 1 to 3 hours when getting stuck before resorting to walkthroughs or hints. My reasoning is that I wouldn't have the luxury of spending 12 hours on researching and testing until I found the issue in the exam. If I was stuck, it was either: I missed something, or I didn't know something. For the former, it would serve as a reminder to update my methodology and not skip steps, and the latter, it would just be faster to learn from the walkthrough and update my methodology accordingly. I like the motto of Try Harder, but I think it's also important to Try Smarter.

---

# Conclusion

I really hope that Offsec will update their material for the OSCP as that had the greatest impact on how I viewed the course. There are already positive changes being introduced to the course, and I think it is a matter of time before the preannial complaint about the material is addressed.

A lot of times I read that Offensive Security is very profit motivated and to Pay Harder, but I think to give credit where it is due, the price of the course hasn't gone up since 2015 for the same length of the labs, despite now offering more, such as proctored exams and updated material.

In all, I learned a lot thoughout this journey and earning the certification has been really rewarding, but this is only just the beginning!
