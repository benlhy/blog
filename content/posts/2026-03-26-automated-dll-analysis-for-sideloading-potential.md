---
title: Automated DLL Analysis for Sideloading potential
date: 2026-03-24T16:16:07+08:00
draft: true
tag: []
---

I got interested in agentic capabilities recently, and while I've found that LLMs are really good at generating content: be it text or code, or summarisation, but when it comes to processes, sometimes a cheaper way is to have it write a script rather than execute the same actions over and over again and costing tokens.

# The Task
The task is simple: given a program, analyse if it has potential for sideloading DLLs. Sideloaded DLLs is a common tactic used to deliver malware. As a signed binary is executed, it has a lower signature than an unsigned binary, and the malware is better able to evade detection.

There are a few steps in the discovery process:
1. Run process explorer with a preset filter to discover any LoadLibrary calls to a DLL in the same folder as the executable. This is often seen as an OpenFile or CreateFile call in `procexp`.
2. Gather a list of DLLs that it is trying to call.
3. Open xdbg with the executable as the target.
4. Set a breakpoint on the LoadLibrary call. 
5. Run the debug process until a DLL in the prior list is loaded.
6. List the exported symbols so that we can create a proxy with the right parameters.

