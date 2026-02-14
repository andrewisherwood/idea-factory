# Idea Factory Mobile

**Date:** 2026-02-14
**Source:** Claude Code conversation

## Raw Capture

iOS app (or Shortcut as MVP) for capturing ideas into the idea factory repo on the go. Minimum viable version: a text field and a submit button that creates a markdown file in inbox/ and pushes to the repo. No scoping, no evaluation, just capture. The whole point is zero friction — thought to recorded in under 10 seconds. Could start as an iOS Shortcut that commits to GitHub via the API before building anything native. Native version becomes relevant when the factory has enough usage to justify it.

## Initial Gut Check

- **Problem it solves:** Ideas happen away from the desk. Right now capture requires opening Claude Code in a terminal. That's fine at the laptop but useless on a walk, in the kitchen, or at 2am.
- **Who has this problem:** Andy — and specifically Andy's AuDHD brain, which will lose the idea entirely if capture takes more than a few seconds.
- **Why me / why now:** The factory exists now and is actively being used. The iOS Shortcut MVP is a 30-minute build — GitHub API, text input, format as markdown, commit. No reason to wait.
