# Open Lingo

An open-source, free language-learning platform. Structured lessons, spaced-repetition flashcards, community-built content, and the practice tools to make all of it stick. Funded by light, opt-in ads — no required subscriptions, no walled gardens, no AI tutor selling you something.

> **Status:** MVP launch prep — pre-public beta. Core learn loop is shipped and tested; community deck browse/subscribe is live; social, finance ops, and second-language scale-out are in flight. See each repo for current state.

---

## Repositories

| Repo | What it is | Stack |
|---|---|---|
| [`lingo`](https://github.com/open-lingo/lingo) | Web app — the learner-facing SPA | React 19 + TypeScript + Vite + Tailwind |
| [`lingo-core`](https://github.com/open-lingo/lingo-core) | Core API — users, lessons, SRS sync, decks, community | FastAPI on AWS Lambda (Mangum) + DynamoDB |
| [`lingo-ops`](https://github.com/open-lingo/lingo-ops) | Ops/finance API — admin-only cost + revenue + job telemetry | FastAPI on AWS Lambda + DynamoDB + Cost Explorer |
| [`lingo-infra`](https://github.com/open-lingo/lingo-infra) | Terraform — every AWS resource the platform runs on | Terraform + AWS |

---

## What works today

- **Structured courses** — Lesson-based pathways with progress tracking. Japanese is the deepest (M1–M27 published); Korean is next.
- **Spaced repetition (FSRS-6)** — Modal recognition + production sub-states per card. Local-first with delta sync.
- **Practice surfaces** — Alphabet, particles, kanji, conjugation tables, sentence builder.
- **Community decks** — Browse, subscribe, study. Upvotes, public profiles at `/u/:username`.
- **Operations dashboard** — Admin view of AWS costs (per-service + per-domain), revenue placeholders, batch-job telemetry.
- **Cost-allocation tagging** — Every Dynamo table tagged by `Domain`. Cost Explorer rolls up by feature so the maintainer can see where the money actually goes.

## Coming next

- **Social** — Friends, friend-only leaderboards, public profiles. Sibling to community, single graph across all learning languages.
- **More content** — Korean to JLPT-N5 parity with Japanese.
- **Ads** — AdSense integration enabled at launch. Always opt-in, always disclosed.

---

## Mission and economics

The whole point: keep a useful free thing online without it becoming a cost burden for the maintainer.

- **Free, ad-supported.** Banner ads + opt-in rewarded gates on deck downloads. No subscription required to learn.
- **Optional supporter tiers ($1 / $5 / mo)** when ready — buy a coffee, fund N free learners. The Patron-tier counter is honest math, not marketing.
- **AWS cost per active user:** ~$0.007/mo (Lambda + DynamoDB + CloudFront, no VPC, ARM compute).
- **Survival target:** $400–800/mo to cover the maintainer's couple hours a week. Anything above is reinvestment buffer.

Full math in [`lingo/docs/ECONOMICS.md`](https://github.com/open-lingo/lingo/blob/main/docs/ECONOMICS.md).

---

## Quick start (local dev)

```bash
# All four repos cloned side-by-side
git clone https://github.com/open-lingo/lingo.git
git clone https://github.com/open-lingo/lingo-core.git
git clone https://github.com/open-lingo/lingo-ops.git
git clone https://github.com/open-lingo/lingo-infra.git

# Top-level Makefile orchestrates the dev loop (in the parent dir)
make install      # deps for all three services
make dev          # runs lingo-core (8000) + lingo-ops (8001) + lingo (5173) in parallel
make test         # all tests, all repos
```

Each repo has its own README with deeper setup details + an `.env.example` to copy.

---

## Architecture at a glance

```
                    ┌──────────────┐
   openlingoapp.com │   lingo SPA  │ ────────────► Auth0
                    └──────┬───────┘
                           │
              ┌────────────┼────────────┐
              ▼                         ▼
    ┌──────────────────┐     ┌─────────────────────┐
    │ lingo-core API   │     │ lingo-ops API       │
    │ (user-facing)    │     │ (admin-only)        │
    └────────┬─────────┘     └─────────┬───────────┘
             │                         │
             ▼                         ▼
    ┌──────────────────┐     ┌─────────────────────┐
    │ DynamoDB tables  │     │ Cost Explorer +     │
    │ (users, srs,     │     │ Stripe + AdSense    │
    │  decks, progress,│     │ (post-launch)       │
    │  social, votes)  │     └─────────────────────┘
    └──────────────────┘
```

Both APIs deploy as Python 3.13 Lambdas on ARM64 via Mangum, fronted by Lambda Function URLs (no API Gateway → no per-request fee). All infra is declared in `lingo-infra`.

---

## Contributing

Issues and PRs are welcome in each repo:

- **Content / lessons** → `lingo` (most lesson data lives under `src/features/lesson/data/`)
- **API or schema changes** → `lingo-core` or `lingo-ops` (Protocols + SQLite-first repos)
- **AWS resources** → `lingo-infra` (Terraform `validate` runs in CI on every PR)
- **Bug reports** → file in whichever repo the bug is most clearly in; we'll triage

Pre-launch, the maintainer is mostly looking for: lesson content review, accessibility feedback, and bug reports from real learners.

---

## License

Each repo is open source — see the `LICENSE` file inside each one for the specific license (the apps and infra ship under permissive licenses; user-generated content is governed separately per the project's content policy).

---

Built with [Claude Code](https://claude.com/claude-code) + a maintainer with too much enthusiasm and not enough time.
