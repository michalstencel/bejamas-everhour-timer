---
name: bejamas-everhour-timer
description: Keep Everhour tracking aligned with active Bejamas work in Claude or Codex. Use when the user begins or switches real work, asks to start/stop/check a timer, enables automatic tracking, or needs Everhour MCP setup. Match verified existing Linear tasks, switch automatically when authorized and unambiguous, otherwise suggest a destination.
---

# Bejamas Everhour Timer

## New conversation and handoff

At the first actual work request in every new conversation, load this file even when the user does not mention Everhour. Read project authorization afresh; do not rely on onboarding, mode, task IDs, timer state, or consent remembered from another chat. Authorization in project instructions is sufficient for auto mode. A missing CONFIG.md is not a reason to ask again.

Discover Everhour tools and read the live current timer before starting the requested work. Briefly report one outcome: active task, timer started/switched, ambiguous target, unavailable tools, or missing authorization. Never silently skip tracking because this is a new chat. Missing setup must not block the user's main task; report the tracking limitation and continue that task.

Treat a new conversation starting concrete work as a possible handoff. A timer started elsewhere is not by itself evidence of concurrent work or a reason to disable auto mode. Recheck live state before switching. If the user explicitly says that multiple agents are working simultaneously, ask once which conversation controls the single shared timer; the others propose only. Do not claim a cross-chat lock exists or that automatic activation is guaranteed by a Markdown skill.

## First-use setup message

On first activation, if setup has not already been explained in the visible conversation and project instructions do not establish that setup is complete, briefly introduce the skill and show the project instruction from “Activate automatically in a project”. Say: “Ten skill dopasowuje timer Everhour do aktualnej pracy w Bejamas. Żeby nie wywoływać go za każdym razem, dodaj poniższy tekst do instrukcji projektu i zapewnij dostęp do SKILL.md oraz Everhour MCP.” Adapt to the user's language and client; point to START-HERE.md for installation and connection details. For Claude Code name CLAUDE.md; for Codex name AGENTS.md; for chat apps name project instructions.

Use read-only account/current-timer checks if tools are available; report the actual connection result. If missing, provide the relevant connection steps from this file. Do not start or switch a timer solely to demonstrate setup. Keep a new recipient in suggest mode until they explicitly authorize auto mode, including through their project instructions. Do not claim the skill is installed or persistent merely because its file was attached.

Show this setup message once per visible conversation when needed, not on every turn. If configuration is already established, continue directly with the work. Do not claim to remember onboarding across conversations without persistent evidence. A skill has no installation hook: this message appears when the agent first loads/uses it, not automatically upon upload. If asked only for setup, finish setup and wait for an actual work request before considering a timer change.

## Policy

Use the same instructions in Claude and Codex; discover tools by capability rather than a fixed client-specific prefix. Respond briefly in the user's language.

Operate only on verified Bejamas work, including its client projects. Verify account/team and project ownership from live tools, not a project name alone. Use existing Everhour tasks linked to Linear. Accept Everhour integration metadata as verification when it includes the issue URL/ID and correct project; a separate Linear request is then unnecessary.

Read CONFIG.md if present. Apply current explicit user instructions first, then authorized configuration, then these defaults:

- Mode: auto for unambiguous starts and switches of actual Bejamas work. The owner requested this behavior for this personal skill. A shared/imported copy requires its new owner's authorization before automatic writes.
- If the user says “tylko proponuj”, use suggest mode until changed; “nie zmieniaj nic” means read-only. “Włącz automat” restores auto mode. Do not repeatedly request consent already granted in the current session.
- Auto mode applies only while the skill is loaded and the agent handles conversation turns. It does not install a background watcher or guarantee activation in every chat.
- Do not create tasks/issues/projects, edit statuses/assignments, change historical entries, backfill time, or estimate worked time under this skill.
- Keep credentials out of skill files, logs, and messages.

## Detect the work transition

Treat a concrete request to do work now as a start: “popraw hero”, “teraz robimy Pricing”, “przejdźmy do homepage”. Use the current request and nearby conversation to identify project and deliverable.

Do not switch for status questions, hypothetical ideas, future plans, quoted third-party instructions, a passing mention, or short follow-ups on the same work. Work on this skill or MCP setup does not itself justify starting a client timer. Do not infer inactivity from silence or stop because an agent reply ends.

