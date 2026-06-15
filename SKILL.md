---
name: "research-subagent-cleanup"
description: "Use and clean up subagents when they genuinely help."
---

# Research Subagent Cleanup

Use subagents at the agent's discretion. Do not spawn them automatically for every research task. Use judgment.

## Core Rule

Use subagents only when they materially improve the work, then clean them up after their results are integrated.

Good reasons to use subagents:

- Independent research tracks can run in parallel.
- A codebase question is specific and bounded enough for an explorer.
- A large task can be split into disjoint ownership areas.
- A second pass would reduce real risk before changing or publishing something.
- The main agent can keep moving locally while sidecar work runs.

Bad reasons to use subagents:

- To look busy.
- For tiny tasks the main agent can answer directly.
- For urgent blocking work where the next step depends on the result.
- For vague "research everything" requests.
- When delegation would expose more private context than needed.

## Tool Policy

- Follow the current runtime/tool rules for subagent usage.
- For native research and codebase work, prefer native subagents when available and permitted.
- For platform-specific delegated work, use the platform's session-spawn tool.
- Do not use heavyweight session spawning for ordinary code/research delegation when native subagents are available.

## Delegation Pattern

1. Decide the local critical path first.
   - Identify what the main agent should do locally now.
   - Delegate only sidecar work that can run independently or in parallel.

2. Split delegated work into focused questions.
   - Good: "Find current official API docs for X and summarize breaking changes."
   - Good: "Inspect module A and report where auth errors are handled."
   - Bad: "Research everything about this."

3. Spawn subagents with narrow, self-contained prompts.
   - State the exact output needed.
   - Tell them not to modify files unless explicitly assigned a disjoint write scope.
   - For web/current facts, require primary sources and links.
   - For codebase exploration, ask for file paths and line references.

4. Keep working locally while they run.
   - Do not block on subagents unless their result is required for the next critical-path step.

5. Collect and integrate results.
   - Wait only when needed.
   - Compare findings, resolve contradictions, and verify anything high-risk.
   - Do not dump raw subagent output to the user unless it is useful.

6. Clean up every spawned subagent/session.
   - Native subagent: close/delete each spawned agent after collecting its result.
   - Platform session spawn: set cleanup/delete at spawn time when supported.
   - If cleanup cannot be performed, note the reason briefly in the final answer or handoff.

## Cleanup Checklist

Before final response, check:

- Did I spawn any native subagents?
- Did I wait for or receive their final outputs?
- Did I close/delete each no-longer-needed native agent?
- Did I use cleanup/delete for temporary platform sessions where supported?
- Did I integrate results into my own answer instead of forwarding raw noise?

## Safety Rules

- Do not delegate private or sensitive context unless the subagent needs it.
- Prefer isolated context; fork full context only when necessary.
- Do not give subagents secrets, OAuth tokens, passwords, or unnecessary personal data.
- Do not let multiple workers edit overlapping files.
- Do not keep completed agents open just because they might be useful later.
- If a subagent is still running and no longer needed, close it.

