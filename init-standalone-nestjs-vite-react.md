# Init Standalone NestJS + Vite React

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

- Remove `src/App.css`, `src/App.tsx`, `src/index.css`, `src/assets/react.svg`, `public/vite.svg`

- Run

```shell
bun add -D tailwindcss @tailwindcss/vite @types/node
bun add -D eslint-config-prettier eslint-plugin-simple-import-sort
bun add -D --exact prettier prettier-plugin-tailwindcss
bun add clsx tailwind-merge react-router @tanstack/react-query
```

- Replace `eslint.config.js` with

```js
import js from "@eslint/js";
import eslintConfigPrettier from "eslint-config-prettier/flat";
import eslintPluginSimpleImportSort from "eslint-plugin-simple-import-sort";
import reactHooks from "eslint-plugin-react-hooks";
import reactRefresh from "eslint-plugin-react-refresh";
import globals from "globals";
import tseslint from "typescript-eslint";
import { defineConfig, globalIgnores } from "eslint/config";

export default defineConfig([
  globalIgnores(["dist"]),
  {
    files: ["**/*.{ts,tsx}"],
    extends: [
      js.configs.recommended,
      tseslint.configs.recommended,
      reactHooks.configs.flat.recommended,
      reactRefresh.configs.vite,
    ],
    languageOptions: {
      ecmaVersion: 2020,
      globals: globals.browser,
    },
    plugins: {
      "simple-import-sort": eslintPluginSimpleImportSort,
    },
    rules: {
      "simple-import-sort/imports": "error",
      "simple-import-sort/exports": "error",
    },
  },
  eslintConfigPrettier,
]);
```

- Create `.prettierrc`

```json
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

- Create `.prettierignore`, copy content of root `../.gitignore` into it

- Replace `tsconfig.json` with

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "files": [],
  "references": [
    { "path": "./tsconfig.app.json" },
    { "path": "./tsconfig.node.json" }
  ]
}
```

- Update `tsconfig.app.json`, appending inside `compilerOptions`

```json
"baseUrl": ".",
"paths": {
  "@/*": ["./src/*"]
}
```

- Replace `vite.config.ts` with

```ts
import tailwindcss from "@tailwindcss/vite";
import react from "@vitejs/plugin-react";
import path from "path";
import { defineConfig } from "vite";

export default defineConfig({
  plugins: [react(), tailwindcss()],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
  server: {
    proxy: {
      "/api": {
        target: "http://localhost:$API_PORT",
        changeOrigin: true,
      },
    },
  },
});
```

- Update `package.json` scripts to

```json
"dev": "vite",
"build": "tsc -b && vite build",
"lint": "eslint . && prettier --check .",
"lint:fix": "eslint . --fix && prettier --write . --log-level warn",
"preview": "vite preview"
```

- Create `src/styles/globals.css`

```css
@import "tailwindcss";
```

- Create `src/lib/utils.ts`

```ts
import { type ClassValue, clsx } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

- Create `src/apps/core/pages/HomePage/HomePage.tsx`

```tsx
export default function HomePage() {
  return <p>Hello, World!</p>;
}
```

- Create `src/router.tsx`

```tsx
import { type ComponentType } from "react";
import { createBrowserRouter } from "react-router";

const lazy =
  (importFn: () => Promise<{ default: ComponentType }>) =>
  async (): Promise<{ Component: ComponentType }> => {
    const { default: Component } = await importFn();
    return { Component };
  };

export const router = createBrowserRouter([
  {
    path: "/",
    lazy: lazy(() => import("@/apps/core/pages/HomePage/HomePage")),
  },
]);
```

- Replace `src/main.tsx` with

```tsx
import "./styles/globals.css";

import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import { RouterProvider } from "react-router";

import { router } from "@/router";

const queryClient = new QueryClient();