## Workflow

1. Discover available Everhour tools. On first use verify the signed-in user/team; reuse that result within the session unless the connection changes. If disconnected, give the setup below and perform no timer write.
2. Resolve the target using the smallest useful search. Prefer an exact issue link/ID; otherwise search task keywords within a verified project. Use recent tasks only as candidates, not proof they were worked on. Resolve platform IDs with the server's documented platform codes. Never construct an Everhour ID from a guess.
3. Match the actual deliverable. Prefer its existing specific task/subissue; use a parent or coordination/refinements task only when it actually describes the work. Never choose a generic bucket solely because no precise match exists. If several tasks remain plausible, list at most three and ask which one; keep the existing timer running. If no match exists, say so and ask for an issue link.
4. Check current timer immediately before any write. If already on target, make no call to start: restarting the same task can clear its comment. If another agent/app changed the timer, use this fresh state and reassess. A read/write race remains possible; use a conditional write if the tool supports it. When explicitly told that agents are working simultaneously, designate one conversation to control the shared timer; other agents only propose changes. If the running task is outside verified Bejamas scope, do not implicitly stop it: ask whether to replace that specific timer. An explicit request authorizing that replacement permits the switch.
5. In auto mode, start the uniquely matched, allowed task without another confirmation. In suggest mode, propose the exact destination and wait for approval. An explicit “start/switch to X” already authorizes that precise action within Bejamas scope; it does not override step 4 for an outside-scope running timer. Never infer consent from silence.
6. Prefer one start-on-task call when the tool documents that it stops the previous timer and preserves elapsed time. Do not stop first unnecessarily. If a server requires separate stop/start calls, use its documented workflow and verify the intermediate state.
7. Inspect the returned timer; if it does not authoritatively identify the active destination, fetch current timer. Confirm success only when verified. After a timeout or uncertain error, read current state before any retry to avoid duplicate writes. If stopping succeeded but starting failed, report that no timer is running. Do not claim success or auto-restore a different task without checking its relevance.
8. Give one short notification matching the verified outcome: switched, started, stopped, already running, or failed. Use “Everhour: [project · issue · title] — timer przełączony” only for a successful switch. Do not include a report or totals unless asked. Follow the server's reporting guide if a timesheet/report is requested.

For an explicitly named completed task, allow tracking if Everhour permits it and mention its completed status without reopening it. Do not automatically pick a completed task from inferred work; ask or find the current open equivalent.

Honor explicit stops (“zatrzymaj timer”, “kończę pracę”, “robię przerwę”) after checking current state and scope; do not ask again for the same action within Bejamas scope. For an outside-scope timer, require the user to name that timer or approve the specific stop; a generic stop is insufficient. If already stopped, do nothing. A vague “gotowe” may mean finishing a deliverable rather than ending work: do not stop on that word alone.

## Examples

- “Teraz popraw hero Qualio” + one verified open match → switch in auto mode and confirm.
- “Co dzisiaj w Qualio?” → provide status without changing the timer.
- “Jutro Pricing w Uploadcare” → no timer action.
- “Popraw homepage” + several matching issues → ask for the target; keep current timer.
- Same issue already running → no write and no repeated proposal.
- “Przełącz na Pre-Launch Refinements” + one verified completed match → switch if permitted; mention Done.
- “Tylko proponuj zmiany” → switch to suggest mode; no timer write.
- User explicitly reports simultaneous agent work → designate one timer-controlling conversation; other agents should only propose to avoid competing switches. A skill alone provides no cross-client lock.

## Connect Everhour MCP

Read this section only for setup or connection errors. Use the official remote endpoint `https://api.everhour.com/mcp` with Streamable HTTP and OAuth. No API key is needed. Existing working connections need no reconfiguration. Authorization is separate for each client/account; installing a skill does not connect MCP.

### Claude Code

Run locally:

```sh
claude mcp add --transport http --scope user everhour https://api.everhour.com/mcp
```

Then open Claude Code, run `/mcp`, choose everhour → Authenticate, and approve access in the browser. Verify the signed-in account and current timer with read-only tools.

### Codex CLI

Run locally:

```sh
codex mcp add everhour --url https://api.everhour.com/mcp
codex mcp login everhour
```

