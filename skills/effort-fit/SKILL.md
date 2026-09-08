---
name: effort-fit
description: Explain or troubleshoot this plugin's effort review and approval workflow when the user asks about Effort Fit.
---

Read ../../README.ja.md for installation, approval syntax, and known limitations.
The synchronous UserPromptSubmit hook performs the review; do not replace it with your own review,
spawn another model, or claim that loading this skill activates enforcement.
Only the effort is reviewed. Preserve the user's execution model.
Do not submit approval commands on the user's behalf or alter/delete pending approval state to unblock work.
If the hook is skipped, tell the user to review and trust it in Codex, then test a new task.
This plugin is a pre-execution guardrail, not a complete policy enforcement boundary.
