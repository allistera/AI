# Agent Instructions

These instructions apply to Codex, Claude Code, OpenCode, and other AI coding agents working in this repository.

## Working principles

- Before planning or making changes, check for repository-provided or installed skills, agent instructions, templates, and automation that could help with the task. Read and follow any relevant guidance.
- Read the relevant code, configuration, and documentation before changing anything.
- Keep changes focused on the requested outcome. Do not refactor unrelated code.
- Prefer clear, maintainable, secure, and production-ready solutions over clever shortcuts.
- Apply KISS (Keep It Simple, Stupid): choose the simplest solution that fully satisfies the current requirement.
- Apply YAGNI (You Aren't Gonna Need It): do not add speculative features, abstractions, configuration, or dependencies for possible future needs.
- Preserve existing conventions unless there is a strong reason to improve them.
- Do not expose, log, commit, or store credentials, tokens, private keys, or other secrets.

## Shared memory with Mem0

- Use Mem0 as the shared memory system across agents.
- At the beginning of work, retrieve relevant project context, decisions, conventions, and prior hand-off notes from Mem0 when it is available.
- At the end of meaningful work, record durable, useful context in Mem0: architectural decisions, non-obvious conventions, completed changes, unresolved issues, and recommended next steps.
- Keep memories concise, factual, and scoped to the project. Do not save secrets, personal data, or transient debugging noise.
- Treat the repository as the source of truth when Mem0 conflicts with the current code or documentation; update the shared memory to reflect the resolved state.

## Quality and verification

- Linting is mandatory. Run the repository's lint command before declaring work complete, and fix lint failures rather than suppressing or bypassing them.
- Run relevant tests, type checks, builds, and formatting checks when available and proportionate to the change.
- Add or update tests when behaviour changes, unless there is a clear documented reason not to.
- Report the commands run and any verification that could not be completed.

## Tooling and dependencies

- Prefer modern, actively maintained tools and libraries when they provide a clear benefit for the task.
- Consider promising up-and-coming tools where appropriate, but assess maturity, maintenance activity, security, compatibility, operational support, and vendor lock-in before adopting them.
- Do not introduce new dependencies or tools without a clear need. Prefer the repository's established stack where it already meets the requirement.
- Pin versions and document notable tooling decisions when adding a new dependency or workflow.
- Before applying a major dependency update, review its official release notes and migration guide. State the exact version change and summarise the relevant new features, breaking changes, deprecated behaviour, and required migration actions.
- Do not apply a major dependency update until its impact on the repository's code, configuration, tests, build pipeline, and runtime environment has been assessed.

## Version control

- Use Jujutsu (`jj`) as the primary version-control interface when it is available. Repositories remain Git-compatible; use Git directly only when `jj` cannot perform the required operation or an external workflow explicitly requires Git commands.
- Inspect the working-copy status before changing files, keep commits focused and reviewable, and do not overwrite or discard unrelated work.
## Completion

- Update documentation, configuration examples, and shared memory when the change affects how the project is used or operated.
- Summarise what changed, why, and how it was verified. Call out assumptions, risks, and follow-up work explicitly.

## Agent communication

- Output clean, human-readable text by default. Keep responses concise, well structured, and free from unnecessary noise.
- Do not expose raw tool output, internal reasoning, debug logs, stack traces, or implementation details unless the user explicitly asks for them or they are needed to explain a failure.


## Shared Memory with Mem0

Use Mem0 as the shared persistent memory layer for all supported agents.


## Mem0 scope

For every Mem0 MCP read or write, pass `user_id: "allister"`.
For a direct call, use:
{ "text": "…", "user_id": "allister" }

The MEM0_USER_ID=allister environment variable applies to the Mem0 CLI, not the hosted MCP connector. Mem0’s hosted
MCP setup and tool list do not document a configurable default user ID. Mem0 MCP documentation
(https://docs.mem0.ai/platform/mem0-mcp)

## Mem0 scope

For every Mem0 MCP read or write, pass `user_id: "allister"`.

For a direct call, use:

{ "text": "…", "user_id": "allister" }

The MEM0_USER_ID=allister environment variable applies to the Mem0 CLI, not the hosted MCP connector. Mem0’s hosted
MCP setup and tool list do not document a configurable default user ID. Mem0 MCP documentation
(https://docs.mem0.ai/platform/mem0-mcp)

### When to use Mem0

Store information that is likely to be useful across future sessions or across different agents, including:

* Architectural decisions and the reasoning behind them.
* Project conventions and established patterns.
* Important user preferences and requirements.
* Known limitations, constraints, and workarounds.
* Significant implementation decisions.
* Useful discoveries made while debugging.
* Important commands, configuration details, or environment-specific behaviour.
* Decisions that another agent would otherwise need to rediscover.

### Before starting work

Before making significant changes:

1. Search Mem0 for context relevant to the repository, component, feature, or issue.
2. Use existing memories to avoid repeating previous investigation or contradicting established decisions.
3. Treat repository source code and current documentation as authoritative if they conflict with stored memory.

Do not blindly trust old memories. Validate anything that may have become stale.

### Writing memories

Create memories when information has lasting value.

Memories should be:

* Concise.
* Self-contained.
* Specific enough to be useful without the original conversation.
* Written in clear human-readable language.
* Focused on decisions and useful context rather than activity logs.
Prefer:

> The API uses RFC 7807 Problem Details for all HTTP error responses.

Instead of:

> Updated the API errors today and changed some files.

Include relevant identifiers such as service names, package names, directories, issue numbers, or architecture components where they improve retrieval.

### What to store
- User preferences (Stated likes, dislikes, and working style)
- Key facts and context (Important information shared by the user)
- Ongoing tasks (Active work, projects, and follow-ups)

### What not to store
- Sensitive data (Credentials, tokens, and PII)
- Filler conversation (Generic chit-chat and pleasantries)

### Updating memory

When an existing decision changes:

* Update or supersede the previous memory rather than creating contradictory memories.
* Record the new decision and, where useful, briefly explain why the previous approach was replaced.
* Remove or invalidate memories known to be incorrect.

### Finishing work

At the end of meaningful work, consider whether another agent or future session would benefit from anything learned during the task.

Persist only the durable knowledge that would reduce future investigation or prevent repeated mistakes.

Mem0 should act as shared project knowledge, not as a transcript of agent activity.
                                                                                                      

