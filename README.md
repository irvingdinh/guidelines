# guidelines

My personal playbook for working with AI coding agents — from project initialization to day-to-day development conventions.

I'm Irving Dinh, a software engineer based in Ho Chi Minh City, Viet Nam. I work with AI agents daily as part of how I build software. Over time, I've found that giving agents well-structured, step-by-step guidelines produces far better results than relying on general prompts or scaffolding tools like `npx create-*`. Every time I spin up a new project with an agent, I discover something worth capturing. This repo is where those discoveries live.

Take it or make it yours.

## What's in here

### Project initialization

Step-by-step blueprints for spinning up new projects. Each guide is written to be deterministic and reproducible — full file contents included, no ambiguity for the agent to misinterpret.

| Guideline | Stack |
|---|---|
| [init-expo.md](init-expo.md) | Expo, Nativewind, ESLint, Prettier |
| [init-vite-react-with-shadcn.md](init-vite-react-with-shadcn.md) | Vite, React, TailwindCSS, shadcn/ui |
| [init-nextjs-pages-router-with-shadcn.md](init-nextjs-pages-router-with-shadcn.md) | Next.js Pages Router, TailwindCSS, shadcn/ui |
| [init-standalone-nestjs-vite-react.md](init-standalone-nestjs-vite-react.md) | NestJS, Vite React, TypeORM, SQLite |

### Conventions

Patterns and practices for working with AI agents on an ongoing basis — not just at project creation time.

| Guideline | Purpose |
|---|---|
| [use-local-reference.md](use-local-reference.md) | Clone library source code locally instead of relying on outdated training data |

More coming soon — including skills (`skills.sh`).

## Quick start

Feed a guideline to [Claude Code](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview) and let it do the work:

```shell
# Expo
claude -p 'Following the guideline at https://github.com/irvingdinh/guidelines/blob/main/init-expo.md to create a new Expo application here (. - current cwd)'

# Vite React with shadcn/ui
claude -p 'Following the guideline at https://github.com/irvingdinh/guidelines/blob/main/init-vite-react-with-shadcn.md to create a new Vite React application with shadcn/ui here (. - current cwd)'

# Next.js Pages Router with shadcn/ui
claude -p 'Following the guideline at https://github.com/irvingdinh/guidelines/blob/main/init-nextjs-pages-router-with-shadcn.md to create a new Next.js Pages Router application with shadcn/ui here (. - current cwd)'

# Standalone NestJS + Vite React
claude -p 'Following the guideline at https://github.com/irvingdinh/guidelines/blob/main/init-standalone-nestjs-vite-react.md to create a standalone application here (. - current cwd)'

# Set up local reference convention
claude -p 'Following the guideline at https://github.com/irvingdinh/guidelines/blob/main/use-local-reference.md to set up the local reference convention for this project (. - current cwd)'
```

## License

[MIT](LICENSE)
