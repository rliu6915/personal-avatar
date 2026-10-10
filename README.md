# Personal Web Avatar

A personal chat avatar that introduces my background, answers recruiter questions from a frozen CV corpus, can search the web, and emails me when someone wants to meet. Replies are spoken aloud with a cloned voice.

**Live:** [avatar.lexoralabshk.com](https://avatar.lexoralabshk.com)

Source code is private. This public repo is the project overview only.

| Role | Repo | Visibility |
| --- | --- | --- |
| Overview | [`personal-avatar`](https://github.com/rliu6915/personal-avatar) | Public |
| UI | [`personal-avatar-frontend`](https://github.com/rliu6915/personal-avatar-frontend) | Private |
| API | [`personal-avatar-backend`](https://github.com/rliu6915/personal-avatar-backend) | Private |

## Features

- Google Sign-In for visitors
- Conversational answers from a structured CV and public technical perspectives (no invented facts)
- LangGraph agent with tools
  - **web_search** — external / time-sensitive facts via Z.AI
  - **request_meeting** — emails the owner when a visitor wants to meet
- Voice replies (`glm-tts`) with a cloned voice
- Animated portrait while speaking (blink, gaze, think, mouth)
- Daily visitor message limit; per-user token budgets

## Architecture

```text
Visitor
  │
  ▼
React 19 + Vite SPA  ──/api──►  Express on Railway
  Google OAuth                  LangGraph: llm ↔ tools
  Streaming chat + TTS          Z.AI GLM-4.7 + glm-tts
  AlivePortrait                 SQLite + frozen CV prefixes
```

| Layer | Stack |
| --- | --- |
| Frontend | React 19, Vite, react-router-dom, Web Audio |
| Backend | Express, TypeScript, LangGraph, Z.AI, Resend, SQLite |
| Hosting | Railway (API + SPA same-origin), Cloudflare custom domain |

Knowledge comes from a structured CV and endorsed public perspectives, injected as frozen system-prompt prefixes.

## Status

Production at [avatar.lexoralabshk.com](https://avatar.lexoralabshk.com).

## License

Documentation only. Implementation remains private.
