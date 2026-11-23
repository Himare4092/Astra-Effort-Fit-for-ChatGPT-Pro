# Astra Effort Fit — ChatGPT Pro

A Codex plugin that reviews the reasoning effort selected for GPT-6 Astra before execution. It asks GPT-5.6 Luna at low effort to review **effort only**. It does not change the selected model or effort.

The Pro policy has no Plus-specific cap. Luna recommends the smallest effort that is sufficient for the task and identifies both over- and under-sized settings.

## How it works

1. Select Astra and an effort in Codex, then send your instruction.
2. The synchronous `UserPromptSubmit` hook pauses the instruction and Luna reports the current effort, recommendation, comparison, and reason.
3. Set the recommended effort in the Codex UI and send the same message again. That second submission is treated as approval without another Luna review.
4. To intentionally use another effort, send the displayed approval command followed by the unchanged original instruction:

```text
effort-fit approve <approval-token> medium
Your original instruction exactly as written
```

Approval is bound to the same task, workspace, model, instruction text, and current effort. It expires after 30 minutes and can be cancelled with `effort-fit cancel`.

## Commands that are skipped

Explicit leading calls to installed Claude, Antigravity (`agy`), or Grok commands are skipped, including forms such as `/claude:review`, `$agy:ask`, `$grok:review`, and skill-selection links. Mentions or quoted examples inside ordinary text are not treated as calls.

## Requirements and installation

- Codex CLI or the Windows ChatGPT Codex app with trusted hooks
- GPT-5.6 Luna access
- Python 3.10 or later (`python` on Windows, `python3` on macOS/Linux)
- Node.js on Windows when Codex is installed through npm

From the personal marketplace:

```text
codex plugin add astra-effort-fit-pro@personal
```

After installation, review and trust this plugin's synchronous `UserPromptSubmit` hook in Codex. Enable only one of the Plus and Pro variants.

## Scope and limitations

The reviewer receives only the task text, current effort, and Pro policy. It cannot inspect your files or execute the task. The review itself consumes Codex usage and stops after 90 seconds without retrying.

This is a pre-execution guardrail, not an absolute enforcement boundary. If hooks are disabled, untrusted, unavailable, or time out at the host level, Codex may continue. In-progress turns, internal continuations, and sub-agent work are outside the same new-prompt guarantee. Transcript metadata is a version-sensitive interface; sessions without it are stopped safely.

Pending approval data is stored locally under `PLUGIN_DATA` and includes the original instruction. Use `effort-fit cancel` to clear a pending review. See [README.ja.md](README.ja.md) for the full Japanese guide.

## License

[GNU General Public License v3.0](LICENSE)
