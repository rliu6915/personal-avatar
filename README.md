# Personal Web Avatar

A personal chat avatar that introduces my background, answers recruiter questions from a frozen CV corpus, can search the web, and emails me when someone wants to meet. Replies are spoken aloud with a cloned voice.

**Live:** [avatar.lexoralabshk.com](https://avatar.lexoralabshk.com)

**Source code is private.** This public repo is the project overview only.

| Role | Repo | Visibility |
| --- | --- | --- |
| Overview (this) | [`personal-avatar`](https://github.com/rliu6915/personal-avatar) | Public |
| UI | [`personal-avatar-frontend`](https://github.com/rliu6915/personal-avatar-frontend) | Private |
| API | [`personal-avatar-backend`](https://github.com/rliu6915/personal-avatar-backend) | Private |

## What it does

- Google Sign-In for visitors (no username/password)
- Conversational answers from a structured CV + public technical perspectives (no invented facts)
- LangGraph agent with tools:
  - **web_search** — time-sensitive / external facts via Z.AI
  - **request_meeting** — emails the owner (Resend) when a visitor wants to meet
- Voice replies (`glm-tts`) with a cloned voice
- Animated portrait (blink, gaze, think, mouth cycle) while speaking
- Daily visitor message limit (10 / Hong Kong day); owners unlimited
- Per-user token budgets in SQLite

## Architecture

```text
Visitor (browser)
      │
      ▼
React 19 + Vite SPA  ──/api──►  Express (Node / Railway)
  Google OAuth redirect           LangGraph: llm ↔ tools
  Streaming chat + TTS playback   Z.AI GLM-4.7 + glm-tts
  AlivePortrait animation         SQLite users / history / budgets
                                  Frozen system prefixes (CV + notes)
```

- **Frontend:** React 19, Vite, `react-router-dom`, Web Audio TTS playback
- **Backend:** Express, TypeScript, LangGraph, Z.AI GLM + TTS, Resend, SQLite (`node:sqlite`)
- **Hosting:** Railway (API + volume for SQLite) serves API and SPA same-origin; Cloudflare custom domain

### Knowledge layers

| Layer | Contents | Who |
| --- | --- | --- |
| Public facts | Structured CV / bio | All signed-in visitors |
| Public views | Endorsed technical perspectives | All signed-in visitors |
| Private notes | Master notes | Owner emails only |

System-prompt prefixes are hashed and frozen so automatic prompt caching stays effective and private notes never leak through a visitor session.

### Access model

- Visitors sign in with Google; chat history and budget are per Google user
- Emails in `OWNER_EMAILS` get the private corpus and skip the daily message cap
- Capability URL keys (`/c/:key`) are legacy; primary path is Google users

## Why this shape

- Stuffing a bounded personal corpus into the system prompt beats RAG for CV accuracy
- Google Sign-In beats anonymous keys for abuse control and continuity across visits
- A thin LangGraph (`llm` ↔ `tools`) adds web search and meeting notify without a heavy agent framework

## Status

Production at [avatar.lexoralabshk.com](https://avatar.lexoralabshk.com): Google auth, LangGraph tools, cloned-voice TTS, daily limits, same-origin SPA on Railway.

## License

Documentation only. Implementation remains private.
