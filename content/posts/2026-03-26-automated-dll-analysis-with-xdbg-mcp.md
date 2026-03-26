---
title: Automated DLL Analysis with xdbg MCP
date: 2026-03-24T16:16:07+08:00
draft: true
tag: []
---

I got interested in agentic capabilities recently, and while I've found that LLMs are really good at generating content: be it text or code, or summarisation, but when it comes to processes, sometimes a cheaper way is to have it write a script rather than execute the same actions over and over again and costing tokens.

# The Task
The task is simple: given a program, analyse if it has potential for sideloading DLLs. Sideloaded DLLs are a common tactic used to evade 