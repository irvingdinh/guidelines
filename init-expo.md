# Init Expo

### Step 1

- Run

```
bun create expo-app <PATH> --yes
```

- CD to the folder, run

```
bun run scripts/reset-project.js
```

### Step 2

- Update the README.md to use `bun`, not `node`, `npm` or `npx`.

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
bun add --dev --exact prettier prettier-plugin-tailwindcss
```

- Create `.prettierrc`, adding this content

```
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

- Create `.prettierignore`, copy content `.gitignore` into it

- Update `package.json` to has these script

```
"lint": "expo lint && prettier --check .",
"lint:fix": "expo lint --fix && prettier --write ."
```

- Run `bun lint` to ensure everything is fine

### Step 4

- Run

```
bunx skills add https://github.com/expo/skills --skill native-data-fetching --agent claude-code cursor --yes
bunx skills add https://github.com/expo/skills --skill building-native-ui --agent claude-code cursor --yes
```

- Following `https://raw.githubusercontent.com/expo/skills/refs/heads/main/plugins/expo-app-design/skills/expo-tailwind-setup/SKILL.md` to install and setup Nativewind, but always use `bun` and `bunx`

- Generate CLAUDE.md file, adding note to always use `bun`, `bunx`, and guideline on always using `nativewindcss` instead of create stylesheet.

### Step 5

- Run `bunx expo install expo-dev-client`
- Run `bunx expo run:ios --device`
