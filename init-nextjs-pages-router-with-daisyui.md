# Init Next.js `pages` router with `daisyUI`

### Step 1

- Run `bun init`, blank template at the folder, or the subfolder
- Remove `tsconfig.json`, `index.ts`, `.gitignore`
- Download https://raw.githubusercontent.com/github/gitignore/refs/heads/main/Node.gitignore into `./.gitignore`, adding
  these lines into the end of the file

```
# JetBrains
.idea
```

- Remove `module` and `type` out of `package.json`

### Step 2

- Run `bun add next@latest react@latest react-dom@latest`
- Add these following script into `package.json`

```
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "eslint",
    "lint:fix": "eslint --fix"
  }
```

- Create `./src/pages/index.tsx`, adding this content

```
export default function Page() {
  return <p>Lorem ipsum dolor sit amet</p>
}
```

- Create `./src/pages/_app.tsx`, adding this content

```
import type { AppProps } from 'next/app'

export default function App({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />
}
```

- Create `./src/pages/_document.tsx`, adding this content

```
import { Html, Head, Main, NextScript } from 'next/document'

export default function Document() {
  return (
    <Html>
      <Head />
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  )
}
```

- Create empty `./public/.gitkeep` file

- Run `bun run build` to ensure everything is fine

### Step 3

- Run `bun add -D tailwindcss @tailwindcss/postcss postcss`

- Create `postcss.config.mjs`, adding this content into

```
const config = {
  plugins: {
    "@tailwindcss/postcss": {},
  },
};
export default config;
```

- Create `./src/styles/globals.css`, adding this content into

```
@import "tailwindcss";
```

- Update `./src/pages/_app.tsx` to append this content into the first line

```
import '../styles/globals.css'
```

- Run `bun run build` to ensure everything is fine

### Step 4

- Run `bun add -D daisyui@latest`

- Update `./src/styles/globals.css`, replacing its content by

```
@import "tailwindcss";
@plugin "daisyui";
```

- Append these lines into `tsconfig.json`, inside the `compilerOptions`

```
"baseUrl": ".",
"paths": {
  "@/*": ["./src/*"]
}
```

- Update `./src/pages/_document.tsx` to set the default theme, replacing its content by

```
import { Html, Head, Main, NextScript } from 'next/document'

export default function Document() {
  return (
    <Html data-theme="light">
      <Head />
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  )
}
```

- Run `bun run build` to ensure everything is fine

### Step 5

- Run `bun add -D eslint eslint-config-next eslint-config-prettier eslint-plugin-simple-import-sort`
- Create `eslint.config.mjs`, adding this content

```jsx
import { defineConfig, globalIgnores } from 'eslint/config'
import eslintConfigPrettier from "eslint-config-prettier/flat";
import eslintPluginSimpleImportSort from "eslint-plugin-simple-import-sort";
import nextVitals from 'eslint-config-next/core-web-vitals'

const eslintConfig = defineConfig([
  ...nextVitals,
  // Override default ignores of eslint-config-next.
  globalIgnores([
    // Default ignores of eslint-config-next:
    '.next/**',
    'out/**',
    'build/**',
    'next-env.d.ts',
  ]),
  // eslint-plugin-simple-import-sort
  {
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
])

export default eslintConfig
```

- Run `bun add --dev --exact prettier prettier-plugin-tailwindcss`
- Create `.prettierrc`, adding this content

```
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

- Create `.prettierignore`, copy content `.gitignore` into it

- Update `package.json` to modify the lint into include the prettier

```
"lint": "eslint && prettier --check .",
"lint:fix": "eslint --fix && prettier --write .",
```

### Step 6

- To configure daisyUI themes, update `./src/styles/globals.css` to specify themes

```
@import "tailwindcss";
@plugin "daisyui" {
  themes: light --default, dark --prefersdark;
}
```

- Create `src/components/layout/layout.tsx`, adding this

```
import { ComponentProps } from "react";

interface LayoutProps extends ComponentProps<"main"> {
  //
}

export const Layout = (props: LayoutProps) => {
  const { children, ...otherProps } = props;

  return (
    <div className="drawer lg:drawer-open">
      <input id="app-drawer" type="checkbox" className="drawer-toggle" />

      <div className="drawer-content flex flex-col">
        <div className="navbar bg-base-100 shadow-sm lg:hidden">
          <div className="navbar-start">
            <label
              htmlFor="app-drawer"
              className="btn btn-ghost btn-square"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                className="inline-block h-5 w-5 stroke-current"
              >
                <path
                  strokeLinecap="round"
                  strokeLinejoin="round"
                  strokeWidth="2"
                  d="M4 6h16M4 12h16M4 18h16"
                />
              </svg>
            </label>
          </div>
          <div className="navbar-center">
            <a className="btn btn-ghost text-xl">App</a>
          </div>
          <div className="navbar-end" />
        </div>

        <main {...otherProps}>{children}</main>
      </div>

      <div className="drawer-side">
        <label
          htmlFor="app-drawer"
          aria-label="close sidebar"
          className="drawer-overlay"
        />
        <aside className="bg-base-200 min-h-full w-64 p-4">
          <div className="mb-4 text-xl font-bold">App</div>
          <ul className="menu">
            <li>
              <a className="menu-active">Dashboard</a>
            </li>
            <li>
              <a>Settings</a>
            </li>
          </ul>
        </aside>
      </div>
    </div>
  );
};
```

- Replace `src/pages/index.tsx` by this

```
import { Layout } from "@/components/layout/layout";

export default function Page() {
  return (
    <Layout className="p-4">
      <p>Lorem ipsum dolor sit amet</p>
    </Layout>
  );
}
```

### Step 7

- Run `bun lint` and `bun lint:fix` to clean-up the codebase, ready to go
