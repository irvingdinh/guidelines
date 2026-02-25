# Use Local Reference

A convention for giving AI coding agents access to accurate, up-to-date source code by cloning repositories locally instead of relying on web search or training knowledge.

## Why

- **Training data goes stale.** Models are trained on snapshots — API signatures change, components get renamed, utilities are added or removed. Local source code is always current.
- **Web search is unreliable.** Results may be outdated, incomplete, or from the wrong version. Blog posts and Stack Overflow answers often lag behind actual source code.
- **Local reads are fast and context-efficient.** Agents can use targeted subagents to explore local repos on-demand without polluting the main conversation context or waiting on network calls.

## Convention

### Directory structure

Use the `.idea/` directory (already gitignored by JetBrains convention) to store cloned reference repositories. Organize them by their GitHub origin:

```
.idea/
  github.com/
    {owner}/
      {repo}/          # full or shallow clone
    {owner}/
      {repo}/
```

For example:

```
.idea/
  github.com/
    facebook/
      react/
    shadcn-ui/
      ui/
    your-org/
      your-specs/
```

### Gitignore

Most JetBrains-oriented `.gitignore` files already include `.idea`. If yours does not, add it:

```
# JetBrains
.idea
```

This ensures cloned repos never get committed to your project.

### Cloning

Use shallow clones to save disk space and clone time:

```shell
# Clone a dependency for local reference
git clone --depth 1 https://github.com/{owner}/{repo}.git .idea/github.com/{owner}/{repo}
```

For a specific branch or tag:

```shell
git clone --depth 1 --branch v4.0.0 https://github.com/{owner}/{repo}.git .idea/github.com/{owner}/{repo}
```

### What to clone

Clone anything the agent might need to reference accurately. Common categories:

- **Dependency source code** — libraries and frameworks your project uses (React, Tailwind, NestJS, etc.)
- **Reference implementations** — projects with patterns you want to follow or learn from
- **Project specs and requirements** — private repos containing product specs, architecture docs, or design decisions
- **Community examples** — well-built open source projects demonstrating patterns relevant to your work (e.g., Vercel AI Chatbot for chat UI/UX patterns)

There is no prescribed categorization. Use whatever makes sense for your project.

## CLAUDE.md integration

Add a `## Reference` section to your project's `CLAUDE.md` to tell the agent what's available and how to use it. Template:

```markdown
## Reference

All reference materials live under `.idea/github.com/` (gitignored). **Do NOT read these
into the main conversation context.** Instead, spawn an Explore subagent to discover and
read from them on-demand. This keeps the main context lean.

- `.idea/github.com/{owner}/{repo}` — Short description of what this is and when to consult it.
- `.idea/github.com/{owner}/{repo}` — Short description.

**Convention for new dependencies:** When you need to reference a dependency that is not yet
cloned locally, shallow-clone it into `.idea/github.com/{owner}/{repo}`
(`git clone --depth 1`) and add it to the list above. Prefer local source code over web
search or training knowledge — training data may be outdated.
```

### Key instructions for the agent

The CLAUDE.md section should make these points clear:

1. **Use Explore subagents** — Never read reference repos directly into the main conversation. Spawn a subagent to search and read on-demand. This prevents context window bloat.
2. **Prefer local over web/training** — When you need to understand API surface, component signatures, utility functions, or implementation details, always check local source first.
3. **Self-extending** — When the agent encounters a dependency not yet cloned, it should clone it into `.idea/github.com/{owner}/{repo}` and update CLAUDE.md. This keeps the reference library growing organically.

## When to use local vs. web search

| Need | Prefer |
|---|---|
| API signatures, function parameters, return types | Local source |
| Component props, default values, internal behavior | Local source |
| Utility functions, helpers, constants | Local source |
| How a library structures its code internally | Local source |
| Changelog, migration guides, breaking changes between versions | Web search |
| GitHub issues, known bugs, workarounds | Web search |
| Community best practices, tutorials, opinions | Web search |

**Rule of thumb:** If you need to know *what the code actually does*, read the code. If you need to know *what changed or what people think*, search the web.

## Maintenance

### Updating clones

Shallow clones can be updated in place:

```shell
cd .idea/github.com/{owner}/{repo}
git pull
```

Or nuke and re-clone for a clean state:

```shell
rm -rf .idea/github.com/{owner}/{repo}
git clone --depth 1 https://github.com/{owner}/{repo}.git .idea/github.com/{owner}/{repo}
```

Consider updating clones when:

- You upgrade a dependency to a new major version
- You notice the agent giving answers that don't match your installed version
- It's been a while and you want to stay current

There is no need to update on a schedule. Do it when it matters.

### Size management

Some repositories are large even with `--depth 1`. Strategies to keep things manageable:

- **Sparse checkout** — Clone only the directories you actually need:

```shell
git clone --depth 1 --filter=blob:none --sparse https://github.com/{owner}/{repo}.git .idea/github.com/{owner}/{repo}
cd .idea/github.com/{owner}/{repo}
git sparse-checkout set src/packages/core src/packages/react
```

- **Delete after use** — If a reference was only needed for a specific task, remove it when done.
- **Check size before cloning** — Large monorepos (e.g., chromium, linux) are usually not worth cloning entirely. Clone only the relevant subdirectory or find a more focused fork.
