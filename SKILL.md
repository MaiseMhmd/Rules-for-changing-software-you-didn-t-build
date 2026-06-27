---
name: inherited-codebase-rules
description: Rules for using an AI coding agent (Claude, Cursor, Replit Agent, Copilot, etc.) to change software that is already built and running in production. Use whenever modifying, debugging, redesigning, or shipping changes to an existing/live codebase you didn't build from scratch, or when drafting prompts for a coding agent to make changes to such a system. Enforces verify-before-change, isolate every change, scoped tasks with explicit guardrails, and add-only/never-delete.
---

# Working in an Already-Built, Live Codebase

Rules for using an AI coding agent on software that is **already built and in production** — joining mid-project, with real users and real data, where the agent writes the actual code. The danger here isn't a blank page; it's an agent making confident assumptions about a system it doesn't understand, editing the live version, deleting "unused" code, or bundling many changes into one. Every rule below shrinks the blast radius of a mistake.

---

# Section A — Working Rules

## 1) Verify Before You Change (YOU MUST FOLLOW)

- Inspect the real system before editing — never trust the problem description alone
- Confirm: current structure, how it's actually implemented, whether the reported problem is really happening, what roles/permissions/data paths exist, and what the change would touch
- Produce a short written summary of findings BEFORE editing a single line
- A reported "bug" is often working behavior misunderstood; a requested change is sometimes already built — check, don't assume

## 2) Never Edit the Live Version Directly (YOU MUST FOLLOW)

- Every change starts in an isolated copy — a task, branch, or sandbox
- Do all editing, testing, and validation there
- Apply to main/production only after the change is verified — deliberately, by a human
- Main stays untouched until the solution is proven

## 3) Be Honest About Capabilities

- State up front what the agent can and cannot do: create an isolated task, apply changes to main, or only edit files directly
- Never claim work is sandboxed or isolated if it isn't
- If a capability is missing, name it and have a human cover the gap

## 4) One Scoped Task at a Time (IMPORTANT)

- Each request is a single, self-contained change that does not depend on or contradict other in-flight work
- Never bundle unrelated changes into one request
- Name what is in scope AND list the systems that must stay untouched ("Do NOT change X, Y, Z")

## 5) Propose → Approve → Apply

- The agent drafts a tight, guard-railed change and shows its work
- The human reviews it and decides when it goes live
- The agent never pushes to main on its own

## 6) Report Findings, Ask Permission

- Relay what you found and wait for a go-ahead before changing data or behavior
- After running, list exactly which files, endpoints, functions, and data changed
- Confirm nothing outside the agreed scope was touched

## 7) Protect Load-Bearing Systems (IMPORTANT)

- Identify the components that quietly hold everything up: data intake, permission/visibility rules, scheduled jobs, assignment logic, sync watermarks
- Mark them off-limits to casual change
- Removing a button or screen must NEVER silently disable the job behind it
- Never assume something is "unused" without proof

## 8) Add Only — Never Delete (YOU MUST FOLLOW)

- Treat every change as additive
- Do NOT remove existing code, fields, endpoints, handlers, or UI as a side effect
- If deletion or replacement is truly required: STOP, state exactly what will be removed/replaced and the impact, and wait for approval
- Redesigns change appearance only — same fields, same actions, same data flow

## 9) Change Discipline

- Make the smallest change that solves the problem
- Fix root causes, not symptoms
- Don't refactor unrelated code unless explicitly requested
- Read the relevant code before modifying it — state assumptions when unclear
- Handle null, empty, loading, and error states explicitly — no silent failures

---

# Section B — Prompting the Agent

## 1) Global Guardrail (paste at the top of EVERY prompt)

```
GLOBAL GUARDRAIL — read first: Make only ADDITIVE changes. Do NOT delete or
remove any existing code, fields, endpoints, handlers, state, or UI. If a change
genuinely requires deleting or replacing existing code, STOP and ask me first —
clearly state exactly what would be removed/replaced and the impact — and wait
for my approval. Any restyling must preserve all existing functionality, data
bindings, and fields; change appearance only, not behavior. Work inside an
isolated task/branch; the main version stays untouched until I explicitly apply
the change.
```

## 2) Two-Step Prompt Template (Verify → Implement)

```
[Global Guardrail]

Scope: ONLY [the specific thing]. Do NOT touch [systems to leave alone].

Step 1 — verify first, report back, do NOT edit yet:
- [What is the current structure / implementation of the thing?]
- [Is the reported problem actually happening? Where exactly?]
- [What data, roles, or other systems would this change affect?]
- Summarize findings. Do not change anything yet.

Step 2 — after I approve (additive; if any existing line must be replaced,
state exactly what and wait for my OK):
- [The specific, minimal change.]
- [How to handle existing/legacy data so nothing breaks.]

When done: list every file changed, confirm the off-limits systems are
untouched, and confirm [the invariant that proves it works].
```

## 3) Definition of Done

- List every file changed
- Confirm the off-limits systems were not touched
- State the invariant that proves the change works
- Provide before/after counts for any data migration or backfill

---

# Why This Works

- Working in a live, already-built codebase is not a creativity problem — it's a **blast-radius** problem
- Every rule above shrinks the blast radius of a mistake: verify before you touch, isolate before you edit, add before you delete, keep a human between "looks done" and "it's live"
- Move fast inside these rails — the rails are what let you move fast without taking the company's software down with you
