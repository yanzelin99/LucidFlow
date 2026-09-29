# Ask-First Workflow · Don't Guess, Ask First

> A request-handling flow that forces AI to "clarify first, execute later":
> put the cheapest clarification up front to eliminate the most expensive rework.
> `AGENTS.md` is the drop-in optimized rule (≈841 tokens, Chinese canonical);
> `AGENTS_EN.md` is the English translation. 中文版见 [README.md](./README.md).

## Why

When AI fails, it is usually not incompetence — it is guessing under incomplete
information: inventing a default value, betting on the most likely interpretation,
trying execution privately and asking only after something breaks.
A correct guess saves one question; a wrong guess wastes a full execution round —
and may mutate state or burn huge amounts of tokens.
This flow forces the "asking" up front: stop when intent is unclear, ask everything
at once, then execute with an unambiguous understanding.

## Before: what went wrong without it

- **Blind guessing**: object, quantity or format unstated → AI invents defaults and builds the wrong thing.
- **Act-first-ask-later**: private trial execution, confirmation only after breakage; state already mutated, rework required.
- **Probability gambling**: one sentence, two readings → AI unilaterally picks the "most likely" one and answers the wrong question.
- **Wasteful burn**: target not even specified, yet a full-repo search runs first — a token and time black hole.
- **Fragmented interrogation**: one question per round, five rounds later the picture is still incomplete.

## After: what it fixes

- **Ask everything at once**: the clarification phase lists ALL pending questions; one answered round locks the picture. No fragmented follow-ups.
- **Doubt means ask**: the clarity check routes anything doubtful to "must clarify" — no more gambling on probabilities.
- **Circuit breaker**: about to violate a prohibition at any step → stop immediately, go clarify. Ambiguity never reaches execution.
- **Reviewable and traceable**: replies go back through the Step 2 re-check; only a state satisfying "direct execution" counts as locked.
- **Token-cheap**: optimized version ≈841 tokens (down from 1151), affordable on every request.

## The full rule (flow-oriented canonical text, Chinese)

See the code block in [README.md](./README.md), or use [AGENTS_EN.md](./AGENTS_EN.md) directly.

## Quick start

```bash
# Copy the rule as your global rule (Chinese canonical)
cp AGENTS.md ~/.config/opencode/AGENTS.md
# Or drop the IDE packs into your project: .cursor/rules/, .windsurf/rules/, snippets/
```

## Contents

- `AGENTS.md` — rule text, Chinese canonical (≈841 tokens / cl100k)
- `AGENTS_EN.md` — rule text, English translation
- `examples/` — Bad Case vs Good Case dialogues
- `.cursor/rules/`, `.windsurf/rules/`, `snippets/` — drop-in IDE packs
- `LICENSE` — MIT

## Why "Ask-First"?

AI errors are rarely stupidity; they are guesses. Asking first resolves the most
ambiguity at the lowest cost.
