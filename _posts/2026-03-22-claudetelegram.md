---
layout: post
title: "Claude Code and Telegram"
date: 2026-03-22
---
# Testing Telegram/Claude Code Integration

Part of the success of OpenClaw is that you can access it remotely through telegram or whatsapp and communicate with it that way, ask it to run tasks or it can send you scheduled updates.

Claude just added a  a telegram and discord interface for claude code. I installed it and I am testing it out now. This is at an experimental stage at Anthropic, so expect some rough edges...

Installation for Telegram are here: `https://code.claude.com/docs/en/channels`

Follow the instructions on Anthropic's site. Basically you will need to create a telegram account for you. I had one already so I skipped that step. Then a bot needs to be created - this is what Claude will need access to to communicate with you. Create the bot via DM to @botfather. It will step you through the process, in essence selecting a name for it. The URL "https://<name>_bot" will be used later to access it for the first time via a DM. You will be issued a token that you need to copy and supply to claude later during setup.

On the claude code side, there is (undocumented?) dependency. I had to install bun and add bun into the path for my terminal. On my mac this was done via: `curl -fsSL https://bun.sh/install | bash` and it ended up in ~/.bun/bin, so `export PATH=$PATH:~/.bun/bin` (add to the .profile for your shell on my mac) took care of the access to bun. Then a plugin needs to be installed in Claude via `/plugin install telegram@claude-plugins-official`, configured via `/telegram:configure 82....92:AAFBpE....aLcXCYyLxthc`. That string "82...hc" the the token from above from the bot creation procss above. Then claude needs to be restarted with `claude --channels plugin:telegram@claude-plugins-official`.

Now DM the bot on your telegram app on your phone or desktop. Use the `https//t.me/...` to find it. Hit start. That should give you a pair code.
`/telegram:access pair <code>` to configure it in Claude and `/telegram:access policy allowlist` to lock down access.

The process wasn't quite smooth for me. The bIggest issue was missing bun installation. Claude figured it out after about 20 minutes of troubleshooting.

I can now remotely ask Claude to write and test code for me...

Mostly I am using it for observability integration scripts, combining data from AWS, databases and monitoring systems to audit configurations and aid in troubleshooting. The results have been excellent so far.

Quick example: for "continous" auditing we want to show that we control SSH access to the server on AWS at all times. A Claude authored script runs daily and logs findings to  a logging system. Logs are kept for the duration of the audit period and can be used as evidence. We also upload the number of security groups found as a metric into the monitoring system and that cna then be plotted as time series. Auditors like the continous approach. It shows that we invested some thought and care into the system.
