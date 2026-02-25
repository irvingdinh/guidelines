# CLAUDE.md

This repository contains guidelines — step-by-step instructions and conventions for working with AI coding agents. The audience is Irving Dinh, his coworkers, friends, and followers.

## Repository structure

- `init-*.md` — Project initialization guidelines. Deterministic, step-by-step blueprints for spinning up new projects.
- `use-*.md` — Convention guidelines. Patterns and practices for ongoing development with AI agents.
- `init-*/` — Template files that accompany an init guideline (directory name matches the guideline file name).
- `skills.sh` — Skills (coming soon).

Files are kept flat in the root. No nested folder structure for guidelines.

## Writing guidelines

Every guideline must follow these principles:

- **Step-by-step and imperative.** Write as a sequence of concrete actions: "Run this command", "Create this file with the following content". No vague instructions.
- **Deterministic and reproducible.** An AI agent following the guide should produce the exact same result every time. Include full file contents inline — never say "add X to your config" when you can provide the complete file.
- **Self-contained.** Each guideline should work on its own. Do not assume the reader has read other guidelines.
- **Template files go in a matching directory.** If a guideline references multiple template files, place them in a directory with the same name as the guideline (e.g., `init-standalone-nestjs-vite-react.md` → `init-standalone-nestjs-vite-react/`).

## File naming

- Guidelines: kebab-case with a type prefix (`init-`, `use-`).
- Template files: match the conventions of their target stack (e.g., `eslint.config.mjs`, `tsconfig.json`).

## Tone

Opinionated but approachable. These are Irving's preferred ways of doing things — not the only way.

## What not to do

- Do not add guidelines for stacks or tools that have not been personally vetted by Irving.
- Do not restructure the flat file layout without explicit instruction.
- Do not remove the `--dangerously-skip-permissions` flag context from quick start examples — users can choose their own permission mode.
