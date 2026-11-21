# The agency: a plan

A small multi-agent system that I build myself (my own code, my own prompts, real API calls) to run my labs. I
run it as a **pretend agency**: each lab is a project for a made-up client, the agents are the staff, and I am
the founder who reviews and approves. The agency, its clients and its staff are invented and always labeled as
invented. The numbers come only from real runs.

Why: to practise multi-agent design on real work, and to stop doing every lab by hand. Only I use it.

Inspiration: Brian Armstrong's workflow picture (lead agents, a review queue, a "Brain" that learns from his
feedback) and the "company brain" picture by @VibeMarketer_ (one shared map, every fix goes in once and stays
fixed, the brain is separate from the model). This plan keeps the ideas and drops the size: **start with one
pipeline for one lab, and add staff only where my time goes.**

Status: plan only. No code yet.

## 1. How it works, in one picture

```
brief ─► Architect ─► [Founder check 1] ─► Engineer ⇄ QA ─► Runner ─► Analyst ─► [Founder check 2] ─► Writer
                        ground truth,         (max 2 rounds)   (code)                 I read failures,      post
                        my prediction                                                  voice note           drafts
                                         every edit I make ─► Librarian ─► brain change I approve
```

The **account lead is plain Python code**, not a model. It runs the stages in order, saves state after each one,
and stops at my two checks. ("Workflows use predefined code paths; agents direct their own process" —
Anthropic, *Building effective agents*. I start with a workflow, on purpose.)

## 2. The staff

| Role | Input | Output | Model or code |
| --- | --- | --- | --- |
| Account lead | the project folder | runs the stages, stops at my checks | code |
| Architect | `brief.md` + company map | a project spec (JSON: principle, arms, data, metrics), draft data and draft ground truth | model |
| Engineer | the spec | the lab code, written in a sandbox folder, tested offline with a stub client | model + tools |
| QA | spec + code + policies | pass, or a list of fixes (JSON) | model |
| Runner | the lab code | a real run with `--runs`, results JSON, cost | code |
| Analyst | results JSON | draft `LOG.md` entry and the facts for the retro | model |
| Writer | log entry + **my voice note** | post drafts (LinkedIn, X/Threads) in my plain English | model |
| Librarian | my edits to any draft | up to 3 proposed brain changes | model |

Rules from my labs hold for the staff: the model proposes, code decides (the Runner and every number check are
code); a separate QA, because an agent rates its own work too high (Anthropic, *Harness design for long-running
apps*); my part stays mine (ground truth, prediction, reading the failures, my opinions in the post).

## 3. Handoffs: files, not chat

Each stage writes one file in `agency/runs/<project>/<date>/` and the next stage reads only that file. Model
outputs that code depends on are **structured outputs** (`messages.parse()` with a Pydantic model), so a handoff
always has its shape; code then checks the meaning (lab 1). Untrusted text (client data, web pages) never reaches
a role with tools as free text (lab 2). `state.json` records which stages are done, so a failed stage re-runs
alone and never repeats a finished one (lab 5).

## 4. Which API, and why

- **Claude Messages API with the official `anthropic` Python SDK**, and my own loop for the Engineer's tools
  (write file, read file, run offline tests, all inside the sandbox folder). Later I may switch the Engineer to
  the SDK's tool runner, once I have written the loop by hand (lab 3).
- **Not the Claude Agent SDK and not Managed Agents for v1.** Both give a ready loop and ready sub-agents. That is
  exactly the part I want to learn, and they would tie the design to one vendor. Later, one post can compare: the
  same team built by hand vs on Managed Agents.
- **One thin model layer:** `llm.py` has one function that takes a role, its prompt and the input, and returns
  the reply and its usage. It is the only file that knows the vendor, so a model or a provider is one flag per
  role (case 17).
- Facts checked in the Claude API docs on 2026-11-21: start every role on `claude-opus-5-5` ($4 / $20 per million
  tokens in / out); `claude-sonnet-5-5` and `claude-haiku-5-5` are cheaper options to test per role later, by
  evals. On Opus 5.5 effort defaults to `medium`, so each role sets it; thinking is adaptive; forced
  `tool_choice` returns an error, so JSON handoffs use structured outputs.

## 5. The brain

One folder, `agency/brain/`, read by every role before it works. Prompts live here as markdown, not inside the
code, so the brain is separate from the model.

```
agency/
  company-map.md        every role reads this first: who we are, how we work, where things are, the rules
  brain/
    memory/             clients.md (made-up clients), decisions/ (one dated file per decision)
    judgment/           voice.md (plain English, no idioms), policies.md (never invent a number, nothing from
                        an employer, what needs my yes), approved-claims.md (numbers that came from real runs)
    capability/         roles/<role>.md (each role's prompt, written by me), skills/<skill>.md (prompt parts
                        that worked twice, reused by several roles)
    learning/           corrections.md (what I changed, dated), what-worked.md
  staff/                llm.py, budget.py, lead.py, tools.py, one small file per role
  evals/                fixtures and checks for each role
  runs/                 one folder per run (not committed)
```

