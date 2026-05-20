# Thread Consolidator

A brain-app for [BrainFoundry](https://brainfoundry.ai) brains.

Created: 2026-05-20

**Take your favorite chats off the big AI and bring them home to your brain.**

Most of your chat history is stuck inside someone else's product, in a format
your brain can't index. Thread Consolidator is the smallest possible bridge:
paste a transcript, your brain reads it and proposes structured memory
entries — classified by layer (episodic / semantic / procedural), tagged by
kind, with a self-scored confidence. You approve per entry; each approval is
appended to the right layer as one durable note.

A productization of the manual chat-consolidation ritual the operator was
already doing by hand.

## Install

In your brain: **Settings → Apps**, paste this repository's URL, **Preview**,
**Install**. The app adds a *Thread* tab.

## How it works

The app carries no intelligence of its own. It borrows the host brain's
reasoner and memory through the brain-app `postMessage` bridge:

- `llm.complete` — the brain reads the pasted transcript and returns a
  JSON array of proposed memory entries (layer + title + content + kind +
  confidence). Retrieval is restricted to the read-mode layers declared in
  `brain-app.yaml`. The app holds no key and picks no model.
- `memory.write` — one call per approved entry, appended to the layer the
  operator confirmed.

Until the operator clicks Approve, nothing touches the brain's memory —
proposals live client-side. Skip discards a proposal.

Opened standalone — as a file, with no host brain — the app shows a "no
brain connected" message rather than fabricate local proposals.

## The layers, briefly

- **episodic** — what happened, when, who, decisions made. Default for
  ambiguous cases.
- **semantic** — durable facts / beliefs / concepts that survive the day.
- **procedural** — how-to / process / protocol the operator wants to
  repeat (commands, sequences, recipes).

## Permissions

| Permission | Why |
|---|---|
| `llm.invoke` | read the transcript and produce classified proposals (RAG over `semantic` + `episodic`) |
| `memory.write` | append each approved proposal to its layer (`episodic` / `semantic` / `procedural`) |

See `brain-app.yaml` for the full manifest.

## Build

None. One self-contained `index.html` — no dependencies, no CDN, no build
step. A sovereignty claim shouldn't ship third-party script.

## License

AGPL-3.0 — see [`LICENSE`](./LICENSE). Brain-apps must be license-compatible
with the brain they install into.
