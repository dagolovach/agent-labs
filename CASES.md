# Labs as client cases

Every lab is told as a **made-up client case**: a client has a problem, here are their inputs and limits, this
is how I would solve it, and this is what the lab measured. The client is invented and always labeled as
invented. The numbers are not: they come only from the lab's own runs.

Why this shape: it is how applied AI work really starts (a customer, a problem, a baseline, success agreed before
building), and it lets a reader judge the solution, not only the technique.

The briefs below are **drafts for me to correct**, like the network analogies. The solution, the conclusions and
the analogy in a post are mine.

## The template

Use it for each lab's README ("The setup") and for its post.

```
> A made-up case. The company and its people are invented; the numbers come from my lab's own runs.

**The client:** who they are, in one line.
**The problem:** what goes wrong today, and what it costs them (in their words, no invented figures).
**Inputs and limits:** what data they have, what the system may and may not do, budget or time limits.
**What success looks like:** agreed before building: the baseline today and the measure that says "better".
**Rule, agent or person:** which parts stay plain code, which need a model, which stay with a person.
**Model choice:** which model for which step, backed by the evals and the cost, and how to change it.

**How I would solve it:** the naive version, why it breaks, the fix. (Mine.)
**The result:** the lab's numbers: shape or correctness, cost, time, pass^k. Missing numbers are written as missing.
**The network analogy:** the same principle in network operations. (Mine.)
**What I learned:** two or three lines. (Mine.)
**Your turn:** one question to readers, for example "How would you solve it?" or "Where would you draw the line?"
```

Rules for a brief:

- Keep the client close to a world I know: networks and network operations, IT support, a skating rink. A
  believable client makes the post believable.
- No real company, person or child. Nothing from my employer.
- No invented numbers about the client, such as money lost or hours wasted. Counts that are part of the lab's data
  (20 messages, 5 fields) are fine.
- Plain English, short sentences, no idioms.
- No lock-in to one vendor. Labs are built mainly on Claude, behind a thin model layer, so the model is one flag.
  A case says which model it would use for which step, and why, from the evals and the cost (case 17 measures
  it). Several agents only where they fit (lab 12).
- Each brief names the skill it shows and the words for its post: the words people use when they search for that
  skill. The post uses them in plain sentences, never as a list of tags.

## The cases, lab by lab

### Lab 1 — Structured output

- **Client:** the IT help desk of a small group of clinics.
- **Problem:** requests arrive as free-text emails; a script turns them into tickets. One bad reply from the model
  breaks the night queue, and a valid but wrong field (a wrong date, a wrong urgency) sends a ticket to the wrong
  place.
- **Inputs:** 20 real-looking messages (relative dates, two requests in one, French, no name, one that gives
  orders) and a ticket with five fields.
- **Success:** every reply becomes a valid ticket, and code catches a wrong field before it reaches the queue.
- **Skill it shows:** guardrails around model output, so it can feed a business system safely.
- **Words for the post:** "guardrails", "LLM behavior and failure modes", "integrating with enterprise systems",
  "prompt engineering"

### Lab 2 — Quarantine

- **Client:** a managed network services provider whose NOC agent reads customer emails and vendor advisories,
  and can open change tickets and run show commands.
- **Problem:** any email can carry text written to make the agent act.
- **Inputs:** a mix of normal and hostile messages (instructions hidden in an email, a poisoned advisory) and the
  agent's tools.
- **Success:** no hostile message leads to a tool call, normal requests are still handled; the attack-success
  rate before and after the fix.
- **Skill it shows:** agent security: untrusted text never drives the tools.
- **Words for the post:** "prompt injection", "data leakage", "guardrails", "AI security"

### Lab 3 — Tools and the loop

- **Client:** a small ISP NOC that wants an agent to work "customer can't reach site X" tickets with read-only
  tools (ping, traceroute, interface status) on a simulated network.
- **Problem:** the agent can loop, repeat the same check, and spend money with no end.
- **Inputs:** a simulated network with known faults, the tool set, a step limit and a budget per ticket.
- **Success:** it finds the fault or stops by rule inside the limits, and logs why it stopped.
- **Skill it shows:** agent design with tools, and stop rules that hold.
- **Words for the post:** "tool calling", "tool-use agents", "agent development", "agentic workflows", "AIOps"

### Lab 4 — The model proposes, code decides

- **Client:** a skating rink that sells private ice time to coaches.
- **Problem:** coaches book by message ("two 30-minute lessons next Tue and Thu after 4"); when the model works
  out the dates, the count or the total, it is sometimes wrong.
- **Inputs:** booking messages, the rink calendar and the price list.
- **Success:** the model only extracts; code computes dates, counts and totals. Sort the tasks into rule, agent
  or person, run the rule ones both as plain code and through the model, compare correctness, cost and time.
- **Skill it shows:** scoping: which part is code, which part needs a model; checks in code.
- **Words for the post:** "use-case scoping", "orchestration patterns", "verification", "LLM failure modes"