createRoot(document.getElementById("root")!).render(
  <StrictMode>
    <QueryClientProvider client={queryClient}>
      <RouterProvider router={router} />
    </QueryClientProvider>
  </StrictMode>,
);
```

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
- Create empty `test/.gitkeep`

- Run

```shell
npm install @nestjs/config @nestjs/event-emitter @nestjs/swagger @nestjs/typeorm typeorm sqlite3 class-validator class-transformer nanoid
npm install -D eslint-plugin-simple-import-sort globals
```

- Replace `tsconfig.json` with

```json
{
  "compilerOptions": {
    "module": "nodenext",
    "moduleResolution": "nodenext",
    "resolvePackageJsonExports": true,
    "esModuleInterop": true,
    "isolatedModules": true,
    "declaration": true,
    "removeComments": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "allowSyntheticDefaultImports": true,
    "target": "ES2023",
    "sourceMap": true,
    "outDir": "./dist",
    "baseUrl": "./",
    "incremental": true,
    "skipLibCheck": true,
    "strictNullChecks": true,
    "forceConsistentCasingInFileNames": true,
    "noImplicitAny": false,
    "strictBindCallApply": false,
    "noFallthroughCasesInSwitch": false
  }
}
```

- Replace `tsconfig.build.json` with

```json
{
  "extends": "./tsconfig.json",
  "exclude": ["node_modules", "test", "dist", "**/*spec.ts"]
}
```

- Replace `eslint.config.mjs` with

```mjs
// @ts-check
import eslint from '@eslint/js';
import eslintPluginPrettierRecommended from 'eslint-plugin-prettier/recommended';
import eslintPluginSimpleImportSort from 'eslint-plugin-simple-import-sort';
import globals from 'globals';
import tseslint from 'typescript-eslint';

export default tseslint.config(
  {
    ignores: ['eslint.config.mjs'],
  },
  eslint.configs.recommended,
  ...tseslint.configs.recommendedTypeChecked,
  eslintPluginPrettierRecommended,
  {
    languageOptions: {
      globals: {
        ...globals.node,
        ...globals.jest,
      },
      sourceType: 'commonjs',
      parserOptions: {
        projectService: true,
        tsconfigRootDir: import.meta.dirname,
      },
    },
  },
  {
    plugins: {
      'simple-import-sort': eslintPluginSimpleImportSort,
    },
    rules: {
      '@typescript-eslint/no-explicit-any': 'off',
      '@typescript-eslint/no-floating-promises': 'warn',
      '@typescript-eslint/no-unsafe-argument': 'warn',
      'simple-import-sort/imports': 'error',
      'simple-import-sort/exports': 'error',
      'prettier/prettier': ['error', { endOfLine: 'auto' }],
    },
  },
);
```

- Replace `.prettierrc` with

```json
{
  "singleQuote": true,
  "trailingComma": "all"
}
```

- Replace `jest.config.js` (remove `jest.config.ts` if exists) with

```js
module.exports = {
  projects: [
    {
      displayName: 'Integration Testing',
      moduleFileExtensions: ['js', 'json', 'ts'],
      rootDir: './test',
      testEnvironment: 'node',
      transform: {
        '^.+\\.(t|j)s$': 'ts-jest',
      },
      transformIgnorePatterns: ['node_modules/(?!(nanoid))'],
      testMatch: ['**/*.spec.ts'],
    },
    {
      displayName: 'Unit Testing',
      moduleFileExtensions: ['js', 'json', 'ts'],
      rootDir: './src',
      testEnvironment: 'node',
      transform: {
        '^.+\\.(t|j)s$': 'ts-jest',
      },
      transformIgnorePatterns: ['node_modules/(?!(nanoid))'],
      testMatch: ['**/*.spec.ts'],
    },
  ],
};
```

- Update `package.json` scripts to

```json
"build": "nest build",
"format": "prettier --write --log-level warn \"src/**/*.ts\" \"test/**/*.ts\"",
"start": "nest start",
"start:dev": "nest start --watch",
"start:debug": "nest start --debug --watch",
"start:prod": "node dist/main",
"lint": "eslint \"{src,apps,libs,test}/**/*.ts\" --fix",
"test": "jest --selectProjects 'Unit Testing'",
"test:watch": "jest --selectProjects 'Unit Testing' --watch",
"test:cov": "jest --selectProjects 'Unit Testing' --coverage",
"test:debug": "node --inspect-brk -r tsconfig-paths/register -r ts-node/register node_modules/.bin/jest --selectProjects 'Unit Testing' --runInBand",
"test:e2e": "jest --selectProjects 'Integration Testing'"
```

### Step 4

Set up the NestJS core modules (config, TypeORM, EventEmitter).

- Create `src/core/config/config.ts`

```ts
import { mkdirSync } from 'fs';
import { homedir } from 'os';
import { join } from 'path';

