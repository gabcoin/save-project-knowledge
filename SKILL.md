---
name: save-project-knowledge
description: >
  Run at the end of a meaningful work session or completed task to preserve
  durable project knowledge before clearing the Claude Code conversation.
  Extracts verified solutions, decisions, architecture changes, project rules,
  agent-routing discoveries, gotchas, and reusable workflows into the
  appropriate project knowledge files. Trigger with /save-project-knowledge or
  /spk.
---

# Save Project Knowledge

Review the work performed and knowledge acquired during the CURRENT
Claude Code session.

The goal is NOT to summarize the conversation.

The goal is to preserve only durable knowledge that future Claude sessions
would otherwise need to rediscover.

Think of this as converting expensive session context into cheap,
persistent project knowledge.

---

# 1. FIRST: INSPECT EXISTING PROJECT KNOWLEDGE

Before writing anything, inspect the existing knowledge system:

- `CLAUDE.md`
- `.claude/rules/`
- `.claude/skills/`
- `docs/architecture.md`
- `docs/decisions.md`
- `docs/solved-problems.md`
- `docs/troubleshooting.md`

Also inspect any other canonical documentation this project actually uses:
`docs/workflows.md`, scoped `AGENTS.md` files, `README.md`, `docs/` in
general, or whatever equivalent structure exists here.

The destinations named below are the default convention. Adapt them to the
project you are in:

- If a destination file already exists, use it.
- If the project keeps the same knowledge somewhere else under a different
  name, use that file instead.
- Create a new knowledge file only when the knowledge is durable and no
  reasonable home exists. Never scatter new files across the repo.

Do NOT assume knowledge from this session is new.

Search before writing.

Prefer:
- updating an existing entry
- correcting an outdated entry
- extending an existing rule

over creating duplicate knowledge.

---

# 2. IDENTIFY DURABLE KNOWLEDGE

Review the current session for the following categories.

## A. Verified solutions

Persist problems that were actually solved and would be expensive or
non-obvious to rediscover.

Capture:

- problem
- symptoms
- root cause
- verified solution
- verification performed
- approaches that should not be repeated
- relevant files/components

Destination:

`docs/solved-problems.md`

Do NOT persist unverified guesses as solutions.

---

## B. Architecture

Persist meaningful discoveries or changes involving:

- system boundaries
- data flow
- component responsibilities
- integrations
- APIs
- database architecture
- rendering architecture
- important dependencies
- architectural invariants

Destination:

`docs/architecture.md`

Do NOT document architecture that is trivially discoverable from the
directory structure or source code.

---

## C. Decisions

Persist technical or product decisions that were actually accepted or
implemented.

Capture:

- decision
- reason
- important alternatives rejected
- consequences
- relevant components/files

Destination:

`docs/decisions.md`

Do NOT persist brainstorming as decisions.

---

## D. Project rules

Persist a rule only when future Claude sessions should consistently obey it.

Examples:

- implementation constraints
- required conventions
- safety requirements
- important project invariants
- mandatory verification behavior

Choose destination based on scope.

Always-relevant, critical rule:

`CLAUDE.md`

Scoped rule:

`.claude/rules/<domain>.md`

Avoid increasing `CLAUDE.md` unless the rule genuinely deserves to be
loaded in most sessions.

---

## E. Agent routing

Projects vary in which specialized agents are available and useful.

Do NOT persist an agent simply because it was used.

Persist agent-routing knowledge only when this session established that:

- an agent MUST be used for a particular project-specific task
- an agent MUST NOT be used for a particular task
- an agent is the established specialist for a project-specific area
- an agent must run before/after another agent
- a particular combination of agents proved valuable
- an agent or delegation pattern proved wasteful or inappropriate
- the user explicitly established a routing preference

Capture:

- trigger
- agent
- MUST / SHOULD / MUST NOT
- direct vs orchestrator
- ordering/dependencies
- reason

Destination:

`CLAUDE.md` only for critical always-relevant routing.

Otherwise:

`.claude/rules/agents.md`

Do NOT reproduce generic capabilities already contained in agent
definitions.

---

## F. Orchestration/workflow discoveries

Persist meaningful discoveries about HOW work should be performed.

Examples:

- when main Claude should work directly
- when Explore is appropriate
- when agent-orchestrator is appropriate
- useful specialist sequences
- useful parallel workflows
- required validation/review stages
- delegation patterns that wasted tokens/context

