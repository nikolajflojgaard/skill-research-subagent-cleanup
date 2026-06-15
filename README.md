# Research Subagent Cleanup

Use subagents when they genuinely help, then clean them up.

This AgentSkill is a small operating rule for agent systems that support delegated workers, subagents, sidecar sessions, or temporary research agents. It is intentionally boring: decide whether delegation is worth it, give the side agent a narrow job, collect the result, integrate it, and close the session.

That sounds obvious until you watch an agent spawn three side quests, lose track of the main task, and leave half the context scattered across abandoned sessions.

## What problem does this solve?

Subagents are useful when work can happen in parallel or when a second focused pass reduces risk.

They are wasteful when the task is small, sequential, or mostly a matter of judgment.

This skill gives the main agent a practical rule:

- do not spawn subagents automatically
- use them only when they improve speed, coverage, independence, or risk reduction
- keep prompts narrow and self-contained
- do not share more private context than needed
- collect and integrate results before answering
- close or delete temporary sessions when finished

The goal is not "more agents." The goal is less context drag and cleaner execution.

## When to use it

Use this skill when:

- a research task has independent tracks
- a codebase question can be split into bounded areas
- a second pass would catch mistakes before publishing or changing files
- the main agent can continue useful work while side research runs
- a long task risks bloating the main context window

Do not use it when:

- the answer is obvious or small
- the next step depends entirely on the delegated result
- the prompt would be "research everything"
- multiple workers would edit overlapping files
- delegation would expose unnecessary private or sensitive context

## Example use cases

Good delegation:

```text
Subagent A: inspect the authentication module and report where expired-token errors are handled. Return file paths and line references only.

Subagent B: check the official API docs for OAuth refresh-token behavior. Use primary sources and return links.

Main agent: keep implementing the local fix while those checks run.
```

Poor delegation:

```text
Research this entire product and tell me everything useful.
```

The first version reduces load. The second version creates noise.

## Installation

Copy `SKILL.md` into your agent skills directory.

Example:

```bash
mkdir -p ~/.config/agent/skills/research-subagent-cleanup
cp SKILL.md ~/.config/agent/skills/research-subagent-cleanup/SKILL.md
```

Adapt the path to your runtime. Different agents name their skill/plugin directories differently.

## How the skill works

The skill tells the main agent to follow this sequence:

1. Decide the local critical path.
2. Delegate only independent side work.
3. Give each subagent a narrow prompt.
4. Keep working locally where possible.
5. Wait only when the result is needed.
6. Integrate the result into the main answer or implementation.
7. Close or delete every temporary subagent/session.

It also includes a cleanup checklist so the main agent does not leave finished workers hanging around.

## Runtime notes

This skill is runtime-agnostic.

It does not assume one specific subagent API. Use the equivalent operations in your environment:

- spawn/create a subagent or temporary session
- wait for or collect its result
- close/delete/cleanup the session afterward

If your platform supports cleanup at spawn time, prefer that for temporary sessions.

## Safety notes

- Do not give subagents secrets, tokens, passwords, or unnecessary private data.
- Do not delegate broad private context just because it is convenient.
- Do not let multiple agents edit the same files unless the write boundaries are explicit and disjoint.
- Verify high-risk findings before acting on them.
- If a subagent is no longer needed, close it.

## Repository contents

- `SKILL.md` - the actual reusable skill
- `README.md` - public documentation
- `LICENSE` - license terms

## Why this exists

Subagents should reduce cognitive and context load.

If they make the main agent track more state, reconcile more noise, or burn more tokens, they are being used wrong.

This skill keeps the rule simple: delegate only when it earns its keep, then clean up.
