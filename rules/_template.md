# New rule fragment — template

<!--
How to add a rule:

1. Copy this file to rules/NN-topic.md using the next free number in the right
   range:
     0x  header/overview        2x  language-agnostic style
     3x  backend/database       4x  frontend
     5x  testing                6x  git
     7x  docs/process           8x  security
     9x  deploy/platform
2. Write the section. Rules below.
3. Wire it in sync.sh: add the filename to the unconditional `frags` list, or
   behind a stack token (`fastify`, `prisma`, `supabase`, `react`, `vercel`)
   or a scoped dir (`backend/`, `api/`, `frontend/`). For a NEW token, also add
   detection in detect_stack() and document the signal in README.md.
4. Bump VERSION and log the rule in CHANGELOG.md.
5. ./sync.sh --all to roll it out.

Fragment writing rules:
- One `## Section Title` per file. Start with it directly (no # title, no
  banner — 00-header.md provides that).
- Write rules as footguns-with-motivation ("X happened, so do Y") rather than
  bare mandates when the reason isn't obvious.
- Invariants and conventions only. No in-progress specs, no status, no dates
  that go stale — those rot and mislead agents.
- Keep it terse. Every line costs context tokens in every agent session.
-->

## Section Title

- Rule 1
- Rule 2
