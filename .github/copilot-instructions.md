<!-- Copilot instructions for working in this Vue 3 + Vite + TypeScript template -->
# Copilot Instructions — new-app

Keep guidance concise and strictly tied to the repository's patterns.

- Project type: Vue 3 single-file components (SFCs) with TypeScript and Vite.
- Key files: [package.json](package.json), [vite.config.ts](vite.config.ts), [src/main.ts](src/main.ts), [src/App.vue](src/App.vue), [README.md](README.md)

What to do first
- Run `npm install`, then `npm run dev` to start the dev server (see `package.json` scripts).
- Use `npm run build` for production build; `npm run type-check` runs `vue-tsc` for `.vue` type checks.

Architecture & patterns to follow
- Entrypoint: `src/main.ts` mounts the app (`createApp(App).mount('#app')`). Keep root-level state or global plugins initialized here.
- Components: use SFCs with `<script setup lang="ts">`. Prefer top-level props and define composables under `src/`.
- Aliases: the project maps `@` -> `src` in `vite.config.ts` — prefer imports like `@/components/MyComp.vue`.
- Devtools: `vite-plugin-vue-devtools` is included; avoid removing it without updating `vite.config.ts` and README notes.

Build / Dev workflows
- Development: `npm run dev` (Vite dev server).
- Type checking: `npm run type-check` uses `vue-tsc --build`. Run this before CI/promote.
- Build: `npm run build` wraps `vite build`; use `npm run preview` to locally preview a production build.

Conventions & project-specific notes
- TypeScript: repo uses `vue-tsc` for `.vue` types; editors should use Volar (see `README.md`).
- No automated tests present—do not add assumptions about test frameworks.
- Keep `type: "module"` in `package.json` and Node engine ranges unchanged unless upgrading Node intentionally.
- Files under `src/` are expected to be ES modules; use relative imports or the `@` alias consistently.

Integration points & dependencies
- `vite` and `@vitejs/plugin-vue` drive dev/build. Changes to plugin config belong in `vite.config.ts`.
- `vue` dependency is v3; use composition API and `<script setup>` patterns.
- Dev dependency `vite-plugin-vue-devtools` adds dev-only integrations; treat it as optional but useful for debugging.

When editing code
- Preserve `<script setup lang="ts">` format for SFCs; place composables in `src/composables` if needed.
- Update `tsconfig*.json` only if you fully understand cross-file TypeScript impacts (type-checking is centralized via `vue-tsc`).
- For new global aliases or plugin initialization, update `vite.config.ts` and `src/main.ts` together.

Examples (from this repo)
- Mounting app: see `src/main.ts`.
- Alias usage: `@` -> `src` configured in `vite.config.ts`.
- Scripts: `dev`, `build`, `preview`, `type-check` in `package.json`.

If uncertain
- Ask for clarification before making cross-cutting changes (build config, tsconfigs, or dependency upgrades).

This file is deliberately short — tell me if you want additional checks (linting, test harness, or CI steps) added.
