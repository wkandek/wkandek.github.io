---
layout: post
title: "Using codex and other LLMs for coding"
date: 2026-04-05
---
In the last 2 months i have been writing plenty of code with codex and claude. Both have worked really well for me.

My code tends to be operationally oriented.

I wrote a audit program for one of our products that requires weekly setups.. The product is the sum of 6 AWS features that have to be configured and wired together the correct way, plus
two monitoring system that  have checks configured.
Writing or better actually asking codex to write an audit program took me about 1 day of natural language conversation and test run and saves us 
manual troubleshooting work. When a setup fails the program tells us where. When a setup is incomplete but functional the program tells us where.
Codex actually introduced me to "mermaid" diagrams in the process and moved our documentation process forward. We also recently 
adapted it to address the issues found, for example in tagging resources consistently.

We do have a good number of programs running that provide observability information. For the above product I have developed a monitoring program
that collects relevant data and publishes it as a prometheus exporter, which makes it simple to visualize the 50+ data points in Grafana.

We have a developed troubleshooting programs that integrate application performance information, logs and infrastructure information plus core dump analysis.
These have been instrumental in making our applications for reliable and the investment in writing programs and MD files with instructions are well worth the effort

No big system has been developed yet, but over 20 programs have been crafted that support operational duties in capacity planning, monitoring and troubleshooting.
When needed programs persist data locally and write to S3. On startup they check for the existence of new data. This pattern is very helpful for accumulating data by
multiple users and gets used daily in our capacity planning.
  
I have consolidated the programs in a codex skill, that provides instructions and a harness for running the programs, helps with the interpretation a permits natural
language queries: "What are our 5 largest S3 buckets? Show their lifecycles. How are objects in bucket abc distributed age wise?" Very useful.

I am using a codex under a business plan which has 5 hour and weekly credit allotments. I frequently run of of credits. I have been experimenting with the Pi Coding agent wired to
local QWEN instance but results have been worse. It is quite usable performance wise, I run it on a PC with a 5090 graphics  card, but it feels less competent in its execution.
I definitely prefer codex.