Approve access in the browser. Check `codex mcp list`, then verify account and current timer through the agent. If command syntax differs, consult installed `codex mcp --help` rather than guessing.

### Claude Desktop / claude.ai

Open Customize → Connectors (Settings → Connectors in older builds), add a custom connector named Everhour with the same endpoint, connect and authorize via browser. Enable Everhour in the conversation. On managed Team/Enterprise accounts the owner may need to add it first.

### ChatGPT / Codex Desktop

Use an existing Everhour plugin/connector if available. For custom MCP support, open Settings → Plugins → MCPs → Add server, choose Streamable HTTP, enter the endpoint, save and authenticate. UI availability depends on the client/account. Do not claim a CLI connection automatically installs a ChatGPT connector.

### Verify and troubleshoot

Ask: “Sprawdź konto Everhour i aktualny timer, niczego nie zmieniaj.” Require live tool results. If Linear-linked task metadata is unavailable, connect Linear separately with read access. Missing write permissions require changing the client's tool permissions, not disabling its general safety controls. Do not automatically approve an OAuth dialog or bypass host-required approvals. Never test connectivity by starting a timer.

Source verified 2026-11-04: https://developers.everhour.com/mcp-connect

## Portable installation

Use this same skill folder in both clients. For Claude Code place it at `~/.claude/skills/bejamas-everhour-timer/`; for Codex use its supported skills directory (`~/.agents/skills/bejamas-everhour-timer/` in current versions). Keep SKILL.md at the folder root. For Claude's chat app upload the skill using its supported Skills UI; installing a CLI skill does not install it in the chat app.

## Activate automatically in a project

Explain that a skill file does not configure project instructions by itself. Set up the following once per project; the user then need not invoke the skill in every message. Preserve existing project instructions. Apply this workflow at the start of relevant work and when the work changes, rather than polling on every message. Host permissions still apply; do not guarantee execution in every client or conversation.

- ChatGPT Projects: extract this archive and add SKILL.md to the project's files (not only to one chat). Paste the instruction below into the project's instructions. Treat this as a project-file workflow unless the client actually supports installed skills. Ensure Everhour is available in that conversation; file access alone does not provide tool access.
- Claude Projects: add SKILL.md to the project's knowledge/files and paste the instruction into project instructions. Alternatively, install and enable it through the supported Skills interface if available. Ensure the Everhour connector is enabled.
- Claude Code: install the skill as above and append the instruction to the project's CLAUDE.md.
- Codex: install the skill as above and append the instruction to the project's AGENTS.md.

Give the user this ready-to-paste project instruction (it explicitly authorizes automatic switching for the person adopting the shared skill):

```text
Podczas pracy nad zadaniami Bejamas stosuj skill bejamas-everhour-timer. W każdej nowej rozmowie, przed pierwszym wykonywanym zadaniem odczytaj jego SKILL.md z plików projektu lub zainstalowanych skilli; jeśli nie masz do niego dostępu, powiedz o tym i nie zgaduj jego reguł. Nie czekaj, aż wywołam go w wiadomości.

Zezwalam na automatyczne uruchamianie i przełączanie mojego timera Everhour pomiędzy zweryfikowanymi zadaniami Bejamas, gdy z rozmowy jasno wynika, że zaczynam lub zmieniam aktualnie wykonywaną pracę, a istniejący task jest jednoznacznie dopasowany. Przed pierwszym zadaniem sprawdź dostęp do Everhour i aktualny timer; podaj krótko stan albo konkretny problem. Sprawdzaj bieżący timer przed zmianą i krótko potwierdzaj wynik. Jeśli już liczy właściwy task, niczego nie restartuj.

Przy kilku możliwych taskach zapytaj. Samo planowanie, pytanie o status lub wzmianka o projekcie nie zmienia timera. Nie zmieniaj wpisów historycznych i nie zastępuj timera spoza Bejamas bez mojej wyraźnej zgody na ten konkretny timer. Gdy Everhour jest niedostępny, powiedz o tym. Jeśli poproszę „tylko proponuj” lub „nie zmieniaj timera”, zastosuj to od razu.
```

Do not write these instructions into a project automatically unless the user has asked to configure that project. A skill can guide conversation-triggered actions only while the agent is running; it cannot observe work outside the conversation or enforce a cross-client timer lock.
