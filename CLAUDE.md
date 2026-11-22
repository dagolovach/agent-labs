# agent-labs — instructions for every session

## What this is

Small, runnable labs. The owner, Dmitry Golovach (principal engineer, 2× CCIE), builds and tests AI agents,
measures what they do, and writes it down. Each lab shows one principle: break the naive version, fix it, report
real numbers (shape, correctness, cost, time). The labs are how he learns how agents are built in detail; each
lab also becomes a post on dmitrygolovach.com.

## The program (one lab at a time, in this order)

Each lab has a suggested network analogy. All twelve are **drafts for the owner to correct**; the post uses his
version.

1. Structured output — "reply in JSON" is a wish; a schema guarantees shape, never truth. **Done, waiting for the
   owner's first run.**
   Analogy (draft): YANG/NETCONF vs screen-scraping CLI output; the schema guarantees the shape, not that the
   config is right.
2. Quarantine — untrusted text never reaches the model that has tools.
   Analogy (draft): segmentation and a DMZ; untrusted traffic ends in a zone that cannot reach the core.
3. Tools and the loop — the model proposes a call, code runs it; stop rules and step limits.
   Analogy (draft): TTL and hop limits; a forwarding loop ends by rule, not by hope.
4. The model proposes, code decides — dates, amounts and counts are computed in code.
   Analogy (draft): a peer advertises routes, but your route policy and prefix filters decide what gets installed.
5. Retries and idempotency — why a retry creates a second task; keys and an outbox.
   Analogy (draft): change scripts that are safe to re-run; TCP sequence numbers that drop a duplicate segment.
6. Least privilege — permission tiers auto / confirm / never.
   Analogy (draft): RBAC and AAA command authorization on devices; show commands run, config needs approval, some
   commands never.
7. Evals — pass@k vs pass^k; how you know it works.
   Analogy (draft): pre- and post-change validation; the same checks before and after every change, every time.
8. Context, memory and caching — what the model actually remembers, and what caching saves.
   Analogy (draft): ARP and MAC tables that age out; a device knows only what is in the table right now.
9. Logs and traces — answering "why did the agent do that?" a week later.
   Analogy (draft): syslog, NetFlow and the post-mortem; reconstructing why traffic went where it did.
10. Timeouts and fallbacks when the model is down.
    Analogy (draft): BFD and failover; detect a dead path fast and move to the backup by rule.
11. Retrieval with sources (RAG).
    Analogy (draft): answer from the source of truth (IPAM, inventory) and cite the record, not from memory.
12. Several agents and an orchestrator.
    Analogy (draft): a controller that holds the intent and devices that do the forwarding.

## Every post has a network analogy

Every lab's post contains one network analogy: the same principle as it shows up in network operations. The
analogy is the owner's: you may draft one, he corrects or replaces it, and the post uses his words. It explains the
principle; it never replaces the lab's numbers, and networks never become separate labs.

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

Every lab follows `BLUEPRINT.md`: one fixed stack, one folder shape, one build order. Start a new lab with
`cp -r lab-template NN-short-name`, change only the parts marked `CASE` in `lab.py`, and fill the README in the
template's sections. In short:

- Python, the official `anthropic` SDK, `pydantic`, JSON data files. One `lab.py`, readable top to bottom, under
  ~250 lines. No web server, front end, Docker or agent framework in a lab.
- Every lab prints its cost from `response.usage` and the price table, its time, and pass^k over `--runs`.
- `--model` is a flag, so the same lab can be re-run on a new model ("same test, new model" posts).
- `lab.py --stub` tests the scoring offline before a lab is handed over; say plainly that the real-model run
  has not happened yet when it hasn't.
- A new tool joins the stack only when a lab cannot show its principle without it; then `BLUEPRINT.md` says
  why, with the date.

## LOG.md — the bridge to posts

`LOG.md` is the one place that says what was done. At the end of every session, before the last commit, add
an entry at the top:

```
## YYYY-MM-DD — Lab N: short name
- Done: what changed this session, in plain words.
- Numbers: only from this lab's own runs (model, runs, the key figures); "not run yet" when there are none.
- Surprised: what did not go as predicted, or "nothing yet".
- Takeaway: one line a reader could quote.
- Network analogy: the owner's version for this lab's post, or "not chosen yet".
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

- The plan with weekly learning, posts and mocks: the "Posts and learning plan" doc
  (https://claude.ai/artifact/FV6LczSAHbeBAwY7C2vW3H).
