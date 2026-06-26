# CLAUDE.md — Rules for Working in an Inherited, Live Codebase

A drop-in `CLAUDE.md` for anyone using an AI coding agent (Claude, Cursor, Replit Agent, Copilot, etc.) to change software that is **already built and in production** — the common case of joining a project mid-stream, with real users and real data, where the agent writes the actual code.

Most agent guidance assumes a greenfield project. This is the opposite: the danger isn't a blank page, it's an agent making confident assumptions about a system it doesn't understand, editing the live version, deleting "unused" code, or bundling five changes into one. These rules shrink the blast radius of every change.

## What's inside

- **Section A — Working in an Inherited, Live Codebase:** nine rules covering verify-before-you-change, isolate every change, capability honesty, scoped tasks, propose→approve→apply, protecting load-bearing systems, and add-only/never-delete.
- **Section B — Prompting the Agent:** a copy-paste **Global Guardrail**, a **Verify → Implement** prompt template, and a **Definition of Done**.

## How to use it

1. Drop [`CLAUDE.md`](./CLAUDE.md) into the root of your repo (or your agent's rules/context file).
2. Paste the **Global Guardrail** at the top of every change request.
3. Structure each request with the **Verify → Implement** template — make the agent inspect and report on the current state before it edits anything, and gate the implementation on your approval.

## The core idea

Working in a live, inherited codebase isn't a creativity problem — it's a **blast-radius** problem. Verify before you touch, isolate before you edit, add before you delete, and keep a human between "looks done" and "it's live." Move fast inside those rails.

## License

MIT — use it, fork it, adapt it to your stack.