### Lab 5 — Retries and idempotency

- **Client:** a network team whose agent opens change requests in the ticket system through an API that
  sometimes times out.
- **Problem:** after a timeout the agent retries, and the same change gets scheduled twice.
- **Inputs:** a fake ticket API that drops some replies, and a set of change requests.
- **Success:** zero duplicate changes over many runs with failures injected.
- **Skill it shows:** reliability when an agent writes into another system.
- **Words for the post:** "production applications", "reliability", "integrating with enterprise systems"

### Lab 6 — Least privilege

- **Client:** a campus network team that wants an assistant for on-call engineers, able to run commands on
  (simulated) switches.
- **Problem:** decide which actions run alone, which wait for a person, and which never run.
- **Inputs:** requests from junior engineers, some risky and some hostile, and the three tiers auto / confirm /
  never.
- **Success:** no "never" action runs, every "confirm" action waits for a person, safe ones don't bother anyone;
  the attack-success rate before and after.
- **Skill it shows:** guardrails on actions: what runs alone, what waits for a person, what never runs.
- **Words for the post:** "least-privilege", "approval gates", "policy enforcement at tool boundaries", "guardrails"

### Lab 7 — Evals

- **Client:** the clinic help desk from lab 1 wants a new prompt and a cheaper model, and asks: how do we know
  the new version is not worse?
- **Problem:** one good run proves nothing; a change can pass today and fail tomorrow.
- **Inputs:** a fixed test set of 30+ cases and the current version as the baseline.
- **Success:** pass@k and pass^k for both versions, and a CI check that blocks the worse one.
- **Skill it shows:** evals, and a check that stops a worse version before it ships.
- **Words for the post:** "evaluation frameworks", "agent evaluation", "evaluation pipelines", "replayable incident
  harnesses", "CI/CD"

### Lab 8 — Context, memory and caching

- **Client:** a NOC where an agent helps through a long incident across a whole shift.
- **Problem:** as the incident grows, the agent gets slower and more expensive, and loses the goal.
- **Inputs:** a long incident log and the task, run twice: keep everything vs managed context; with and without
  caching.
- **Success:** tokens, cost and time for each, and whether the agent stays on the goal to the end.
- **Skill it shows:** context design, and what it costs.
- **Words for the post:** "context design", "memory", "cost-per-request", "latency"

### Lab 9 — Logs and traces

- **Client:** a support team whose agent closed a customer's ticket a week ago; the customer asks why.
- **Problem:** without a record of what the agent saw and decided, nobody can answer.
- **Inputs:** agent runs with and without tracing, and a set of "why did it do that?" questions.
- **Success:** each question answered from the trace alone, and how long it took to answer.
- **Skill it shows:** observability for agents.
- **Words for the post:** "tracing", "observability", "LLM Ops", "monitoring"

### Lab 10 — Timeouts and fallbacks

- **Client:** a skating rink whose parents ask an assistant "is the ice open tonight?".
- **Problem:** when the model API is slow or down, parents get nothing.
- **Inputs:** a model wrapper that adds delays and errors, a smaller backup model, and a plain rule that reads
  the schedule.
- **Success:** every question gets an answer or a clear "try later" inside a time limit; success rate and time
  under failure.
- **Skill it shows:** reliability when the model is slow or down.
- **Words for the post:** "reliability", "latency", "monitoring", "production"

### Lab 11 — Retrieval with sources

- **Client:** a network services provider whose engineers ask "which sites use VLAN 210?" or "what is the routing
  policy for customer X?".
- **Problem:** answers from the model's memory sound right and are wrong.
- **Inputs:** an inventory export (a source of truth, like NetBox), runbooks, and questions with known answers,
  some with no answer in the sources.
- **Success:** each answer cites its record, and "not in the source" when it isn't there; correct and cited rates.
- **Skill it shows:** retrieval that cites its record.
- **Words for the post:** "RAG", "RAG-like architectures", "vector databases", "source of truth"

### Lab 12 — Several agents and an orchestrator

- **Client:** an enterprise network team: "add a new branch office" needs a design step, a config step and a
  check step.
- **Problem:** one big agent mixes the steps and skips the check.
- **Inputs:** a change request, config templates and a simulated lab to check against.
- **Success:** one agent vs an orchestrator with workers, compared on correctness, cost and time, with a person
  approving before anything is applied.
- **Skill it shows:** multi-agent design with a person in the loop.
- **Words for the post:** "multi-agent orchestration", "sub-agents", "orchestration patterns", "approval gates"

## Cases beyond the twelve labs

Five more cases cover skills the twelve labs don't show. Three are built like labs; two are write-ups with no code.

### Case 13 — An MCP server over a source of truth

- **Client:** the network services provider from lab 11. Its engineers already use AI tools and want them to read
  the inventory without pasting exports into a chat.
