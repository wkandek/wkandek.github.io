---
layout: post
title: "Mac Notfifier for Claude Cowork"
date: 2026-04-27
---
# Claude Mac Notification Setup Guide

Recently I need to implement Notification on my Mac for Claude Cowork schedueld tasks.
That was quite involved and a many shot problem. 

This is a quick guide on how to reproduce the Claude Cowork → terminal-notifier notification system on any Mac.

---

### 1. Prerequisites

Install Homebrew
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Install terminal-notifier
```bash
brew install terminal-notifier
```
Verify it works:
```bash
terminal-notifier -message "hello" -title "test"
```
A banner should appear.

Allow notifications in macOS System Settings  
System Settings → Notifications → terminal-notifier → set to **Banners** or **Alerts**, enable sounds.

---

### 2. Install the handler script

Create the notification drop folder and processed log
```bash
mkdir -p ~/.claude-notifications/.done
touch ~/.claude-notifications/.processed
```

Create `/usr/local/bin/claude-notify-handler`

The script does two things:
1. Checks `~/.claude-notifications/` for manually dropped files
2. Scans all Claude Cowork session output folders for `*.notification` files

```bash
sudo tee /usr/local/bin/claude-notify-handler <<'EOF'
#!/usr/bin/env bash
NOTIFIER="/opt/homebrew/bin/terminal-notifier"
[ ! -f "$NOTIFIER" ] && NOTIFIER="/usr/local/bin/terminal-notifier"
SESSION_BASE="$HOME/Library/Application Support/Claude/local-agent-mode-sessions"
NOTIFY_DIR="$HOME/.claude-notifications"
DONE_DIR="$NOTIFY_DIR/.done"
PROCESSED_LOG="$NOTIFY_DIR/.processed"
mkdir -p "$DONE_DIR"; touch "$PROCESSED_LOG"

fire() {
  "$NOTIFIER" -title "$1" -message "$2" -sound default -group "claude-task" \
    -activate "com.anthropic.claudefordesktop" 2>/dev/null || \
  "$NOTIFIER" -title "$1" -message "$2" -sound default -group "claude-task"
}

for f in "$NOTIFY_DIR"/*.notification; do
  [ -f "$f" ] || continue
  TITLE=$(sed -n '1p' "$f"); MESSAGE=$(sed -n '2p' "$f")
  [ -z "$TITLE" ] && TITLE="Claude Task"
  [ -z "$MESSAGE" ] && MESSAGE="A scheduled task completed."
  fire "$TITLE" "$MESSAGE"
  mv "$f" "$DONE_DIR/$(date +%Y%m%d-%H%M%S)-$(basename "$f")"
done

while IFS= read -r -d '' f; do
  grep -qxF "$f" "$PROCESSED_LOG" && continue
  TITLE=$(sed -n '1p' "$f"); MESSAGE=$(sed -n '2p' "$f")
  [ -z "$TITLE" ] && TITLE="Claude Task"
  [ -z "$MESSAGE" ] && MESSAGE="A scheduled task completed."
  fire "$TITLE" "$MESSAGE"
  echo "$f" >> "$PROCESSED_LOG"
done < <(find "$SESSION_BASE" -name "*.notification" -print0 2>/dev/null)
EOF
sudo chmod 755 /usr/local/bin/claude-notify-handler
```

---

### 3. Install the launchd agent

Create `~/Library/LaunchAgents/com.claude.tasknotify.plist`

Runs the handler every 60 seconds and once at login.

```bash
cat > ~/Library/LaunchAgents/com.claude.tasknotify.plist <<'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
  "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key><string>com.claude.tasknotify</string>
  <key>ProgramArguments</key>
  <array>
    <string>/bin/bash</string>
    <string>/usr/local/bin/claude-notify-handler</string>
  </array>
  <key>StartInterval</key><integer>60</integer>
  <key>RunAtLoad</key><true/>
  <key>StandardOutPath</key><string>/tmp/claude-tasknotify.log</string>
  <key>StandardErrorPath</key><string>/tmp/claude-tasknotify.err</string>
</dict>
</plist>
EOF
```

Load the agent
```bash
launchctl load -w ~/Library/LaunchAgents/com.claude.tasknotify.plist
```

> macOS will show a "Background Items Added" alert — this is normal, dismiss it.

---

### 4. Wire up a scheduled task

Add this instruction to the end of any Cowork scheduled task prompt:

> Use the Write tool to create a file named `[unix_timestamp]-[taskname].notification` in the current outputs directory with two lines:
> - Line 1: Task name
> - Line 2: One-line result summary

The handler finds the file within 60 seconds and fires terminal-notifier.

---

### 5. Verify & debug

Quick test — drop a file manually
```bash
printf 'Test Task\nNotification system is working.' \
  > ~/.claude-notifications/test.notification
```
A banner should appear within 60 seconds.

Check agent status and errors
```bash
launchctl list | grep claude.tasknotify
cat /tmp/claude-tasknotify.err
```
Exit code `0` = healthy. If you see "Permission denied":
```bash
sudo chmod 755 /usr/local/bin/claude-notify-handler
```

---

### Key insight

Cowork scheduled tasks run in isolated sessions with no persistent folder access. The handler scans the Claude session output folders (`~/Library/Application Support/Claude/local-agent-mode-sessions/`) rather than watching a fixed path. The 60-second `StartInterval` poll works regardless of where the task writes the `.notification` file.

---

### Known pitfalls (hard-won)

#### 1. Handler script permissions — "Permission denied"
`sudo cp` preserves the source file's permissions. If the source script was `rwx--x--x` (execute-only for non-root), launchd can execute it but `/bin/bash` can't *read* it, causing a silent failure. Always run:
```bash
sudo chmod 755 /usr/local/bin/claude-notify-handler
```
Check with `cat /tmp/claude-tasknotify.err` — "Permission denied" on the handler path is the telltale sign.

#### 2. WatchPaths doesn't work here — use StartInterval instead
`WatchPaths` in a launchd plist only fires when a *specific known path* changes. Cowork task sessions write to a new UUID-named folder every run, so no fixed path can be watched. `StartInterval: 60` polling + a `.processed` log to avoid double-firing is the only reliable approach.

#### 3. Scheduled tasks cannot write to your connected folders
Even if you've connected `~/Documents/` or `~/Downloads/` as a Cowork workspace folder in a chat session, scheduled task sessions are isolated — they have no folder access and fall back to writing to their own session outputs folder. This is why the handler scans `~/Library/Application Support/Claude/local-agent-mode-sessions/` rather than a predictable path. Do not try to solve this with `request_cowork_directory` in the task prompt — that requires human approval and will hang an unattended run.

#### 4. TCC blocks bash from accessing ~/Documents and ~/Desktop
macOS Transparency, Consent & Control (TCC) prevents LaunchAgent-spawned `/bin/bash` scripts from reading or writing `~/Documents`, `~/Desktop`, and `~/Downloads` without Full Disk Access. If you try to have the handler read report files from those locations, it will silently fail. Keep all handler state (`.processed` log, `.done` archive) in `~/.claude-notifications/` which is not TCC-protected.

#### 5. "Background Items Added" alert from macOS
When the launchd agent is loaded, macOS shows a system alert saying bash was added as a background item. This is expected — dismiss it. You can review background items at System Settings → General → Login Items & Extensions.
