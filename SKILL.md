---
name: chrome-extension-init-skill
description: Initialize a browser extension project from an empty folder by gathering choices in multi-turn dialogue (framework, language, bundler, manifest version, UI library, styling, tooling), then scaffolding all configs, wiring build scripts, initializing git, and making the first commit. Use when a user asks to start or bootstrap a Chrome/Chromium-based extension from scratch.
---

# Chrome Extension Init Skill

## Overview

Guide the user through selecting a complete extension tech stack and scaffold a working project with all required configs, then initialize git and commit.

## Workflow

### 1) Confirm Preconditions

- Verify the target directory is empty or get permission to scaffold into it.
- Confirm target browsers (default to Chrome/Chromium). If the user needs Firefox, note differences and ask before proceeding.

### 2) Collect Stack Choices (multi-turn)

Ask these in order; use numeric options for each choice and allow multi-select where meaningful. Add a "Default (recommended)" option that skips the step-by-step questions and uses:
- Extension type: MV3
- Framework: React
- Language: TypeScript
- Bundler: Vite
- UI surface: Popup + Options
- UI library: shadcn/ui + Tailwind CSS
- Styling: Tailwind CSS
- Tooling: ESLint + Prettier
- Package manager: pnpm

- Extension type: MV3 (default), MV2 only if explicitly requested.
- Framework: React, Vue, Svelte, Vanilla.
- Language: TypeScript, JavaScript.
- Bundler: Vite (default), Webpack, Rsbuild.
- UI surface (explain briefly for beginners): Popup (toolbar button panel), Options (full settings page), Side panel (Chrome side panel), DevTools (panel inside DevTools), Content script UI (in-page UI injected into websites).
- UI library (optional, allow multi-select): shadcn/ui, Tailwind UI, Material UI, Ant Design, None.
- Styling: Tailwind CSS, CSS Modules, plain CSS.
- Tooling: ESLint, Prettier, commit lint (ask as a set).
- Package manager: pnpm, npm, yarn (default to project/user preference).

Use toast for user-facing prompts and avoid alert when generating UI scaffolding.

### 3) Scaffold Project

- Create package.json with scripts for dev/build/preview and extension-specific build outputs.
- Generate base source structure:
  - `src/manifest.ts` or `src/manifest.json` (ensure MV3 fields).
  - `src/background/` for service worker.
  - `src/content/` for content scripts.
  - `src/ui/popup/` and other UI surfaces selected.
  - `src/constants/` for shared constants (place configuration constants here).
- Wire bundler config (Vite/Webpack/Rsbuild) to output a `/dist` folder with correct extension layout.
- Ensure manifest references built assets correctly and is produced for `/dist`.

### 4) Configure Tooling

- Add lint/format configs based on user choices.
- Add TypeScript config if TypeScript is selected.
- Add Tailwind config if Tailwind is selected; add shadcn/ui setup only if chosen.

### 5) Initialize Git and Commit

- Initialize git in the project root.
- Create `.gitignore` suitable for the chosen tooling.
- Run an initial install and build if appropriate.
- Make the first commit with a clear message like "chore: initialize extension project".

### 6) Hand-off

- Provide quick start commands and explain where to edit manifest, background, content, and UI.
