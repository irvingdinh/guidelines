# Init Expo

### Step 1

- Run

```
bun create expo-app <PATH> --yes
```

### Step 2

Update the README.md to use `bun`, not `node`, `npm` or `npx`.

### Step 3

- Run 

```
bun add -D eslint-config-prettier eslint-plugin-simple-import-sort
```

- Update the content of `eslint.config.js` to

```
// https://docs.expo.dev/guides/using-eslint/
const { defineConfig } = require("eslint/config");
const expoConfig = require("eslint-config-expo/flat");
const eslintConfigPrettier = require("eslint-config-prettier/flat");
const eslintPluginSimpleImportSort = require("eslint-plugin-simple-import-sort");

module.exports = defineConfig([
  expoConfig,
  {
    ignores: ["dist/*"],
    files: ["**/*.{ts,tsx}"],
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

- Run 

```
bun add --dev --exact prettier
```

- Create `.prettierrc`, adding this content

```
{}
```

- Create `.prettierignore`, copy content `.gitignore` into it

- Update `package.json` to has these script

```
"lint": "expo lint && prettier --check .",
"lint:fix": "expo lint --fix && prettier --write ."
```

- Run `bun lint` to ensure everything is fine

### Step 4

- Run `bunx expo install expo-dev-client`
- Run `bunx expo run:ios --device`
