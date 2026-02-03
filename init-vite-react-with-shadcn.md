1. Create a new project

- Run this script to create a new project

```shell
bun create vite --template react-ts --no-interactive $FOLDER
cd $FOLDER
bun install
```

2. Setup eslint & prettier

- Run this script to install additional eslint plugins

```shell
bun add -D eslint-config-prettier eslint-plugin-simple-import-sort
```

- Update the `eslint.config.js` to add new plugin into

```js
import eslintConfigPrettier from "eslint-config-prettier/flat";
import eslintPluginSimpleImportSort from "eslint-plugin-simple-import-sort";

// ...

export default defineConfig([
    globalIgnores([
        "dist",
        "src/components/ui", // shadcn/ui
    ]),
    {
        // ...
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

- Run this script to install `prettier`

```shell
bun add -D --exact prettier prettier-plugin-tailwindcss
```

- Create `.prettierrc` to add the config into

```json
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

- Create `.prettierignore` to copy content `.gitignore` into, also appending new line:

```
# shadcn/ui
src/components/ui
```

- Update the `package.json` to add these new scripts

```
"lint": "eslint . && prettier --check .",
"lint:fix": "eslint . --fix && prettier --write .",
```

3. Install tailwindcss

- Run this script to install TailwindCSS

```shell
bun add -D tailwindcss @tailwindcss/vite
```

- Update the `vite.config.ts` to have the new plugin

```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

// https://vite.dev/config/
export default defineConfig({
  plugins: [react(), tailwindcss()],
})
```

- Move the `src/index.css` into `src/styles/globals.css`, replace its content by

```
@import "tailwindcss";
```

- Update the `src/App.tsx` to reflect the new path to the `globals.css` import.

4. Install shadcn/ui

- Update the `tsconfig.json` file

```
{
  // ...
  "compilerOptions": {
    // ...
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

- Update the `tsconfig.app.json` file

```
{
  // ...
  "compilerOptions": {
    // ...
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

- Run this script to install `@types/node`

```shell
bun add -D @types/node
```

- Update `vite.config.ts` to has alias

```
import path from "path"

// ...
 
export default defineConfig({
  // ...
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
})
```

- Run this script to init the shadcn/ui

```shell
bunx --bun shadcn@latest init --template vite --base-color neutral --yes
```

- Testing the additional with this script

```shell
bunx --bun shadcn@latest add button
```