export interface AppConfig {
  http: {
    host: string;
    port: number;
  };
  dir: {
    data: string;
  };
}

function ensureDataDir(): string {
  const dir = process.env.DATA_DIR || join(homedir(), '.$PROJECT_NAME');
  mkdirSync(dir, { recursive: true });
  return dir;
}

export const config = (): { root: AppConfig } => ({
  root: {
    http: {
      host: process.env.HOST || '127.0.0.1',
      port: parseInt(process.env.PORT || '$API_PORT', 10),
    },
    dir: {
      data: ensureDataDir(),
    },
  },
});
```

- Create `src/core/entities/setting.entity.ts`

```ts
import {
  Column,
  CreateDateColumn,
  Entity,
  PrimaryColumn,
  UpdateDateColumn,
} from 'typeorm';

@Entity('settings')
export class SettingEntity {
  @PrimaryColumn()
  key: string;

  @Column({ type: 'text' })
  value: string;

  @CreateDateColumn()
  createdAt: Date;

  @UpdateDateColumn()
  updatedAt: Date;
}
```

- Create `src/core/entities/index.ts`

```ts
import { SettingEntity } from './setting.entity';

export const entities = [SettingEntity];
```

- Create `src/core/services/directory.service.ts`

```ts
import { Injectable } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import { join } from 'path';

import type { AppConfig } from '../config/config';

@Injectable()
export class DirectoryService {
  private readonly config: AppConfig;

  constructor(private readonly configService: ConfigService) {
    this.config = this.configService.get<AppConfig>('root')!;
  }

  dataDir(...segments: string[]): string {
    return join(this.config.dir.data, ...segments);
  }

  databasePath(): string {
    return join(this.config.dir.data, '$PROJECT_NAME.db');
  }
}
```

- Create `src/core/services/index.ts`

```ts
import { DirectoryService } from './directory.service';

export const services = [DirectoryService];

export { DirectoryService };
```

- Create `src/core/modules/config.module.ts`

```ts
import { ConfigModule } from '@nestjs/config';

import { config } from '../config/config';

export const configModule = ConfigModule.forRoot({
  isGlobal: true,
  load: [config],
});
```

- Create `src/core/modules/event-emitter.module.ts`

```ts
import { EventEmitterModule } from '@nestjs/event-emitter';

export const eventEmitterModule = EventEmitterModule.forRoot();
```

- Create `src/core/modules/typeorm.module.ts`

```ts
import { TypeOrmModule, TypeOrmModuleOptions } from '@nestjs/typeorm';

import { entities } from '../entities';
import { DirectoryService } from '../services';

export const typeormForRoot = TypeOrmModule.forRootAsync({
  extraProviders: [DirectoryService],
  useFactory: (directoryService: DirectoryService): TypeOrmModuleOptions => {
    return {
      type: 'sqlite',
      database: directoryService.databasePath(),
      entities: [...entities],
      synchronize: true,
    };
  },
  inject: [DirectoryService],
});

export const typeormForFeature = TypeOrmModule.forFeature([...entities]);
```

- Create `src/core/modules/index.ts`

```ts
import { configModule } from './config.module';
import { eventEmitterModule } from './event-emitter.module';
import { typeormForFeature, typeormForRoot } from './typeorm.module';

export const modules = [
  configModule,
  eventEmitterModule,
  typeormForRoot,
  typeormForFeature,
];
```

- Create `src/core/core.module.ts`

```ts
import { Module } from '@nestjs/common';

import { modules } from './modules';
import { typeormForFeature } from './modules/typeorm.module';
import { services } from './services';

@Module({
  imports: [...modules],
  providers: [...services],
  exports: [...services, typeormForFeature],
})
export class CoreModule {}
```

### Step 5

Create the health module and a sample setting module to demonstrate the one-action-per-controller pattern.

- Create `src/health/controllers/health/index.controller.ts`

```ts
import { Controller, Get } from '@nestjs/common';
import { ApiOperation, ApiTags } from '@nestjs/swagger';

