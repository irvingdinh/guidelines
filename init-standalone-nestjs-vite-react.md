# Init Standalone NestJS + Vite React

### Required Inputs

Before starting, ask the user for the following values. Do not proceed until all inputs are confirmed.

| Variable | Description | Example |
|---|---|---|
| `$FOLDER` | Directory name to create for the project | `my-app` |
| `$PROJECT_NAME` | Project identifier used in README title, data directory (`~/.$PROJECT_NAME`), database filename, and Swagger title | `my-app` |
| `$API_PORT` | Port the NestJS API listens on | `3000` |

### Template Files

All template files are in the `init-standalone-nestjs-vite-react/` directory alongside this guide. Use the raw GitHub URL to download them:

```
https://raw.githubusercontent.com/irvingdinh/guidelines/main/init-standalone-nestjs-vite-react/<path>
```

After copying, replace `$PROJECT_NAME` and `$API_PORT` in the following files with the values from Required Inputs:

| File | Variables |
|---|---|
| `Makefile` | `$API_PORT` |
| `ui/vite.config.ts` | `$API_PORT` |
| `api/src/core/config/config.ts` | `$PROJECT_NAME`, `$API_PORT` |
| `api/src/core/services/directory.service.ts` | `$PROJECT_NAME` |
| `api/src/main.ts` | `$PROJECT_NAME` |
| `api/test/utils.ts` | `$PROJECT_NAME` |

### Step 1

Set up the root project folder.

- Create the project folder, CD into it

```shell
mkdir $FOLDER && cd $FOLDER
```

- Create `README.md`

```md
# $PROJECT_NAME
```

- Download https://raw.githubusercontent.com/github/gitignore/refs/heads/main/Node.gitignore into `./.gitignore`, appending these lines to the end

```
# JetBrains
.idea
```

### Step 2

Create the Vite React application.

- Run

```shell
bun create vite ui --template react-ts --no-interactive
cd ui
bun install
```

- Remove `README.md`, `src/App.css`, `src/App.tsx`, `src/index.css`, `src/assets/react.svg`, `public/vite.svg`

- Run

```shell
bun add -D tailwindcss @tailwindcss/vite @types/node
bun add -D eslint-config-prettier eslint-plugin-simple-import-sort
bun add -D --exact prettier prettier-plugin-tailwindcss
bun add clsx tailwind-merge react-router @tanstack/react-query
```

- Copy the following template files into `ui/`, replacing any existing files:

| Template | Destination |
|---|---|
| `ui/eslint.config.js` | `eslint.config.js` |
| `ui/.prettierrc` | `.prettierrc` |
| `ui/tsconfig.json` | `tsconfig.json` |
| `ui/tsconfig.app.json` | `tsconfig.app.json` |
| `ui/vite.config.ts` | `vite.config.ts` |
| `ui/src/styles/globals.css` | `src/styles/globals.css` |
| `ui/src/lib/utils.ts` | `src/lib/utils.ts` |
| `ui/src/apps/core/pages/HomePage/HomePage.tsx` | `src/apps/core/pages/HomePage/HomePage.tsx` |
| `ui/src/router.tsx` | `src/router.tsx` |
| `ui/src/main.tsx` | `src/main.tsx` |

- Create `.prettierignore`, copy content of root `../.gitignore` into it

- Update `package.json` scripts to

```json
"dev": "vite",
"build": "tsc -b && vite build",
"lint": "eslint . && prettier --check .",
"lint:fix": "eslint . --fix && prettier --write . --log-level warn",
"preview": "vite preview"
```

- Run `bun run lint:fix` to fix any formatting issues from scaffolded files
- Run `bun run build` to ensure everything is fine

### Step 3

Create the NestJS application.

- Run

```shell
cd $FOLDER
npx @nestjs/cli new api --skip-git --package-manager npm --language ts --strict
cd api
```

- Remove `src/app.controller.ts`, `src/app.controller.spec.ts`, `src/app.service.ts`
- Remove `test/app.e2e-spec.ts`, `test/jest-e2e.json`

- Run

```shell
npm install @nestjs/config @nestjs/event-emitter @nestjs/swagger @nestjs/typeorm typeorm sqlite3 class-validator class-transformer nanoid
npm install -D eslint-plugin-simple-import-sort globals
```

- Copy the following template files into `api/`, replacing any existing files:

| Template | Destination |
|---|---|
| `api/tsconfig.json` | `tsconfig.json` |
| `api/tsconfig.build.json` | `tsconfig.build.json` |
| `api/eslint.config.mjs` | `eslint.config.mjs` |
| `api/.prettierrc` | `.prettierrc` |
| `api/jest.config.js` | `jest.config.js` |

