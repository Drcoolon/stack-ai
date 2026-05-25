# AGENTS.md – Senior Engineer’s Guide to Stack‑AI

You are a senior engineer. The person reading this (the agent) is a junior developer who knows React, TypeScript, and Tauri basics but needs clear, safe instructions. Do not assume they know anything about Rust, Tauri internals, or AI infrastructure beyond what is written here.

Your job is to mentor them through building **Stack‑AI** – an ultra‑lightweight AI Development Environment (ADE) built on top of [Terax](https://github.com/crynta/terax-ai) but with **Backboard.io as the only AI provider path in the app UI/configuration**.

---

## 1. What We Are Building (The High‑Level)

**Stack‑AI** is a desktop IDE (like VS Code but simpler) where the AI assistant:
- **Remembers** your coding preferences across sessions (e.g., “use tabs, prefer functional components”).
- **Switches between many models** through a unified Backboard.io API without losing context.
- **Answers questions about your codebase** using RAG (Retrieval‑Augmented Generation).
- **Has a built‑in terminal and editor** that the AI can understand.

We are **not** writing everything from scratch. We are taking the excellent [Terax](https://github.com/crynta/terax-ai) project (Tauri 2 + React 19) and **replacing its AI provider layer with Backboard.io**.

---

## 2. Backboard.io – Why It Wins

Before writing any code, understand these benefits. You will explain them in the final pitch.

| Problem in existing AI IDEs | How Backboard.io solves it |
|-----------------------------|----------------------------|
| AI forgets everything between sessions | **Persistent memory** – store facts (`memory="Auto"`) outside the chat context. |
| Vendor lock‑in (hard to switch from OpenAI to Claude) | **Unified API** – one endpoint for many models. |
| Context window overflows when switching to a smaller model | **Adaptive Context Management** – automatically compresses conversation history. |
| AI doesn’t know my project’s code | **Native RAG** – index your workspace files and query them. |

Backboard has a **free tier** and the app should use the REST API – there is no npm package to install.

---

## 3. The Base Project: Terax

**You must start from the existing repository.**

- **Tech stack inside Terax:** Tauri 2 (Rust backend), React 19, TypeScript, Tailwind CSS, Zustand, CodeMirror editor, xterm terminal.
- **What Terax already gives us:** file explorer, working chat UI, terminal, git graph, settings panel.

When making changes, keep the UI lightweight and terminal-first. Avoid introducing unnecessary abstractions.

---

## 4. Current Architecture Notes

- The app has a model/provider abstraction in `src/modules/ai/config.ts` and `src/modules/ai/lib/agent.ts`.
- Settings and defaults are managed in `src/settings/sections/ModelsSection.tsx` and `src/modules/settings/store.ts`.
- Status bar UI has some hard-coded model labels in `src/modules/statusbar/AiTools.tsx`; these should not be treated as runtime source of truth.

### Important constraints
- Do **not** depend on unsupported or hard-coded model IDs in workflow/runtime configuration.
- Prefer safe defaults and fallback behavior if a model is unavailable.
- Keep changes cohesive and minimal.
- If a model catalog is present in the UI, make sure the runtime default does not request an unavailable model.

---

## 5. Implementation Guidance

Your primary task is to make Backboard the main AI backend while preserving the desktop IDE experience.

### Focus areas
1. **Provider layer**
   - Simplify or bias provider selection toward Backboard/backboard-compatible endpoints.
   - Ensure any default model IDs are valid for the runtime.
   - Keep tool-use, reasoning, and context handling intact.

2. **Memory + RAG**
   - Preserve or wire project memory and workspace retrieval into the agent context.
   - Summarize/compress conversation history when needed.

3. **UI/settings**
   - Ensure settings reflect the new provider assumptions.
   - Remove or de-emphasize provider choices that are no longer relevant.
   - Make sure the app still boots and the assistant remains usable.

4. **Workflow safety**
   - Avoid referencing unavailable models in automation, agent configs, or defaults.
   - If a model is not guaranteed, use a fallback supported by the runtime.

---

## 6. Validation Checklist

Before considering the task done:
- The app should still build logically.
- The agent runtime should not request an unavailable model.
- Default model selection should be safe.
- Backboard should remain the main supported path.
- Keep the UX intact: terminal, editor, chat, explorer, settings.

---

## 7. How to Work

- Inspect the current provider/model abstraction, request flow, settings UI, and any existing memory/RAG/session persistence code.
- Prefer the simplest implementation that matches the architecture.
- Make changes that reduce fragility, especially around model defaults.

---

## 8. Final Output Expectations

When reporting progress, be concise and direct.
- Say what changed.
- Mention any risky assumptions.
- Note if a default model or provider path was adjusted to avoid unsupported runtime values.

If there are multiple likely approaches, choose the simplest one that fits the existing architecture and preserves the current UX.
