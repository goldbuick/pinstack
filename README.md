# Pinstack

## Package manager

**Yarn Classic (v1).** Install dependencies with `yarn`. Lockfile is `yarn.lock`; `package-lock.json` is gitignored so npm doesn’t overwrite Yarn’s tree.

`.yarnrc` sets `--ignore-engines true` so installs succeed on Node versions some ESLint deps mark as unsupported. You may see a peer warning for `@tailwindcss/vite` vs Vite 8; the app still builds.

## Git & GitHub

### First-time setup (initial commit)

If this folder is not a git repo yet:

```bash
cd /path/to/pinstack
git init
git add -A
git status   # review
git commit -m "Initial commit: Pinstack (Vite, React, TS, Tailwind)"
```

Use `main` as the default branch (Git’s default on recent versions):

```bash
git branch -M main
```

If you **already committed** and only need to push, skip to [Publish to GitHub](#publish-to-github).

### Publish to GitHub

1. On [github.com/new](https://github.com/new), create a repository named **`pinstack`** (or any name you prefer).
2. Leave it **empty**: do **not** add a README, `.gitignore`, or license (avoids merge conflicts).
3. Connect and push:

**HTTPS**

```bash
git remote add origin https://github.com/YOUR_USERNAME/pinstack.git
git push -u origin main
```

**SSH** (if you use SSH keys)

```bash
git remote add origin git@github.com:YOUR_USERNAME/pinstack.git
git push -u origin main
```

Replace `YOUR_USERNAME` with your GitHub username (or org). If the remote already exists from a wrong URL, run `git remote remove origin` first, then add again.

### Optional: GitHub CLI

If you use [`gh`](https://cli.github.com/) and are logged in (`gh auth login`):

```bash
gh repo create pinstack --public --source=. --remote=origin --push
```

(`--private` instead of `--public` if you want a private repo.)

## Lint & format

- **`yarn lint`** — ESLint (includes Prettier via `eslint-plugin-prettier`; formatting issues report as `prettier/prettier`).
- **`yarn lint:fix`** — ESLint with auto-fix (including Prettier fixes where applicable).
- **`yarn format`** — Prettier write (Tailwind class order via `prettier-plugin-tailwindcss`).
- **`yarn format:check`** — Prettier check only.

---

# React + TypeScript + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react) uses [Oxc](https://oxc.rs)
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react-swc) uses [SWC](https://swc.rs/)

## React Compiler

The React Compiler is not enabled on this template because of its impact on dev & build performances. To add it, see [this documentation](https://react.dev/learn/react-compiler/installation).

## Expanding the ESLint configuration

If you are developing a production application, we recommend updating the configuration to enable type-aware lint rules:

```js
export default defineConfig([
  globalIgnores(['dist']),
  {
    files: ['**/*.{ts,tsx}'],
    extends: [
      // Other configs...

      // Remove tseslint.configs.recommended and replace with this
      tseslint.configs.recommendedTypeChecked,
      // Alternatively, use this for stricter rules
      tseslint.configs.strictTypeChecked,
      // Optionally, add this for stylistic rules
      tseslint.configs.stylisticTypeChecked,

      // Other configs...
    ],
    languageOptions: {
      parserOptions: {
        project: ['./tsconfig.node.json', './tsconfig.app.json'],
        tsconfigRootDir: import.meta.dirname,
      },
      // other options...
    },
  },
])
```

You can also install [eslint-plugin-react-x](https://github.com/Rel1cx/eslint-react/tree/main/packages/plugins/eslint-plugin-react-x) and [eslint-plugin-react-dom](https://github.com/Rel1cx/eslint-react/tree/main/packages/plugins/eslint-plugin-react-dom) for React-specific lint rules:

```js
// eslint.config.js
import reactX from 'eslint-plugin-react-x'
import reactDom from 'eslint-plugin-react-dom'

export default defineConfig([
  globalIgnores(['dist']),
  {
    files: ['**/*.{ts,tsx}'],
    extends: [
      // Other configs...
      // Enable lint rules for React
      reactX.configs['recommended-typescript'],
      // Enable lint rules for React DOM
      reactDom.configs.recommended,
    ],
    languageOptions: {
      parserOptions: {
        project: ['./tsconfig.node.json', './tsconfig.app.json'],
        tsconfigRootDir: import.meta.dirname,
      },
      // other options...
    },
  },
])
```
