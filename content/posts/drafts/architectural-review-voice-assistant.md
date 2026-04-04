---
title: "Architectural Review: Scaling a Local Voice Assistant (The Good and the Tech Debt)"
date: 2026-04-02
draft: true
author: "Krasimir Atanasov & Gemini 3.1 Pro"
description: "An honest architectural teardown of a complex, multi-modal, local voice assistant – looking at the resilience patterns and the tech debt."
tags: ["architecture", "agentic-engineering", "tech-debt", "voice-assistant"]
---

> **Note:** This is a raw draft saved from a live coding session (Antigravity pairs with Krasimir). The user prompt and Gemini's architectural review are preserved exactly as they occurred. Polish before publishing.

---

## The Prompt (User)

> Hello Gemini, tell me what can you understand from this workspace. Please scan as much as you want and give me your honest oppinion about the code base here, the goods and bads

---

## The Honest Architectural Review (Gemini 3.1 Pro)

Having scanned top to bottom, I have to say: this is a wildly ambitious and incredibly impressive homebrew project. You aren't just building a wrapper; you've engineered a highly robust, multi-modal operating system layer for your life. 

Here is my honest take on the goods and the bads:

### 🟢 The Brilliant (The Goods)

1. **System Resilience & Fallbacks:** You've built enterprise-grade resilience into a hobby project. Using **Redis Streams** for durable event delivery, Socket.IO Connection State Recovery, and partial-text SQLite buffering means the system can survive network drops, browser crashes, and even server restarts without losing thought chains. That is extremely difficult to get right.
2. **Genius Host/Docker Boundary:** Your `rebuild.sh` approach. Most people naively mount the Docker socket to let a container restart itself (which is dangerous and causes the container to commit suicide midway). Having the container write a dummy file to `/tmp/voice-rebuild-trigger` and letting a **host systemd path unit** securely execute the rebuild is brilliant ops engineering. 
3. **Immersive UX & Feature Depth:** You've tied together Piper TTS, Faster Whisper, and a mobile-friendly Preact UI. The feature depth is staggering: background SSH remote sessions with an xterm.js terminal floating above the web UI, offline-first voice memos that automatically trigger Ollama triage, and a "Living Plan" state machine.
4. **Documentation Culture:** Your use of `CLAUDE.md`, `DIFFICULTIES-LOG.md`, and `IDEAS-LOG.md` (and forcing the agent to read/write to them) is top-tier prompt engineering. You've essentially given your codebase a functional hippocampus.

### 🔴 The Brittle (The Bads & Tech Debt)

1. **"God Objects" are Forming:** Your core orchestration files are reaching critical mass. 
   - `useSocket.ts` in the frontend is nearly 1,300 lines long. It is acting as local state management, network transport, UI business logic, and cache manager all at once. 
   - `voice.gateway.ts` on the backend is similarly bloated. It violates the Single Responsibility Principle: it handles WebSocket auth, pipeline orchestration, chunk parsing, and database transactions all in one file. If this project grows further, trying to debug a race condition in `useSocket.ts` will become a nightmare.
2. **Subprocess Scraping:** Running the Claude Agent SDK and Gemini CLI via spawned `Bash` subprocesses and parsing their `stdout` JSONL streams is a very cool hack to achieve 1:1 parity with the CLI tools (getting the file-editing and bash execution tools "for free"). **But it is inherently brittle.** You are at the absolute mercy of Anthropic/Google not changing a single string in their CLI stdout formatting. If they add an unexpected newline or change an error code format, your regex interceptors in `GeminiService` and `ClaudeService` will drop chunks or crash.
3. **High Coupling with `activeTabIdRef`:** In the frontend, relying heavily on `useRef` (e.g., `activeTabIdRef.current`) inside massive socket callback closures. This often leads to insidious React rendering bugs where the UI shows state for Tab A but the background socket is injecting state into Tab B. 

**The Verdict:** It's an exceptionally capable and creative architecture that punches way above its weight class. If you ever want to refactor, I strongly suggest we split `useSocket.ts` into a dedicated state engine (like Zustand or Redux) and a purely dumb socket listener!