@ApiTags('health')
@Controller('/api/health')
export class IndexController {
  @Get()
  @ApiOperation({ summary: 'Health check' })
  invoke() {
    return { status: 'ok' };
  }
}
```

- Create `src/health/controllers/health/index.ts`

```ts
import { IndexController } from './index.controller';

export const healthControllers = [IndexController];
```

- Create `src/health/controllers/index.ts`

```ts
import { healthControllers } from './health';

export const controllers = [...healthControllers];
```

- Create `src/health/health.module.ts`

```ts
import { Module } from '@nestjs/common';

import { controllers } from './controllers';

@Module({
  controllers: [...controllers],
})
export class HealthModule {}
```

- Create `src/setting/dtos/upsert-setting.request.dto.ts`

```ts
import { ApiProperty } from '@nestjs/swagger';
import { IsNotEmpty, IsString } from 'class-validator';

export class UpsertSettingRequestDto {
  @ApiProperty({ description: 'Setting key' })
  @IsString()
  @IsNotEmpty()
  key: string;

  @ApiProperty({ description: 'Setting value' })
  @IsString()
  @IsNotEmpty()
  value: string;
}
```

- Create `src/setting/dtos/index.ts`

```ts
export { UpsertSettingRequestDto } from './upsert-setting.request.dto';
```

- Create `src/setting/services/settings.service.ts`

```ts
import { Injectable, NotFoundException } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';

import { SettingEntity } from '../../core/entities/setting.entity';
import { UpsertSettingRequestDto } from '../dtos';

@Injectable()
export class SettingsService {
  constructor(
    @InjectRepository(SettingEntity)
    private readonly settingRepository: Repository<SettingEntity>,
  ) {}

  async findAll(): Promise<SettingEntity[]> {
    return this.settingRepository.find({
      order: { key: 'ASC' },
    });
  }

  async findOne(key: string): Promise<SettingEntity> {
    const setting = await this.settingRepository.findOne({ where: { key } });
    if (!setting) throw new NotFoundException(`Setting "${key}" not found`);
    return setting;
  }

  async upsert(dto: UpsertSettingRequestDto): Promise<SettingEntity> {
    const existing = await this.settingRepository.findOne({
      where: { key: dto.key },
    });

    if (existing) {
      existing.value = dto.value;
      return this.settingRepository.save(existing);
    }

    const setting = this.settingRepository.create({
      key: dto.key,
      value: dto.value,
    });
    return this.settingRepository.save(setting);
  }

  async remove(key: string): Promise<void> {
    const setting = await this.findOne(key);
    await this.settingRepository.remove(setting);
  }
}
```

- Create `src/setting/services/index.ts`

```ts
import { SettingsService } from './settings.service';

export const services = [SettingsService];

export { SettingsService };
```

- Create `src/setting/controllers/settings/index.controller.ts`

```ts
import { Controller, Get } from '@nestjs/common';
import { ApiOperation, ApiTags } from '@nestjs/swagger';

import { SettingsService } from '../../services';

@ApiTags('settings')
@Controller('/api/settings')
export class IndexController {
  constructor(private readonly settingsService: SettingsService) {}

  @Get()
  @ApiOperation({ summary: 'List all settings' })
  async invoke() {
    return this.settingsService.findAll();
  }
}
```

- Create `src/setting/controllers/settings/show.controller.ts`

```ts
import { Controller, Get, Param } from '@nestjs/common';
import { ApiOperation, ApiTags } from '@nestjs/swagger';

import { SettingsService } from '../../services';

@ApiTags('settings')
@Controller('/api/settings')
export class ShowController {
  constructor(private readonly settingsService: SettingsService) {}

  @Get(':key')
  @ApiOperation({ summary: 'Get a setting by key' })
  async invoke(@Param('key') key: string) {
    return this.settingsService.findOne(key);
  }
}
```

- Create `src/setting/controllers/settings/upsert.controller.ts`

```ts
import { Body, Controller, Put } from '@nestjs/common';
import { ApiOperation, ApiTags } from '@nestjs/swagger';

import { UpsertSettingRequestDto } from '../../dtos';
import { SettingsService } from '../../services';

