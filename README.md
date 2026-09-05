# Personal Web Avatar

A personal chat avatar that introduces my background, answers recruiter questions from my CV, shares technical perspectives I have written down, and — for me only — recalls private master notes. No visitor signup; spend is capped with owner-minted URL keys.

**Source code is private.** This public repo is the project overview only.

| Role | Repo | Visibility |
| --- | --- | --- |
| Overview (this) | [`personal-avatar`](https://github.com/rliu6915/personal-avatar) | Public |
| UI | [`personal-avatar-frontend`](https://github.com/rliu6915/personal-avatar-frontend) | Private |
| API | [`personal-avatar-backend`](https://github.com/rliu6915/personal-avatar-backend) | Private |

## What it does

- Introduces me with a static photo (v1 — no talking head)
- Answers recruiter questions from a structured CV (no invented dates/titles/skills)
- Shares public technical perspectives I endorse
- Recalls private Notion / master notes when opened with a private key
- Caps token spend without accounts

## Architecture

```text
Recruiter / me
      │
      ▼
Vite + React 19 UI  ──/api──►  Express (Node)
  react-router-dom              GLM-4.7 stream
  /  /c/:key  /admin            SQLite budgets + history
                                Frozen system prefixes
```

- **Frontend:** React 19, Vite, `react-router-dom`
- **Backend:** Express, TypeScript, GLM-4.7 (Z.AI), SQLite (`node:sqlite`)
- **Hosting target:** Railway (API + volume for SQLite; frontend as static site)

### Knowledge layers (not one prompt)

| Layer | Contents | Who |
| --- | --- | --- |
| Public facts | Structured CV / bio | Recruiter + me |
| Public views | Endorsed technical perspectives | Recruiter + me |
| Private notes | Notion master document | Me only |

Two frozen system-prompt prefixes keep GLM automatic prompt caching effective and prevent private notes leaking through a forwarded recruiter link.

### Access without registration

I mint capability URLs (`/c/:key`):

- **public / demo** — CV + perspectives, small budget
- **private** — above + master notes

Each key has a budget, optional expiry, and revoke. Keys are never stuffed into the system prompt (that would break cache hits).

## Why this shape

- Stuffing a bounded personal corpus into the system prompt beats RAG for CV accuracy
- Unique URLs beat signup for recruiters
- A thin `runTurn` adapter leaves room for LangGraph later (search, scheduling) without shipping an agent framework on day one

## Status

v1 scaffolding is in the private repos: chat UI, key mint/admin, corpus snapshots, streaming chat, Railway volume notes. Fill CV / perspectives / Notion, set `GLM_API_KEY`, then share recruiter links.

## License

Documentation only. Implementation remains private.
