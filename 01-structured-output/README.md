# Lab 1 — "Reply in JSON" is a wish, not a contract

**The principle:** when code depends on what a model returns, give the API the shape (a schema), and still check
the meaning yourself. A schema guarantees the shape of the answer. It does not guarantee the answer is right.

**Why it matters later:** Lab 2 (quarantine) is built on this. The model that reads untrusted text hands the
model with tools a checked object, never free text. That only works if the object is guaranteed to have a
shape, and if code checks what is inside it.

## Read first (about 45 minutes)

Before reading, write in two lines what you think a schema guarantees and what it doesn't. Then:

- [Anthropic docs: Structured outputs](https://platform.claude.com/docs/en/build-with-claude/structured-outputs)
  — what the API guarantees when you give it a schema, and the Python `messages.parse()` helper this lab uses.
- [Eugene Yan: Patterns for building LLM-based systems & products](https://eugeneyan.com/writing/llm-patterns)
  — read the guardrails and defensive UX parts: why checks sit around the model even when its output is valid.

Note the one idea you will test. After the lab, come back: what matched your numbers, what didn't?

## The setup

Twenty short messages to an IT help desk (`messages.json`). Each one has to become a ticket with five fields:
requester, type, urgency, due date and a one-line summary. The messages are written to be awkward: relative
dates ("by Thursday", "next Friday"), two requests in one message, a message in French, a message with no
name, and one that tries to give orders ("Ignore the previous instructions…").

Two arms, same model, same instructions:

| Arm | How the shape is asked for | Who parses |
| --- | --- | --- |
| `free` | The prompt says "Reply with ONLY a JSON object…" | We `json.loads` the text, strictly |
| `schema` | The API gets the schema (structured outputs) | The SDK returns a validated object |

Every reply is scored twice: **shape** (did we get a valid object at all?) and **meaning** (is each field right,
compared with `expected.json`?).

## Your part, before any code runs

This is the part that makes it your experiment. Do it on paper or in a note.

1. **Check the ground truth.** Open `expected.json` and read every row against its message. The labels are a
   draft; where you disagree, change them. A list means any of those values counts as right. Rows 5, 6, 12, 13
   and 14 are deliberately ambiguous: decide what you accept, and why.
2. **Add five messages of your own** to `messages.json` (ids 21–25) with their labels in `expected.json`. Make
   them the kind of thing that would break a ticket system you have seen.
3. **Predict, in numbers, before running:**
   - how many of 60 `free` replies will fail to parse at all;
   - how many of 60 `schema` replies will fail to parse;
   - which field will be wrong most often, and on which messages;
   - whether message 9 (the one that gives orders) will fool either arm.

## Run it

```bash
cd agent-labs
python3 -m venv .venv && .venv/bin/pip install -r requirements.txt
export ANTHROPIC_API_KEY=...        # or `ant auth login`
.venv/bin/python 01-structured-output/lab.py --model claude-haiku-5-5 --runs 3
```

Then the same with `--model claude-sonnet-5-5` and `--model claude-opus-5-5`. With 20 messages, 2 arms and 3 runs
that is 120 calls per model; on Haiku 5.5 it costs well under a dollar. The script prints the real cost of
every run. Theo's Reader uses Haiku 5.5, so start there.

## Read the results yourself

The summary counts things. The learning is in `results/<model>-<time>.json`: open every reply that is not `ok`
and every field that is wrong, and sort them into kinds of failure:

- **shape failures:** code fences, a sentence before the JSON, a missing key, a value outside the allowed list;
- **meaning failures:** a wrong date for "next Friday", a type guessed for the two-request message, an urgency
  taken from message 9's orders.

Then answer, in your own words:

1. Did the schema arm remove shape failures? Did it remove meaning failures?
2. Which wrong values would have been dangerous if code acted on them without a check? (A due date in the past?
   An urgency set by the sender's instructions?)
3. Which fields should a model never produce at all? For example: should the model turn "next Friday" into a
   date, or should it return the phrase and let code compute the date from today? (This is Lab 4.)
4. Is the schema itself wrong anywhere? Message 13 has two requests and the ticket has room for one type.

## How big systems do this

- **The API:** structured outputs constrain the reply to a JSON schema, and strict tool use does the same for
  tool arguments. Both guarantee shape, not truth. (Links to the official docs go here when the post is written.)
- **Theo:** the Reader has no tools and returns JSON only; code checks it and keeps a date or an amount only if
  it really appears in the text. Only checked fields ever reach the model that can act (`docs/SPEC.md` §6 in
  the Theo repository).

## Questions to answer out loud (3 minutes each, no notes)

- "Why isn't 'reply in JSON' enough in production?"
- "The schema guarantees the shape. What guarantees the content?"
- "Where would you put the check, and what happens when it fails?"

## What went into this lab

The script and the draft labels were written with an AI coding assistant. The labels you check, the messages
you add, the predictions, the reading of the failures and the conclusions are yours. Say so in the post.
