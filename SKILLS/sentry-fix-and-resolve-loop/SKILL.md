---
name: sentry-fix-and-resolve-loop
description: "In one Codex session you asked the agent to \"look through the sentry issues for cookie-web and implement fixes\" six times over two days. After most rounds you had to follow up with a separate request to mark or close the issues."
---

# Sentry triage for cookie-web is re-requested six times, and "mark as fixed" is asked for separately each time

Save this as a skill so the whole loop, including resolving, runs from one short prompt.

```markdown
---
name: sentry-triage-and-fix
description: Triage unresolved Sentry issues for a project, fix one or more in code, open a PR, and mark the issues resolved in Sentry once the fix is merged. Use when asked to "look through the sentry issues for cookie-web and implement fixes", "fix sentry issues", "mark the tickets as fixed" or "close the fixed ones".
---

# Sentry triage and fix

## Steps

1. List unresolved issues with the Sentry MCP `search_issues` tool (project `cookie-web` unless told otherwise, query `is:unresolved`, sort by frequency). Show the top issues with ID, title, event count and last seen.
2. If the user did not name an issue, pick the most frequent one that is fixable in code. Skip issues that are old, have no recent events, or come from external causes. Say why you skipped them.
3. Open each chosen issue with `get_sentry_resource` and read the stack trace, tags and release.
4. Find the code in the matching repository (`Cookie-Web` or `Cookie-Worker`). Check for AGENTS.md first. Make the smallest fix and add or update a test.
5. Run the project's tests and lint. Do not finish if they fail.
6. Open a PR with `gh pr create` and reference the Sentry issue IDs in the description.

## Resolving issues

- When the PR is merged (check with `gh pr view {number} --json state,mergedAt`), call the Sentry MCP `update_issue` with status `resolved` for every issue it fixed, without waiting to be asked.
- If the PR is not merged yet, say the issues are still open and offer to resolve them after the merge.
- Finish with a list of the issues fixed and resolved, and how many unresolved issues remain.
```