Destination:

`CLAUDE.md` for universal workflow policy.

`.claude/rules/` for domain-specific workflow policy.

If the procedure is reusable and multi-step, evaluate it as a Skill
candidate instead. Stable repeatable procedures belong in the project's
workflow document (`docs/workflows.md` by convention) when one exists.

---

## G. Gotchas

Persist non-obvious traps that caused or could cause meaningful future
problems.

Capture:

- gotcha
- symptoms/consequence
- cause
- correct behavior
- affected scope

Destination:

`docs/troubleshooting.md`

or `.claude/rules/` when preventative behavior should be enforced.

---

## H. Reusable commands / operations

Persist commands only when they contain project-specific or non-obvious
operational knowledge.

Examples:

- special deployment procedure
- unusual testing sequence
- production validation command
- migration procedure
- debugging command

Do NOT document routine commands that are already obvious from
`package.json`, scripts, or tooling configuration.

---

# 3. DETECT SKILL CANDIDATES

Look for workflows discovered or repeated during this session.

A workflow is a Skill candidate when:

- it is likely to be performed again
- it involves multiple ordered steps
- it contains non-obvious methodology
- it requires a repeatable agent sequence
- the user would otherwise need to explain the procedure again
- performing it incorrectly is expensive or risky

Examples:

- production readiness audit
- deploying a specific subsystem
- implementing a major UI feature
- auditing an n8n workflow
- generating a marketing plan
- validating a release

If a strong Skill candidate is found:

DO NOT automatically create a new Skill unless the workflow is clearly
stable and reusable.

Instead report:

SKILL CANDIDATE:
- proposed name
- trigger
- workflow
- why it deserves a Skill

If an existing Skill already covers the workflow, update it only when the
session established a verified improvement.

---

# 4. HANDLE CONTRADICTIONS

Session history may contradict existing documentation.

When this happens, determine authority in this order:

1. current verified runtime behavior
2. current code/configuration
3. explicit user decision
4. verified result from this session
5. existing documentation
6. older conversation assumptions

Do NOT silently overwrite a meaningful unresolved conflict.

If the correct state cannot be established, report the conflict instead.

---

# 5. DELETE OBSOLETE KNOWLEDGE WHEN APPROPRIATE

Knowledge persistence includes removing incorrect knowledge.

If this session proves an existing rule, solution, architectural
description, or decision obsolete:

- update it
- replace it
- or remove it

Do not keep both old and new instructions active if they contradict.

Historical context may remain in `docs/decisions.md` when the history itself
is valuable, but operational instructions must reflect the current state.

---

# 6. TOKEN / CONTEXT DISCIPLINE

Be aggressive about what deserves persistence.

Do NOT persist:

- conversation summaries
- temporary debugging output
- routine code changes
- obvious information available directly from code
- generic framework knowledge
- generic agent capabilities
- speculative ideas
- abandoned brainstorming
- failed attempts with no reusable lesson
- intermediate states
- temporary file contents
- information already accurately documented

The objective is NOT maximum memory.

The objective is:

minimum persistent context required to prevent expensive rediscovery.

---

# 7. UPDATE THE KNOWLEDGE BASE

After identifying durable knowledge:

1. search the appropriate destination for an existing entry
2. update rather than duplicate
3. create a new entry only when necessary
4. remove or correct obsolete instructions
5. keep entries concise
6. preserve enough evidence/context to understand WHY non-obvious rules exist

Do not create new documentation files unnecessarily.

Use the established project knowledge structure.

---

# 8. FINAL KNOWLEDGE REPORT

After updating the files, return a concise report:

## Saved

List durable knowledge actually persisted and destination files.

## Updated

List existing knowledge that was corrected or extended.

## Removed / superseded

List obsolete knowledge removed or replaced.

## Not persisted

Briefly mention important session work intentionally considered temporary.

## Skill candidates

List only strong candidates discovered.

## Open questions

List unresolved contradictions or important unknowns.

## Safe to clear

Return:

YES

if the important durable knowledge from this session is now represented
in the project knowledge system.

Return:

NO

if something important remains dependent on this conversation, and explain
exactly what still needs to be preserved.

---

# Core principle

The conversation is temporary working memory.

The repository is long-term project memory.

Preserve conclusions, decisions, rules, and verified knowledge.

Do not preserve the conversation itself.