- **Problem:** a pasted export goes out of date, holds more than the question needs, and nobody knows which copy
  an answer came from.
- **Inputs:** an inventory export (sites, devices, VLANs), a read-only MCP server with a few narrow tools, and an
  AI client such as Claude Code.
- **Success:** every answer comes from a tool call to the current record; there is no write tool; a question the
  tools can't answer gets "not available". Compare with pasting the export: correct answers, tokens, time.
- **Skill it shows:** building MCP servers and tools an agent can use safely.
- **Words for the post:** "MCP servers", "MCP tool servers", "tool calling", "agent skills", "source of truth"

### Case 14 — Discovery and a pilot plan (a write-up)

- **Client:** the IT operations team of a mid-size company. The ask is vague: "we want AI for our tickets".
- **Problem:** building before scoping is how pilots fail; nobody has agreed what "better" means.
- **Inputs:** a made-up description of their ticket flow, the questions to map a workflow, and the rule / agent /
  person sort from lab 4.
- **The write-up:** discovery notes, what stays code, what needs a model, what stays with a person; success
  measures with today's baseline (marked missing until measured); a 30-day pilot plan; the risks.
- **Success:** a reader could start the pilot from the page.
- **Skill it shows:** technical discovery and scoping with a customer.
- **Words for the post:** "technical discovery", "use-case scoping", "success criteria", "path to production",
  "PoC"

### Case 15 — A written cloud architecture (a write-up)

- **Client:** the small ISP NOC from lab 3. It wants the ticket agent for a team of 50 engineers.
- **Problem:** a lab on one laptop is not a service: who can use it, where tools run, what it costs, what happens
  when it fails.
- **Inputs:** labs 3, 6, 9 and 10 and their numbers; AWS as the main cloud, with a short note on a Cloudflare
  Workers version.
- **The write-up:** one diagram and one page: data flow, identity and secrets, where the tools run, guardrails,
  logs and traces, cost per request (from lab numbers only), failure modes and the fallback, how it grows.
- **Success:** every box has a reason, and every number comes from a lab or is written as missing.
- **Skill it shows:** cloud architecture and integration on paper.
- **Words for the post:** "scalable cloud architectures", "reference architecture", "integrating with enterprise
  systems", "Amazon Bedrock", "AgentCore"

### Case 16 — One agent in production

- **Client:** the same NOC. The pilot from cases 14 and 15 goes live.
- **Problem:** a demo that works once is not a system that works every day.
- **Inputs:** one agent built from the labs (tools, retrieval, the MCP server), running on the case 15 design
  with simulated tickets for a few weeks.
- **Success:** the eval suite runs in CI and blocks a worse version; traces answer "why did it do that?";
  permission tiers hold; pass^k, cost per ticket and time are tracked over the weeks.
- **Skill it shows:** an AI agent shipped and run in production, with evals.
- **Words for the post:** "production LLM agents", "evaluation frameworks", "shipping production applications",
  "monitoring", "guardrails"
- **Honest line:** I designed it; AI agents wrote most of the code under my direction.

### Case 17 — Same evals, other models

- **Client:** the clinic help desk from labs 1 and 7. Their cloud team offers several models, and finance asks:
  do we pay for the best model on every ticket?
- **Problem:** a model is often picked by habit or by a benchmark, not by the client's own test set. The client
  also does not want to be locked to one vendor.
- **Inputs:** the agent built mainly on Claude, behind a thin model layer (one function that sends the prompt and
  returns the reply and its usage, so changing the model is one flag); lab 7's fixed test set; two or three other
  models, for example through Amazon Bedrock (one client, many models).
- **Success:** the same evals on every model: pass^k, cost per ticket and time per ticket. The result is a short
  table: which model for which job (for example a small model for routing, a big one for hard cases), and what
  breaks when you change it.
- **Skill it shows:** choosing a model by evals, with cost and speed in view, and no lock-in to one vendor.
- **Words for the post:** "model selection", "evaluation frameworks", "latency/cost", "Amazon Bedrock",
  "multi-model"
- **Network analogy (draft, mine to correct):** a multi-vendor bake-off. Before you buy routers from a second
  vendor, you run the same test plan on both boxes and compare. You don't trust the datasheet.
- **Post idea (draft):** "I ran the same 30 tests on three models. The cheapest one passed [number]. Here is
  which model I would use for which job." The numbers come only from the run.

## Order

The labs go in this order: 1, 2, 3, 7, 4, 5, 6, 8, 9, 10, 11, 12. Lab 7 (evals) comes right after lab 3 (my
decision, 2026-11-20): evals are the skill asked for most, and labs 1 to 3 give lab 7 something to test. Case 17
comes right after lab 7, since it reuses lab 7's test set. Case 13 starts small (read-only, one tool) as a side
study and is finished after lab 6. Cases 14 and 15 are write-ups done next to the labs. Case 16 comes last, after
labs 7 and 9 and case 13.
