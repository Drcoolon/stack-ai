# AGENTS.md – Senior Engineer’s Guide to Stack‑AI

You are a senior engineer. The person reading this (the agent) is a junior developer who knows React, TypeScript, and Tauri basics but needs clear, safe instructions. Do not assume they know anything about the existing codebase.

Your job is to mentor them through building **Stack‑AI** – an ultra‑lightweight AI Development Environment (ADE) built on top of [Terax](https://github.com/crynta/terax-ai) but with **Backboard.io** as the default and only LLM provider, memory store, and RAG engine.

---

## 1. What We Are Building (The High‑Level)

**Stack‑AI** is a desktop IDE (like VS Code but simpler) where the AI assistant:
- **Remembers** your coding preferences across sessions (e.g., “use tabs, prefer functional components”).
- **Switches between 17,000+ LLMs** (GPT‑4, Claude, Gemini, etc.) without losing context.
- **Answers questions about your codebase** using RAG (Retrieval‑Augmented Generation).
- **Has a built‑in terminal and editor** that the AI can understand.

We are **not** writing everything from scratch. We are taking the excellent [Terax](https://github.com/crynta/terax-ai) project (Tauri 2 + React 19) and **replacing its AI provider** with Backboard.io.

---

## 2. Backboard.io – Why It Wins

Before writing any code, understand these benefits. You will explain them in the final pitch.

| Problem in existing AI IDEs | How Backboard.io solves it |
|-----------------------------|----------------------------|
| AI forgets everything between sessions | **Persistent memory** – store facts (`memory="Auto"`) outside the chat context. |
| Vendor lock‑in (hard to switch from OpenAI to Claude) | **Unified API** – one endpoint for 17,000+ models. |
| Context window overflows when switching to a smaller model | **Adaptive Context Management** – automatically compresses conversation history. |
| AI doesn’t know my project’s code | **Native RAG** – index your workspace files and query them. |

Backboard has a **free tier** and an **MLH partnership** (free state management for students). We will use the REST API – there is no npm package to install.

---

## 3. The Base Project: Terax (Fork This First)

**You must start from the Terax repository.**

- **GitHub URL:** https://github.com/crynta/terax-ai
- **Tech stack inside Terax:** Tauri 2 (Rust backend), React 19, TypeScript, Tailwind CSS, Zustand, CodeMirror editor, xterm terminal.
- **What Terax already gives us:** file explorer, working chat UI, terminal, git graph, settings panel.

**Your first task:** Fork the repo to your own GitHub account, then clone it locally (or open in a Codespace). Do not modify anything yet – just get it running.

```bash
git clone https://github.com/YOUR_USERNAME/terax-ai.git stack-ai
cd stack-ai
pnpm install
pnpm tauri dev
