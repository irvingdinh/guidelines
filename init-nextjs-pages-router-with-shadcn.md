# Init Next.js `pages` router with `shadcn/ui`

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

- Run

```
bun add -D class-variance-authority clsx tailwind-merge lucide-react tw-animate-css
```

- Append these lines into `tsconfig.json` , inside the `compilerOptions`

```
"baseUrl": ".",
"paths": {
  "@/*": ["./src/*"]
}
```

- Replace `./src/styles/globals.css` content by this

```
@import "tailwindcss";
@import "tw-animate-css";

@custom-variant dark (&:is(.dark *));

:root {
  --background: oklch(1 0 0);
  --foreground: oklch(0.145 0 0);
  --card: oklch(1 0 0);
  --card-foreground: oklch(0.145 0 0);
  --popover: oklch(1 0 0);
  --popover-foreground: oklch(0.145 0 0);
  --primary: oklch(0.205 0 0);
  --primary-foreground: oklch(0.985 0 0);
  --secondary: oklch(0.97 0 0);
  --secondary-foreground: oklch(0.205 0 0);
  --muted: oklch(0.97 0 0);
  --muted-foreground: oklch(0.556 0 0);
  --accent: oklch(0.97 0 0);
  --accent-foreground: oklch(0.205 0 0);
  --destructive: oklch(0.577 0.245 27.325);
  --destructive-foreground: oklch(0.577 0.245 27.325);
  --border: oklch(0.922 0 0);
  --input: oklch(0.922 0 0);
  --ring: oklch(0.708 0 0);
  --chart-1: oklch(0.646 0.222 41.116);
  --chart-2: oklch(0.6 0.118 184.704);
  --chart-3: oklch(0.398 0.07 227.392);
  --chart-4: oklch(0.828 0.189 84.429);
  --chart-5: oklch(0.769 0.188 70.08);
  --radius: 0.625rem;
  --sidebar: oklch(0.985 0 0);
  --sidebar-foreground: oklch(0.145 0 0);
  --sidebar-primary: oklch(0.205 0 0);
  --sidebar-primary-foreground: oklch(0.985 0 0);
  --sidebar-accent: oklch(0.97 0 0);
  --sidebar-accent-foreground: oklch(0.205 0 0);
  --sidebar-border: oklch(0.922 0 0);
  --sidebar-ring: oklch(0.708 0 0);
}

.dark {
  --background: oklch(0.145 0 0);
  --foreground: oklch(0.985 0 0);
  --card: oklch(0.145 0 0);
  --card-foreground: oklch(0.985 0 0);
  --popover: oklch(0.145 0 0);
  --popover-foreground: oklch(0.985 0 0);
  --primary: oklch(0.985 0 0);
  --primary-foreground: oklch(0.205 0 0);
  --secondary: oklch(0.269 0 0);
  --secondary-foreground: oklch(0.985 0 0);
  --muted: oklch(0.269 0 0);
  --muted-foreground: oklch(0.708 0 0);
  --accent: oklch(0.269 0 0);
  --accent-foreground: oklch(0.985 0 0);
  --destructive: oklch(0.396 0.141 25.723);
  --destructive-foreground: oklch(0.637 0.237 25.331);
  --border: oklch(0.269 0 0);
  --input: oklch(0.269 0 0);
  --ring: oklch(0.439 0 0);
  --chart-1: oklch(0.488 0.243 264.376);
  --chart-2: oklch(0.696 0.17 162.48);
  --chart-3: oklch(0.769 0.188 70.08);
  --chart-4: oklch(0.627 0.265 303.9);
  --chart-5: oklch(0.645 0.246 16.439);
  --sidebar: oklch(0.205 0 0);
  --sidebar-foreground: oklch(0.985 0 0);
  --sidebar-primary: oklch(0.488 0.243 264.376);
  --sidebar-primary-foreground: oklch(0.985 0 0);
  --sidebar-accent: oklch(0.269 0 0);
  --sidebar-accent-foreground: oklch(0.985 0 0);
  --sidebar-border: oklch(0.269 0 0);
  --sidebar-ring: oklch(0.439 0 0);
}

@theme inline {
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  --color-card: var(--card);
  --color-card-foreground: var(--card-foreground);
  --color-popover: var(--popover);
  --color-popover-foreground: var(--popover-foreground);
  --color-primary: var(--primary);
  --color-primary-foreground: var(--primary-foreground);
  --color-secondary: var(--secondary);
  --color-secondary-foreground: var(--secondary-foreground);
  --color-muted: var(--muted);
  --color-muted-foreground: var(--muted-foreground);
  --color-accent: var(--accent);
  --color-accent-foreground: var(--accent-foreground);
  --color-destructive: var(--destructive);
  --color-destructive-foreground: var(--destructive-foreground);
  --color-border: var(--border);
  --color-input: var(--input);
  --color-ring: var(--ring);
  --color-chart-1: var(--chart-1);
  --color-chart-2: var(--chart-2);
  --color-chart-3: var(--chart-3);
  --color-chart-4: var(--chart-4);
  --color-chart-5: var(--chart-5);
  --radius-sm: calc(var(--radius) - 4px);
  --radius-md: calc(var(--radius) - 2px);
  --radius-lg: var(--radius);
  --radius-xl: calc(var(--radius) + 4px);
  --color-sidebar: var(--sidebar);
  --color-sidebar-foreground: var(--sidebar-foreground);
  --color-sidebar-primary: var(--sidebar-primary);
  --color-sidebar-primary-foreground: var(--sidebar-primary-foreground);
  --color-sidebar-accent: var(--sidebar-accent);
  --color-sidebar-accent-foreground: var(--sidebar-accent-foreground);
  --color-sidebar-border: var(--sidebar-border);
  --color-sidebar-ring: var(--sidebar-ring);
}

@layer base {
  * {
    @apply border-border outline-ring/50;
  }
  body {
    @apply bg-background text-foreground;
  }
}
```

