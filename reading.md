# Reading list, lab by lab

Each lab starts with reading: one or two sources that explain the principle, then the lab tests it, then the
post. Links were checked on 2026-11-16. Later labs get their reading fixed when the lab is built.

**The backbone book (optional, for depth):** Chip Huyen, *AI Engineering: Building Applications with Foundation
Models* (O'Reilly, 2024) — https://www.oreilly.com/library/view/ai-engineering/9781098166298/

| Lab | Read first | Why this one |
| --- | --- | --- |
| 1. Structured output | [Anthropic docs: Structured outputs](https://platform.claude.com/docs/en/build-with-claude/structured-outputs) · [Eugene Yan: Patterns for building LLM-based systems & products](https://eugeneyan.com/writing/llm-patterns) (the guardrails and defensive UX parts) | What the API guarantees, and why checks sit around the model anyway |
| 2. Quarantine | [Simon Willison: The Dual LLM pattern](https://simonwillison.net/2023/Apr/25/dual-llm-pattern/) · [Design Patterns for Securing LLM Agents against Prompt Injections](https://arxiv.org/abs/2506.08837) (Willison's [summary](https://simonwillison.net/2025/Jun/13/prompt-injection-design-patterns/)) | The original idea, then the catalogue of patterns and their limits |
| 2, going further | [CaMeL: Defeating Prompt Injections by Design](https://arxiv.org/abs/2503.18813) (Willison's [summary](https://simonwillison.net/2025/Apr/11/camel/)) · [Delimiters won't save you](https://simonwillison.net/2023/May/11/delimiters-wont-save-you/) | Where the dual-LLM split is not enough; why "just wrap the text" fails |
| 3. Tools and the loop | [Anthropic: Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) · [Anthropic: Writing effective tools for agents](https://www.anthropic.com/engineering/writing-tools-for-agents) | When you need an agent at all, and how tools should look |
| 4. The model proposes, code decides | [12-Factor Agents](https://github.com/humanlayer/12-factor-agents) (own your control flow, own your context) | Code, not the model, owns the decisions |
| 5. Retries and idempotency | [Stripe: Designing robust and predictable APIs with idempotency](https://stripe.com/blog/idempotency) | The classic on idempotency keys and retries |
| 6. Least privilege | [Simon Willison: The lethal trifecta for AI agents](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/) · [OWASP LLM01: Prompt injection](https://owasp.org/www-project-top-10-for-large-language-model-applications/2_0_vulns/LLM01_PromptInjection.html) | Private data + untrusted content + a way out = danger; remove one leg |
| 7. Evals | [Hamel Husain: Your AI product needs evals](https://hamel.dev/blog/posts/evals/) · [Anthropic: Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) | How teams actually know their system works |
| 8. Context, memory and caching | to be fixed when the lab is built (Anthropic docs on context and prompt caching) | |
| 9. Logs and traces | Hamel Husain, the traces part of the evals post above; more when the lab is built | |
| 10. Timeouts and fallbacks | to be fixed when the lab is built | |
| 11. Retrieval with sources | Eugene Yan's patterns post (the RAG part); *AI Engineering*, the RAG chapter | |
| 12. Several agents | Anthropic's *Building effective agents* (orchestrator-workers) | |

## How to read

1. **Before reading, 5 minutes:** write down what you think the answer is. Wrong guesses are the point.
2. **Read** the "Read first" sources (about 45–60 minutes). Note the one idea you will test in the lab.
3. **After the lab:** go back to the source. What matched your numbers? What did not?
4. **In the post:** quote and link the source in the "how others do it" part, in your own words.
