# Bejamas Everhour Timer

Conversation-driven Everhour tracking for Claude and Codex. Uses official Everhour MCP and existing Linear-linked Bejamas tasks. Current version: **1.1.0**.

Start with [START-HERE.md](START-HERE.md). Add SKILL.md to each chat project's files, or install the skill folder in your client's supported skills directory. Add the authorization example to project instructions. Connect Everhour separately in each client.

## New conversations

Each new conversation must be able to read the skill and access Everhour tools. It must check live timer state before starting concrete work and report missing setup. No background monitoring or cross-chat lock is provided. Concurrent conversations control one shared timer; designate one controller when working simultaneously.

## Updates

See [CHANGELOG.md](CHANGELOG.md). Versions follow major.minor.patch. Commits preserve changes; numbered releases provide snapshots. Updating GitHub does not automatically update installed skills or project uploads. Replace SKILL.md in every project/client you use, retaining your project authorization. Do not overwrite unrelated project instructions.

Do not commit credentials, personal task IDs, timesheets, or private client data. Qualio and Uploadcare in examples are illustrative, not an allowlist.
