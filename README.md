<h1 align="center">LucidFlow</h1>

<p align="center">Don't guess, ask first — a request-handling flow that forces AI to clarify before executing.</p>

<p align="center">
  <a href="https://github.com/yanzelin99/LucidFlow/releases/tag/v1.0.0"><img src="https://img.shields.io/github/v/release/yanzelin99/LucidFlow" alt="release" /></a>
  <a href="./LICENSE"><img src="https://img.shields.io/github/license/yanzelin99/LucidFlow" alt="license" /></a>
</p>

<p align="center">English | <a href="./README_ZH-CN.md">简体中文</a></p>

This repo's `AGENTS.md` is the drop-in optimized rule (English); `AGENTS_CN.md` is the Chinese canonical version (≈841 tokens measured on it)

## Why

When AI fails, it is usually not incompetence — it is guessing under incomplete information: inventing a default value, betting on the most likely interpretation, trying execution privately and asking only after something breaks.
A correct guess saves one question; a wrong guess wastes a full execution round — and may mutate state or burn huge amounts of tokens.
This flow puts "asking" first: stop when intent is unclear, ask everything at once, then execute with an unambiguous understanding.

## Problems before this flow

- **Blind-guessed parameters**: object, quantity, format unstated → AI invents defaults and builds the wrong thing.
- **Act-first-ask-later**: private trial execution, confirmation only after breakage; state already mutated, rework required.
- **Probability gambling**: one sentence, two readings → AI unilaterally picks the "most likely" one and answers the wrong question.
- **Wasteful burn**: target not even specified, yet a full-scope search runs first — a token and time black hole.
- **Fragmented interrogation**: one question per round; five rounds later the picture is still incomplete.

## Problems solved after adopting it

- **Ask everything at once**: the clarification phase lists ALL pending questions; one answered round locks the picture. No fragmented follow-ups.
- **Doubt means ask**: the clarity check routes anything doubtful to "must clarify" — no more gambling on probabilities.
- **Circuit breaker**: about to violate a prohibition at any step → stop immediately, go clarify. Ambiguity never reaches execution.
- **Reviewable and traceable**: replies go back through the Step 2 re-check; only a state satisfying "direct execution" counts as locked.
- **Token-cheap**: optimized version 841 tokens (was 1151), affordable on every request.

## Rule flow (illustration of the flow only — do not copy and use)

```text
[User request input]
  │
  ├─► [Exception check] Is it a clear-cut pure knowledge Q&A / chitchat?
  │     │
  │     ├─► Yes (no execution risk) ───► [Answer directly] (flow ends)
  │     │
  │     └─► No (involves state change / tool execution / external action)
  │           │
  │           ▼
  │   [Step 1: Intent parsing] (extract 5 elements)
  │     ├─ Goal: what outcome the user wants
  │     ├─ Object: the concrete target of the operation
  │     ├─ Parameters: quantity, format, scope, time, etc.
  │     ├─ Environment: platform, tools, context
  │     └─ Constraints: limits and prohibitions
  │           │
  │           ▼
  │   [Step 2: Clarity check] (completeness gate)
  │     │
  │     ├─► [Branch A: direct execution]
  │     │     │
  │     │     ├─ All must hold:
  │     │     │    ├─ All 5 elements present (or directly inferable from context)
  │     │     │    ├─ Only one reasonable interpretation
  │     │     │    └─ No ambiguity, no unclear references
  │     │     │
  │     │     └─► Straight to ──► [Step 4: Execute task] ──► [Deliver result]
  │     │
  │     └─► [Branch B: must clarify]
  │           │
  │           ├─ Triggers (any single hit):
  │           │    ├─ Two or more reasonable interpretations
  │           │    ├─ Missing key parameters (object/quantity/format unspecified)
  │           │    ├─ Unclear reference (e.g. "that file" cannot be located)
  │           │    └─ Irreversible operation with unbounded scope
  │           │
  │           ▼
  │   [Step 3: Clarifying questions] (ask first, strict interaction discipline)
  │     │
  │     ├─ Questioning rules:
  │     │    ├─ List ALL pending questions at once (no fragmented follow-ups)
  │     │    ├─ Short, specific, directly answerable
  │     │    └─ Numbered options when several readings exist (e.g. Option 1 / Option 2)
  │     │
  │     ├─ Defensive constraint:
  │     │    └─ Questions ONLY in this phase: no task execution, no irrelevant output
  │     │
  │     ▼
  │   [Wait for the user's explicit reply and confirmation]
  │     │
  │     └─► User supplies missing elements / picks options
  │           │
  │           ▼
  │   [Step 4: Execute after confirmation]
  │     │
  │     └─► Execute on the locked unambiguous understanding ──► [Deliver result] (flow ends)


─────────────────────────────────────────────────────────────
Prohibited behaviors (circuit breaker on all branches)
─────────────────────────────────────────────────────────────
  ✖ [No blind guessing] Never invent default values for missing parameters and execute
  ✖ [No act-first-ask-later] Never "try execution privately, come back only when something breaks"
  ✖ [No probability gambling] Never unilaterally pick the seemingly most likely option when several exist
  ✖ [No wasteful burn] Never blindly search everything when the target is unspecified (token/time black hole)
```

## Quick start
Copy the text below and send it to your AI tool:

```text
Please fetch AGENTS.md from https://github.com/yanzelin99/LucidFlow/ and install it as my global agent rules. Global rule locations or defined filenames differ per tool — locate where YOUR tool keeps its global rules (config directory, system prompt, or instruction file) and install there. Back up existing ones first; if anything already exists there, ask me before replacing.
```
