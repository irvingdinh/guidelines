# Init NestJS

### Step 1

- Run `bun init`, blank template at the folder, or the subfolder
- Remove `index.ts`, `.gitignore`
- Download https://raw.githubusercontent.com/github/gitignore/refs/heads/main/Node.gitignore into `./.gitignore`, adding
  these lines into the end of the file

```
# JetBrains
.idea
```

- Remove `module` and `type` out of `package.json`

### Step 2

- Run

```
bun add @nestjs/common @nestjs/core @nestjs/platform-express reflect-metadata rxjs
```

- Run

```
bun add -D @nestjs/cli @nestjs/schematics @nestjs/testing typescript @types/node @types/express ts-loader
```

- Add these following scripts into `package.json`

```
  "scripts": {
    "build": "nest build",
    "start": "nest start",
    "start:dev": "nest start --watch",
    "start:debug": "nest start --debug --watch",
    "start:prod": "node dist/main",
    "lint": "eslint",
    "lint:fix": "eslint --fix",
    "test": "jest",
    "test:watch": "jest --watch",
    "test:cov": "jest --coverage",
    "test:e2e": "jest --config ./test/jest-e2e.json"
  }
```

### Step 3

- Create `tsconfig.json`, adding this content

```
{
  "compilerOptions": {
    "module": "commonjs",
    "declaration": true,
    "removeComments": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "allowSyntheticDefaultImports": true,
    "target": "ES2021",
    "sourceMap": true,
    "outDir": "./dist",
    "baseUrl": "./",
    "incremental": true,
    "skipLibCheck": true,
    "strictNullChecks": true,
    "noImplicitAny": true,
    "strictBindCallApply": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

- Create `tsconfig.build.json`, adding this content

```
{
  "extends": "./tsconfig.json",
  "exclude": ["node_modules", "test", "dist", "**/*spec.ts"]
}
```

- Create `nest-cli.json`, adding this content

```
{
  "$schema": "https://json.schemastore.org/nest-cli",
  "collection": "@nestjs/schematics",
  "sourceRoot": "src",
  "compilerOptions": {
    "deleteOutDir": true
  }
}
```

### Step 4

- Create `./src/main.ts`, adding this content

```
import { NestFactory } from '@nestjs/core';

import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(process.env.PORT ?? 3000);
}
bootstrap();
```

- Create `./src/app.module.ts`, adding this content

```
import { Module } from '@nestjs/common';

import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

- Create `./src/app.controller.ts`, adding this content

```
import { Controller, Get } from '@nestjs/common';

import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
}
```

- Create `./src/app.service.ts`, adding this content

```
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello World!';
  }
}
```

- Run `bun run build` to ensure everything is fine

### Step 5

- Run `bun add -D eslint @eslint/js eslint-config-prettier eslint-plugin-simple-import-sort typescript-eslint`
- Create `eslint.config.mjs`, adding this content

```js
import eslint from '@eslint/js';
import eslintConfigPrettier from 'eslint-config-prettier/flat';
import eslintPluginSimpleImportSort from 'eslint-plugin-simple-import-sort';
import tseslint from 'typescript-eslint';

export default tseslint.config(
  {
    ignores: ['dist/**', 'node_modules/**'],
  },
  eslint.configs.recommended,
  ...tseslint.configs.recommended,
  {
    files: ['**/*.{ts,tsx}'],
    plugins: {
      'simple-import-sort': eslintPluginSimpleImportSort,
    },
    rules: {
      'simple-import-sort/imports': 'error',
      'simple-import-sort/exports': 'error',
      '@typescript-eslint/interface-name-prefix': 'off',
      '@typescript-eslint/explicit-function-return-type': 'off',
      '@typescript-eslint/explicit-module-boundary-types': 'off',
      '@typescript-eslint/no-explicit-any': 'off',
    },
  },
  eslintConfigPrettier,
);
```

- Run `bun add --dev --exact prettier prettier-plugin-organize-imports`
- Create `.prettierrc`, adding this content

```
{
  "singleQuote": true,
  "trailingComma": "all"
}
```

- Create `.prettierignore`, copy content `.gitignore` into it

- Update `package.json` to modify the lint scripts to include prettier

```
"lint": "eslint && prettier --check .",
"lint:fix": "eslint --fix && prettier --write .",
```

### Step 6

- Run `bun add -D jest @types/jest ts-jest @nestjs/testing supertest @types/supertest`

- Create `jest.config.ts`, adding this content

```
import type { Config } from 'jest';

const config: Config = {
  moduleFileExtensions: ['js', 'json', 'ts'],
  rootDir: 'src',
  testRegex: '.*\\.spec\\.ts$',
  transform: {
    '^.+\\.(t|j)s$': 'ts-jest',
  },
  collectCoverageFrom: ['**/*.(t|j)s'],
  coverageDirectory: '../coverage',
  testEnvironment: 'node',
  moduleNameMapper: {
    '^@/(.*)$': '<rootDir>/$1',
  },
};

export default config;
```

- Create `./test/jest-e2e.json`, adding this content

```
{
  "moduleFileExtensions": ["js", "json", "ts"],
  "rootDir": ".",
  "testEnvironment": "node",
  "testRegex": ".e2e-spec.ts$",
  "transform": {
    "^.+\\.(t|j)s$": "ts-jest"
  }
}
```

- Create `./src/app.controller.spec.ts`, adding this content

```
import { Test, TestingModule } from '@nestjs/testing';

import { AppController } from './app.controller';
import { AppService } from './app.service';

describe('AppController', () => {
  let appController: AppController;

  beforeEach(async () => {
    const app: TestingModule = await Test.createTestingModule({
      controllers: [AppController],
      providers: [AppService],
    }).compile();

    appController = app.get<AppController>(AppController);
  });

  describe('root', () => {
    it('should return "Hello World!"', () => {
      expect(appController.getHello()).toBe('Hello World!');
    });
  });
});
```

- Create `./test/app.e2e-spec.ts`, adding this content

```
import { INestApplication } from '@nestjs/common';
import { Test, TestingModule } from '@nestjs/testing';
import * as request from 'supertest';

import { AppModule } from './../src/app.module';

describe('AppController (e2e)', () => {
  let app: INestApplication;

  beforeEach(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    await app.init();
  });

  it('/ (GET)', () => {
    return request(app.getHttpServer())
      .get('/')
      .expect(200)
      .expect('Hello World!');
  });
});
```

### Step 7

- Run `bun run build` to ensure everything is fine
- Run `bun lint` and `bun lint:fix` to clean-up the codebase, ready to go