@ApiTags('settings')
@Controller('/api/settings')
export class UpsertController {
  constructor(private readonly settingsService: SettingsService) {}

  @Put()
  @ApiOperation({ summary: 'Create or update a setting' })
  async invoke(@Body() dto: UpsertSettingRequestDto) {
    return this.settingsService.upsert(dto);
  }
}
```

- Create `src/setting/controllers/settings/destroy.controller.ts`

```ts
import {
  Controller,
  Delete,
  HttpCode,
  HttpStatus,
  Param,
} from '@nestjs/common';
import { ApiOperation, ApiTags } from '@nestjs/swagger';

import { SettingsService } from '../../services';

@ApiTags('settings')
@Controller('/api/settings')
export class DestroyController {
  constructor(private readonly settingsService: SettingsService) {}

  @Delete(':key')
  @HttpCode(HttpStatus.NO_CONTENT)
  @ApiOperation({ summary: 'Delete a setting by key' })
  async invoke(@Param('key') key: string) {
    await this.settingsService.remove(key);
  }
}
```

- Create `src/setting/controllers/settings/index.ts`

```ts
import { DestroyController } from './destroy.controller';
import { IndexController } from './index.controller';
import { ShowController } from './show.controller';
import { UpsertController } from './upsert.controller';

export const settingControllers = [
  DestroyController,
  IndexController,
  ShowController,
  UpsertController,
];
```

- Create `src/setting/controllers/index.ts`

```ts
import { settingControllers } from './settings';

export const controllers = [...settingControllers];
```

- Create `src/setting/setting.module.ts`

```ts
import { Module } from '@nestjs/common';

import { CoreModule } from '../core/core.module';
import { controllers } from './controllers';
import { services } from './services';

@Module({
  imports: [CoreModule],
  controllers: [...controllers],
  providers: [...services],
})
export class SettingModule {}
```

- Replace `src/app.module.ts` with

```ts
import { Module } from '@nestjs/common';

import { CoreModule } from './core/core.module';
import { HealthModule } from './health/health.module';
import { SettingModule } from './setting/setting.module';

@Module({
  imports: [CoreModule, HealthModule, SettingModule],
})
export class AppModule {}
```

- Replace `src/main.ts` with

```ts
import { ValidationPipe } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import { NestFactory } from '@nestjs/core';
import { DocumentBuilder, SwaggerModule } from '@nestjs/swagger';

import { AppModule } from './app.module';
import type { AppConfig } from './core/config/config';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  const config = app.get(ConfigService).get<AppConfig>('root')!;
  app.useGlobalPipes(new ValidationPipe({ transform: true }));

  const swaggerConfig = new DocumentBuilder()
    .setTitle('$PROJECT_NAME API')
    .setVersion('0.0.1')
    .addTag('health')
    .addTag('settings')
    .build();

  const document = SwaggerModule.createDocument(app, swaggerConfig);
  SwaggerModule.setup('swagger', app, document);

  await app.listen(config.http.port, config.http.host);
}

void bootstrap();
```

- Run `npm run build` to ensure everything is fine

### Step 6

Set up the Makefile and finalize.

- Create `Makefile` at the project root

```makefile
.PHONY: dev kill check check-api check-ui

check:
	$(MAKE) -j2 check-api check-ui

check-api:
	cd api && npm run format && npm run lint && npm run build

check-ui:
	cd ui && bun run lint && bun run build

kill:
	@lsof -ti :$API_PORT | xargs kill -9 2>/dev/null || true
	@lsof -ti :5173 | xargs kill -9 2>/dev/null || true

dev: kill
	cd api && npm start & cd ui && bun dev
```

- Run `make check` to ensure everything is fine

- Generate a `CLAUDE.md` at the project root, including notes on:
  - Project structure: root with `api/` (NestJS, npm) and `ui/` (React + Vite, bun)
  - Commands: `make dev`, `make kill`, `make check`
  - API conventions: one-action-per-controller (method named `invoke`), barrel exports via `index.ts`, feature modules with `controllers/`, `services/`, `dtos/` directories
  - UI conventions: `@/` path alias, lazy-loaded routes, React Query for server state, pages in `src/apps/{feature}/pages/{PageName}/PageName.tsx`
  - Always run `make check` before completing
