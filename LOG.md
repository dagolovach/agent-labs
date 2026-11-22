# Log

Newest first. One entry per working session; posts are written from these entries.

## 2026-11-22 — The lab blueprint

- Done: added `BLUEPRINT.md` (one fixed stack, one folder shape, an 11-step build order, what changes per case
  and what never changes, how HQ uses it) and `lab-template/` (README in the fixed sections, `lab.py` with the
  parts marked `CASE`, example data). New flags in the template: `--stub` (offline test, no cost) and `--effort`.
  Tested offline with `--stub` only.
- Numbers: none (no real run).
- Surprised: nothing yet.
- Takeaway: decide the stack once, so each lab only changes the case.
- Network analogy: not chosen yet (a draft: a golden config template that every new site is built from).
- Hooks: "Every lab I build uses the same stack. Only the client changes." / "I stopped choosing tools for
  each project."

## 2026-11-21 — The agency: plan

- Done: wrote `agency/PLAN.md`, a plan for a multi-agent system I build myself (Messages API, my own prompts) to
  run the labs, as a pretend agency: each lab is a client project, the agents are the staff, a brain in a
  separate private repo keeps every fix. First milestone: lab 1 end to end. No code yet.
- Numbers: none.
- Surprised: nothing yet.
- Takeaway: start with one pipeline in plain code and add agents only where my time goes.
- Network analogy: not chosen yet.
- Hooks: "I run a pretend AI agency. Every fix I make goes into one brain." (Only with numbers from project 1.)

## 2026-11-16 — Reading before every lab

- Done: added `reading.md` (checked sources for labs 1–7, placeholders for 8–12) and a "Read first" section to
  lab 1. The week is now read → your part → lab → post.
- Numbers: none.
- Surprised: nothing yet.
- Takeaway: the reading is the theory; the lab is the test; the post says where they agreed.
- Hooks: none yet.

## 2026-11-16 — Lab 1: structured output

- Done: built the lab: 20 awkward help-desk messages, draft labels for 2026-11-16, two arms (ask for JSON vs
  the API's schema), scoring for shape and for each field, pass^k, cost and time. Scoring tested offline with
  a stub client. Added CLAUDE.md and this log.
- Numbers: not run on a real model yet.
- Surprised: nothing yet.
- Takeaway: a schema guarantees the shape of an answer, never that it is right. (To be confirmed by the run.)
- Hooks: "'Reply in JSON' is a wish, not a contract." / "I asked a model for JSON 60 times. Here is what came
  back." (Only once the numbers exist.)
