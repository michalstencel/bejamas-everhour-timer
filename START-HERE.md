# Bejamas Everhour Timer — start here

A skill for Claude and Codex that matches your current work in the conversation to existing Bejamas tasks and suggests or automatically switches your Everhour timer. It does not run in the background. It asks when several tasks match; planning and status questions do not switch the timer.

## 1. Add the skill

- **ChatGPT / Claude projects:** extract the ZIP and add SKILL.md to the project's files or knowledge. If your app supports installing skills, you can use that interface instead. Attaching the file to one chat does not configure other conversations.
- **Claude Code:** place the bejamas-everhour-timer folder in ~/.claude/skills/.
- **Codex:** place the folder in a supported skills directory, such as ~/.agents/skills/.

After adding it, start setup with one message: “Use bejamas-everhour-timer and help me set it up. Do not change my timer yet.” On first use, the agent should show a short setup guide. Uploading the file alone does not display a welcome message.

## 2. Connect Everhour MCP

Skip this step if Everhour already works in your client. The official server URL is https://api.everhour.com/mcp — Streamable HTTP with browser-based OAuth sign-in. No API key needs to be copied.

**Claude Code — terminal:**

```sh
claude mcp add --transport http --scope user everhour https://api.everhour.com/mcp
```

Then in Claude Code: /mcp → everhour → Authenticate. Sign in and approve access.

**Codex — terminal:**

```sh
codex mcp add everhour --url https://api.everhour.com/mcp
codex mcp login everhour
```

**Chat apps:** add or enable Everhour as a connector or remote MCP server and authorize the connection. Client-specific details are in SKILL.md; the official guide is at https://developers.everhour.com/mcp-connect. Connections are not necessarily shared between clients.

## 3. Add this to your project instructions once

Use project instructions in ChatGPT/Claude, CLAUDE.md in Claude Code, or AGENTS.md in Codex. Append this text to your existing instructions:

```text
During Bejamas work, use the bejamas-everhour-timer skill. In every new conversation, before the first actual work request, read its SKILL.md from project files or installed skills. Do not wait for me to invoke it; if the file is unavailable, tell me.

I authorize automatic starting and switching of my Everhour timer between verified Bejamas tasks when the conversation clearly indicates that I am beginning or changing the work I am doing now, and one existing task matches unambiguously. Before the first work request, verify Everhour access and report the current timer or a concrete tracking limitation. Check the current timer before making a change and briefly confirm the result. If the correct task is already running, do not restart it.

Ask when several tasks could match. Planning and status questions must not change the timer. Do not modify historical time entries or replace a timer outside Bejamas without my approval for that specific timer. If Everhour is unavailable, tell me. Apply “suggest only” and “do not change the timer” instructions immediately.
```

This removes the need to ask for the skill in every message. Tool access and client permissions must still be enabled.

## Quick test

1. Say: “Check my Everhour account and current timer. Do not change anything.”
2. Then describe real work: “Let's now work on [specific task] in [Bejamas project].” This can actually start or switch your timer. With one clear match, the agent should make the change and confirm it; with several matches, it should ask.
3. Say: “What are we working on in this project tomorrow?” The timer should remain unchanged.

If you use several agents at once, designate one conversation to control the timer. The timer is shared, and the skill does not provide a lock across clients.

## If tracking does not activate in a new chat

Check that the new conversation can read SKILL.md, has Everhour tools available, and receives your project authorization. A file attached in another conversation, a CLI installation, or an enabled connector in another client is not proof of access here. Ask the agent to report which of these checks failed.

All conversations control the same Everhour timer. Sequential handoffs are supported through a fresh live check; simultaneous work requires one designated timer-controlling conversation. Updating this repository does not automatically replace uploaded project files or installed copies.
