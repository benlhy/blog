---
title: "Bypassing MFA with Modern Phishing"
date: 2025-08-10T15:59:19+00:00
slug: "bypassing-mfa-with-modern-phishing"
draft: false
description: "Even with MFA enabled, it is possible to bypass these protections with a man-in-the-middle attack"
cover:
  image: "/images/2025/04/ChatGPT-Image-Apr-9--2025--07_54_34-PM.png"
  relative: false
  hiddenInList: false
tags: ["2025", "MFA", "Phishing", "Evilginx"]
---

The process of red teaming consists of many aspects and after exploring the technicals of malware (although I'm still continuing to explore!), I thought exploring the ingress point would be interesting.

Despite the many protections that are in place for users, phishing still appears to be a highly common source of breaches.

Your standard phishing page attempts to steal the user's credentials by presenting a realistic looking login page that will hopefully capture their credentials. However, the challenge with this site is that it will deviate from the actual site in terms of the layout, and would be quite as sites changes their layouts from time to time. Additionally, with the move towards passwordless signins, tokens, and MFAs, capturing passwords is just one part of the puzzle.

Instead of presenting a user with a likely looking page in order to capture their credentials, you can instead use reverse proxy phishing where an attacker stands in the middle and intercepts whatever requests and replies that are occurring between the victim and the actual site. This makes the phishing site indistinguishable from the actual site to the end user. Because HTTP communication is designed to be stateless, it is challenging for the actual site to tell if the traffic is coming from a legitimate user, or through a malicious proxy. If the phishing site is set up correctly, there is very little indication that something is wrong.

As an experiment, I set up an Evilginx2 server and registered a domain.

{{< figure src="/images/2025/04/image1.jpg" alt="" caption="Nothing to look at here, just a normal Microsoft login page" >}}

Evilginx2 has been optimised over the years and it is really easy to set up and use. To side-step building the binary, I used a prebuilt one from the kali repos when setting up the server.

# Anti-discovery

One thing I noticed was that when I registered a new TLS certificate with LetsEncrypt, there are a bunch of scans that are triggered and logged by Evilginx2. This is due to certificate transparency logging where scanners monitor new registrations of TLS certificates to scan for potentially malicious sites. But this is effectively nullified by the registration of wildcard certificates, which prevents scanners from knowing what subdomain to scan. By registering the certificate for `*.evilphishing.com`, it effectively prevents scanners from determining which subdomain to scan when the certificate is published in the certificate transparency log.

Evilginx2 also adds another layer of anti-discovery by appending a random string of characters to the generated phishing link. Again, this prevents scanners from stumbling on the phishing page accidentally.

To further reduce the likelihood that the domain is scanned, depending on your registrar, there may be options to turn on anti-DDoS traffic filters so that these scanners are presented with a captcha when attempting to access your site for scanning. I found it interesting how tools used for defense can also be turned against the defender itself.

# Increasing Realism

Even if clicked through, most savvy users are taught to look out for URLs and to reject any websites that don't match known URLs. However, with companies having a wide range of URLs (*microsoftonline.com anyone?) *typosquatting similar domains may still be useful.

One thing I noticed is that some SaaS services that provide temporary URLs as part of web hosting are also used as part of the attack chain as they often use familiar words in these temporary URLs. The downside to this is that you'll need to register an account with the provider and may get banned if they find that you are using their services for phishing (as you should).

# Thoughts

Evilginx makes setting up the phishing pathway quite easy. Once I had it set up, clicking on the phishing lure allowed me to log into an MFA-protected account that I had set up for testing smoothly, with no differences from the original login page as Evilginx was just proxying the requests, and on the phishing end, I was able to capture not only the password/username, but also the session token and log into the account.

I think the ease of use and the ease of bypassing MFA really indicates that there is a need for alternative authentication mechanisms. MFA will not save you against a savvy attacker.
