# Personal Web Avatar

A personal chat avatar that introduces my background, answers recruiter questions from a frozen CV corpus, can search the web, and emails me when someone wants to meet. Replies are spoken aloud with a cloned voice.

**Live:** [avatar.lexoralabshk.com](https://avatar.lexoralabshk.com)

## Features

- Google Sign-In
- Answers grounded in a structured CV and public technical perspectives (no invented facts)
- LangGraph tools: **web_search**, **request_meeting**
- Cloned-voice TTS (`glm-tts`)
- Animated portrait while speaking
- Daily visitor limits and per-user token budgets

## Architecture

```text
Visitor → React SPA → Express (Railway)
              │              │
              │              ├─ LangGraph: llm ↔ tools
              │              ├─ Z.AI GLM-4.7 + glm-tts
              │              └─ SQLite + frozen CV prefixes
              └─ Streaming chat + Web Audio playback
```

| | |
| --- | --- |
| Frontend | React 19, Vite, react-router-dom, Web Audio |
| Backend | Express, TypeScript, LangGraph, Z.AI, Resend, SQLite |
| Hosting | Railway (API + SPA same-origin), Cloudflare |

## Status

In production at [avatar.lexoralabshk.com](https://avatar.lexoralabshk.com).
