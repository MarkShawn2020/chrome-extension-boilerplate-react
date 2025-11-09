# Repository Guidelines

## Project Structure & Module Organization
- `src/` hosts the React 18 extension code: components under `components/`, feature logic in `hooks/` or `services/`, shared styles in `styles/`, and manifests in `manifest/`. Keep popup/options/background concerns isolated in their own subfolders to avoid cross-coupling.
- `utils/` contains Node helpers (`build.js`, `webserver.js`) that wrap Webpack for production bundles and local preview packaging; prefer extending these scripts over duplicating logic elsewhere.
- Build artifacts land in `build/`, and zipped browser distributions live under `zip/`. Never edit either by hand—regenerate via the scripts.

## Build, Test, and Development Commands
- `pnpm install` — sync dependencies; run after pulling anything that touches `package.json` or lockfiles.
- `pnpm start` — launches `utils/webserver.js` for hot reloading against Chromium-based browsers. Use Chrome’s “Load unpacked” pointing at `build/`.
- `pnpm build` — produces an optimized extension bundle plus zipped package in `zip/`.
- `pnpm prettier` — formats all supported sources; run before committing UI-heavy work.

## Coding Style & Naming Conventions
- Stick to TypeScript in new modules; React function components only. Use 2-space indentation, named exports for reusable units, and suffix hooks with `use*`.
- Components live in PascalCase files (`PopupPanel.tsx`), hooks in camelCase (`useTelemetry.ts`), and utility helpers in snake-free camelCase.
- Keep CSS/SCSS colocated with the component and import via relative paths; prefer CSS Modules over global rules unless editing `global.scss`.
- Lint via `eslint` (configured through `eslint-config-react-app`); honor Prettier defaults—no manual alignment tweaks.

## Testing Guidelines
- There is no automated suite yet; for complex logic, add lightweight unit tests under `src/__tests__/` using Jest (mirroring React app conventions) and reference them in PRs.
- Manual verification is required: rebuild, reload the extension in Chrome, and capture screenshots/GIFs that prove popup, options, and background behaviors.
- Document any feature flags or permissions added to `manifest.json` so reviewers can reproduce your setup quickly.

## Commit & Pull Request Guidelines
- Use semantic commit headers (`feat:`, `fix:`, `chore:`); keep scopes short (`feat(popup): add search`). One logical change per commit.
- PRs should summarize intent, list user-facing impacts, and attach extension screenshots for UI adjustments. Link to relevant issues and mention any follow-up tasks.
- Ensure `pnpm build` is green before requesting review; include notes about manual tests (e.g., “Reloaded in Chrome 120, verified context menu”).***