**How my edits feed it.** After each check, code saves the difference between the draft and my version to
`learning/corrections.md`. The Librarian reads it and proposes up to three short changes to `judgment/` or a
role's prompt, as a diff. I approve or drop each one; an approved change is committed with an owner and a date.
A mistake is fixed once, in the brain, and stays fixed for every role.

The repo is public, so the brain holds only how the agency works: no job search, no private life, nothing from my
employer.

## 6. Each lab is a client project

The lab folder is the project folder, so nothing moves:

- `brief.md` — the client case from `CASES.md` (who, problem, inputs, success agreed before building)
- the deliverable — `lab.py`, its data and `README.md`, as today
- `expected.json` and the offline stub test — the project's evals
- `retro.md` — what worked, what I changed, what goes into the brain, the cost and time of the project
- the post — written in my private career repo, as today; `retro.md` links to it. No post text in this repo.

## 7. My review: two batches per project

1. **Before the run:** the spec, the draft data and ground truth. I check the labels, add my own cases and write
   my prediction (my part, as in every lab).
2. **After the run:** I read the failures myself and record a voice note. The Writer drafts the posts from it; I
   edit in the same sitting.

Both checks land as one file each in a queue folder, so I can review a batch at once. Nothing is posted, sent,
pushed or paid by any role.

## 8. Cost guard and safety

- Every call goes through `llm.py`, which adds `usage` × price to a ledger for the project.
- A cap per project that I set; at the cap the lead stops and reports. `max_tokens` per role; the Engineer has a
  step limit; QA has at most 2 rounds.
- Before a real lab run, the lead prints the number of calls and an estimated cost, and waits for my yes.
- The Engineer can write only inside its sandbox folder. No role can push, post or send. API keys stay in the
  environment, never in the brain or the logs.

## 9. Where it runs

No server to start. Three stages, each only when the one before is not enough:

1. **Now: on my laptop.** The brain is this git repo (markdown files); brain changes go through PRs. The staff are
   Python scripts I run with one command, for example `python agency/staff/lead.py 01-structured-output`.
2. **Later: GitHub Actions** for repeat jobs: re-run the role evals when a prompt changes, or re-run a lab's evals
   on a new model on a schedule. The API key lives in the repo's secrets.
3. **Much later: Cloudflare** (Workers or its Agents SDK), only for agents that must always be on or for a small
   web page to review the queue. A good second milestone, not the first step.

## 10. Evals for the agency itself

- **Per role:** 3 fixed inputs with checks in code (the spec has every field; the code passes the stub test and
  stays under 250 lines; every number in a log or post draft exists in the results JSON). Run them 3 times when a
  prompt changes; a worse prompt does not go in (lab 7).
- **A golden project:** lab 1 is already built by hand, so the agency's version of lab 1 is compared with it.
- **Health numbers per project:** cost, time, QA rounds, and the share of each draft I changed. If the brain
  works, that share goes down from project to project.

## 11. What each step teaches me

| Step | Multi-agent pattern | Lab it repeats |
| --- | --- | --- |
| Lead in code, stages in order | prompt chaining; workflow before agent | 4 |
| Handoffs as checked JSON files | structured handoffs | 1, 2 |
| Engineer with tools and a step limit | the tool loop and stop rules | 3 |
| Engineer ⇄ QA, max 2 rounds | evaluator-optimizer | 7 |
| Re-run one stage from `state.json` | idempotent stages | 5 |
| Two founder checks, sandbox, no send | approval gates, least privilege | 6 |
| Brain and Librarian | memory that is written and reviewed | 8 |
| Ledger and run files | traces: why did the agency do that? | 9 |
| Later: the lead becomes a model | orchestrator-workers vs a workflow, measured | 12 |

## 12. First milestone: project 01 (lab 1) end to end

Build order, one small PR each:

1. `company-map.md`, the brain folders, `llm.py` and the cost ledger.
2. Runner, Analyst and Writer, used on lab 1 as it is today (it needs its first real run anyway).
3. Architect, Engineer and QA: rebuild lab 1 from its brief in a sandbox, and compare it with the hand-built one.
4. Librarian and the corrections loop.
5. The role evals.

Done when: lab 1's brief goes in, and out come a spec I checked, a lab that passes the stub test, one real run with
its cost, a `LOG.md` entry and post drafts, `retro.md`, and at least one brain change I approved, all within the
cap, with every number traced to the results.

## 13. Post angle (draft, mine to rewrite)

"I run a pretend AI agency. The client is made up, the staff are agents I built myself, and every fix I make goes
into one brain, so the next project needs fewer fixes. Here is what project 1 cost and how many of the drafts I
had to change." Numbers only from the run. The post says plainly which code AI coding agents wrote.
