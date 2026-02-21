# Open Lingo

An open-source language learning platform. Community-built content, spaced repetition flashcards, structured lessons, and practice tools — free forever.

## Repositories

| Repo | Description |
|---|---|
| [`lingo`](./lingo) | React frontend — the web app |
| [`lingo-core`](./lingo-core) | FastAPI backend — REST API, auth, SRS sync, deck management |

## Features

- **Structured courses** — Lesson-based learning with progress tracking
- **Spaced repetition (SRS)** — SM-2 flashcard review with local-first sync
- **Practice tools** — Particles, alphabet, kanji, stories, videos
- **Community content** — Browse, subscribe to, and create community decks
- **Creator studio** — Deck editor for community contributors
- **Forum** — Discuss content and ask questions

## Languages

Korean and Japanese have full content support. Additional languages are planned.

## Quick start

```bash
# Backend
cd lingo-core
pip install -e ".[dev]" && cp .env.example .env
uvicorn app.main:app --reload   # http://localhost:8000

# Frontend (separate terminal)
cd lingo
npm install && cp .env.example .env
npm run dev                     # http://localhost:5173
```

See each repo's README for full setup instructions.

## Tech stack

**Frontend:** React 19, TypeScript, Vite, Tailwind CSS, Auth0, TanStack Query  
**Backend:** Python 3.13, FastAPI, SQLite (dev) / DynamoDB (prod), Auth0 JWT

## Contributing

Content, bug fixes, and new features are all welcome. Open an issue or pull request in the relevant repo.
