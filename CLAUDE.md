# agent-labs — instructions for every session

## What this is

Small, runnable labs. Each lab shows one principle of building AI agents that are safe to let act: break the
naive version, fix it, report real numbers (shape, correctness, cost, time). The owner, Dmitry Golovach
(2× CCIE, network engineer moving into applied AI architecture), uses the labs to learn how agents are built in
detail; each lab also becomes a post on dmitrygolovach.com.

## The program (one lab at a time, in this order)

1. Structured output — "reply in JSON" is a wish; a schema guarantees shape, never truth. **Done, waiting for the
   owner's first run.**
2. Quarantine — untrusted text never reaches the model that has tools.
3. Tools and the loop — the model proposes a call, code runs it; stop rules and step limits.
4. The model proposes, code decides — dates, amounts and counts are computed in code.
5. Retries and idempotency — why a retry creates a second task; keys and an outbox.
6. Least privilege — permission tiers auto / confirm / never.
7. Evals — pass@k vs pass^k; how you know it works.
8. Context, memory and caching — what the model actually remembers, and what caching saves.
9. Logs and traces — answering "why did the agent do that?" a week later.
10. Timeouts and fallbacks when the model is down.
11. Retrieval with sources (RAG).
12. Several agents and an orchestrator.

Networks appear only as analogies (zones/DMZ for quarantine, ACLs for least privilege, TCP sequence numbers for
idempotency), never as separate network labs.

## The owner's part is the point

The labs exist so the owner learns. Never do his part for him:

- the ground truth (`expected.json` or equivalent) is drafted by you, **checked and decided by him**;
- he adds his own cases and **writes a prediction before the first run**;
- he reads the failures and explains the result out loud; the post is edited from his words, not written for
  him.

Each lab README therefore has a "Your part, before any code runs" section and "Questions to answer out loud".
If he asks you to skip these, remind him once why they exist, then do what he decides.

## The week: read → your part → lab → post

One lab a week. Reading comes first and is the theory for both the lab and the post:

1. **Read first** — the lab's sources from `reading.md` (45–60 minutes), after a 5-minute written first guess.
   Then the **reading check**: six questions from Claude (recall, explain simply, why, apply, limits, predict),
   answered out loud or on paper; the protocol and the notes live in the private `career` repo.
2. **Your part** — check the ground truth, add own cases, predict.
3. **Lab** — run, read the failures, compare with the reading.
4. **Post** — from `LOG.md` and his voice note; the reading becomes the "how others do it" part, quoted and
   linked.

When a new lab is built, fix its reading first: one or two primary sources, links checked in a browser or a
search, never from memory. Update `reading.md` and the lab README's "Read first" section.

## How a lab is built

- Folder `NN-short-name/` with `README.md`, `lab.py`, its data files, and `results/` (git-ignored).
- README sections: the principle in one sentence · why it matters later · read first · the setup · your part before any
  code runs · run it · read the results yourself · how big systems do this · in Theo · questions to answer out
  loud · what went into this lab (an honest line on AI help).
- Python, the official `anthropic` SDK, `pydantic`. Minimal: one file, readable top to bottom, under ~250 lines.
- Every lab prints its cost from `response.usage` and the price table, its time, and pass^k over `--runs`.
- `--model` is a flag, so the same lab can be re-run on a new model ("same test, new model" posts).
- Test the scoring offline with a stub client before handing a lab over; say plainly that the real-model run
  has not happened yet when it hasn't.

## LOG.md — the bridge to posts

`LOG.md` is the one place that says what was done. At the end of every session, before the last commit, add
an entry at the top:

```
## YYYY-MM-DD — Lab N: short name
- Done: what changed this session, in plain words.
- Numbers: only from this lab's own runs (model, runs, the key figures); "not run yet" when there are none.
- Surprised: what did not go as predicted, or "nothing yet".
- Takeaway: one line a reader could quote.
- Hooks: two or three first lines for a post.
```

Social posts (LinkedIn, X, Threads) are written in the owner's chat project from this log and his voice
notes, never in this repository. A post never states a number or conclusion that is not in the log.

## Rules

- Never invent a number. A lab's numbers come from its own runs; a missing number is written as missing.
- Anything about Claude models or the API: follow the current official docs (the `claude-api` skill), not
  memory.
- Nothing from the owner's employer: no names, internals or figures. Work topics only as general concepts or
  lab rebuilds.
- English in code, READMEs and commits. The owner may write in Russian; reply in Russian.
- Small commits with conventional messages (`feat(lab2): …`). Push to `main` only when the owner says so.

## Related

- Theo (private repo `dagolovach/inhand`) is the owner's production assistant; many labs have a Theo
  counterpart (the Reader with no tools, the outbox, permission tiers, evals at pass^3).
- The plan with weekly learning, posts and mocks: the "Posts and learning plan" doc
  (https://claude.ai/artifact/FV6LczSAHbeBAwY7C2vW3H).