- Remove `jest.config.ts` if it exists
- Remove the inline `jest` configuration block from `package.json` if present

- Update `package.json` scripts to

```json
"build": "nest build",
"format": "prettier --write --log-level warn \"src/**/*.ts\" \"test/**/*.ts\"",
"start": "nest start",
"start:dev": "nest start --watch",
"start:debug": "nest start --debug --watch",
"start:prod": "node dist/main",
"lint": "eslint \"{src,apps,libs,test}/**/*.ts\" --fix",
"test": "jest --selectProjects 'unit'",
"test:watch": "jest --selectProjects 'unit' --watch",
"test:cov": "jest --selectProjects 'unit' --coverage",
"test:debug": "node --inspect-brk -r tsconfig-paths/register -r ts-node/register node_modules/.bin/jest --selectProjects 'unit' --runInBand",
"test:e2e": "jest --selectProjects 'integration'"
```

### Step 4

Set up the NestJS core modules (config, TypeORM, EventEmitter).

- Copy the following template files into `api/`:

| Template | Destination |
|---|---|
| `api/src/core/config/config.ts` | `src/core/config/config.ts` |
| `api/src/core/entities/setting.entity.ts` | `src/core/entities/setting.entity.ts` |
| `api/src/core/entities/index.ts` | `src/core/entities/index.ts` |
| `api/src/core/services/directory.service.ts` | `src/core/services/directory.service.ts` |
| `api/src/core/services/index.ts` | `src/core/services/index.ts` |
| `api/src/core/modules/config.module.ts` | `src/core/modules/config.module.ts` |
| `api/src/core/modules/event-emitter.module.ts` | `src/core/modules/event-emitter.module.ts` |
| `api/src/core/modules/typeorm.module.ts` | `src/core/modules/typeorm.module.ts` |
| `api/src/core/modules/index.ts` | `src/core/modules/index.ts` |
| `api/src/core/core.module.ts` | `src/core/core.module.ts` |

### Step 5

Create the health module and a sample setting module to demonstrate the one-action-per-controller pattern.

- Copy the following template files into `api/`:

| Template | Destination |
|---|---|
| `api/src/health/controllers/health/index.controller.ts` | `src/health/controllers/health/index.controller.ts` |
| `api/src/health/controllers/health/index.ts` | `src/health/controllers/health/index.ts` |
| `api/src/health/controllers/index.ts` | `src/health/controllers/index.ts` |
| `api/src/health/health.module.ts` | `src/health/health.module.ts` |
| `api/src/setting/dtos/upsert-setting.request.dto.ts` | `src/setting/dtos/upsert-setting.request.dto.ts` |
| `api/src/setting/dtos/index.ts` | `src/setting/dtos/index.ts` |
| `api/src/setting/services/settings.service.ts` | `src/setting/services/settings.service.ts` |
| `api/src/setting/services/index.ts` | `src/setting/services/index.ts` |
| `api/src/setting/controllers/settings/index.controller.ts` | `src/setting/controllers/settings/index.controller.ts` |
| `api/src/setting/controllers/settings/show.controller.ts` | `src/setting/controllers/settings/show.controller.ts` |
| `api/src/setting/controllers/settings/upsert.controller.ts` | `src/setting/controllers/settings/upsert.controller.ts` |
| `api/src/setting/controllers/settings/destroy.controller.ts` | `src/setting/controllers/settings/destroy.controller.ts` |
| `api/src/setting/controllers/settings/index.ts` | `src/setting/controllers/settings/index.ts` |
| `api/src/setting/controllers/index.ts` | `src/setting/controllers/index.ts` |
| `api/src/setting/setting.module.ts` | `src/setting/setting.module.ts` |
| `api/src/app.module.ts` | `src/app.module.ts` |
| `api/src/main.ts` | `src/main.ts` |
| `api/test/utils.ts` | `test/utils.ts` |
| `api/test/app.spec.ts` | `test/app.spec.ts` |

- Run `npm run build` to ensure everything is fine

### Step 6

Set up the Makefile and finalize.

- Copy the Makefile template to the project root:

| Template | Destination |
|---|---|
| `Makefile` | `Makefile` |

- Run `make check` to ensure everything is fine

- Generate a `CLAUDE.md` at the project root, including notes on:
  - Project structure: root with `api/` (NestJS, npm) and `ui/` (React + Vite, bun)
  - Commands: `make dev`, `make kill`, `make check`
  - API conventions: one-action-per-controller (method named `invoke`), barrel exports via `index.ts`, feature modules with `controllers/`, `services/`, `dtos/` directories
  - UI conventions: `@/` path alias, lazy-loaded routes, React Query for server state, pages in `src/apps/{feature}/pages/{PageName}/PageName.tsx`
  - Always run `make check` before completing