- Create `./src/lib/utils.ts`, adding this content

```
import { type ClassValue, clsx } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

- Create `components.json`, adding this content

```
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "new-york",
  "rsc": false,
  "tsx": true,
  "tailwind": {
    "config": "",
    "css": "src/styles/globals.css",
    "baseColor": "neutral",
    "cssVariables": true,
    "prefix": ""
  },
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils",
    "ui": "@/components/ui",
    "lib": "@/lib",
    "hooks": "@/hooks"
  },
  "iconLibrary": "lucide"
}
```

- Run `bunx --bun shadcn@latest add button -y` to add first shadcn/ui component to ensure the setup is complete
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

    // shadcn/ui
    'src/components/ui/**'
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

- Create `.prettierignore`, copy content `.gitignore` into it, appending a new line

```
# shadcn/ui
src/components/ui
```

- Update `package.json` to modify the lint into include the prettier

```
"lint": "eslint && prettier --check .",
"lint:fix": "eslint --fix && prettier --write .",
```

### Step 6

- Run `bunx --bun shadcn@latest add sidebar -y`
- Create `src/components/layout/layout.tsx` , adding this

```
import { ComponentProps } from "react";

import {
  Sidebar,
  SidebarContent,
  SidebarFooter,
  SidebarHeader,
  SidebarInset,
  SidebarProvider,
  SidebarRail,
  SidebarTrigger,
} from "@/components/ui/sidebar";

interface LayoutProps extends ComponentProps<"main"> {
  //
}

export const Layout = (props: LayoutProps) => {
  const { children, ...otherProps } = props;

  return (
    <SidebarProvider>
      <Sidebar variant="inset">
        <SidebarHeader />
        <SidebarContent />
        <SidebarFooter />
        <SidebarRail />
      </Sidebar>

      <SidebarInset>
        <header className="sticky top-0 flex h-16 shrink-0 items-center gap-2">
          <div className="flex flex-1 items-center gap-2 px-3">
            <SidebarTrigger className="-ml-1" />
          </div>
        </header>

        <main {...otherProps}>{children}</main>
      </SidebarInset>
    </SidebarProvider>
  );
};

```

- Replace `src/pages/index.tsx` by this

```
import { Layout } from "@/components/layout/layout";

export default function Page() {
  return (
    <Layout className="px-4">
      <p>Lorem ipsum dolor sit amet</p>
    </Layout>
  );
}
```

### Step 7

- Run `bun lint` and `bun lint:fix` to clean-up the codebase, ready to go
