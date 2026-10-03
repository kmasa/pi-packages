# Pi Packages

Nx monorepo for public npm-distributed [Pi](https://pi.dev) packages.

## kmasa fork: Git install

The root Pi manifest loads **only fast-mode**, including `gpt-6.1-sol` support.
Other extensions, skills, prompts, and themes in this monorepo are not enabled by this install.

```bash
pi install git:github.com/kmasa/pi-packages
pi update git:github.com/kmasa/pi-packages
```

Run `/reload` after installing or updating. `/fast` or F3 toggles the persistent
`fast on` / `fast off` footer status. Use `pi --fast` to start enabled in a new session.
Remove any previous `npm:pi-fast` or local fast-mode declaration to avoid duplicate hooks.

Git installs use Pi's TypeScript loader and the published statusline-protocol runtime
package; no monorepo build is required. Root dependency versions use npm-compatible
specifiers because Pi installs Git dependencies with npm by default, which cannot
parse pnpm's `catalog:` specifiers. Workspace packages keep their existing catalogs.
The Nx generators do not provide a root-only manifest for existing nested resources,
so this Git-install adapter is maintained in the root `package.json`.

## Packages

| Workspace                              | npm package                                         | Description                                                                         | Install                                                            |
| -------------------------------------- | --------------------------------------------------- | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| `packages/angular-developer`           | `@aliaksei-raketski/pi-angular-developer`           | Angular developer skill with local documentation helper scripts.                    | `pi install npm:@aliaksei-raketski/pi-angular-developer`           |
| `packages/continuation-gate-inspector` | `@aliaksei-raketski/pi-continuation-gate-inspector` | Read-only local inspector for continuation-gate diagnostics.                        | `pi install npm:@aliaksei-raketski/pi-continuation-gate-inspector` |
| `packages/continuation-gate-protocol`  | `@aliaksei-raketski/pi-continuation-gate-protocol`  | Shared continuation-gate protocol for cooperating Pi extensions.                    | `pi install npm:@aliaksei-raketski/pi-continuation-gate-protocol`  |
| `packages/caffeinate`                  | `@aliaksei-raketski/pi-caffeinate`                  | Keeps the computer awake while Pi or continuation work is active.                   | `pi install npm:@aliaksei-raketski/pi-caffeinate`                  |
| `packages/fast-mode`                   | `@aliaksei-raketski/pi-fast-mode`                   | Extension that enables fast-mode payload tuning for supported Claude/OpenAI models. | `pi install npm:@aliaksei-raketski/pi-fast-mode`                   |
| `packages/goal`                        | `@aliaksei-raketski/pi-goal`                        | Persistent, evidence-gated thread goals and continuation support.                   | `pi install npm:@aliaksei-raketski/pi-goal`                        |
| `packages/nx-developer`                | `@aliaksei-raketski/pi-nx-developer`                | Nx workspace development and documentation skills.                                  | `pi install npm:@aliaksei-raketski/pi-nx-developer`                |
| `packages/statusline`                  | `@aliaksei-raketski/pi-statusline`                  | Extension for a customizable, ANSI-aware statusline footer.                         | `pi install npm:@aliaksei-raketski/pi-statusline`                  |
| `packages/statusline-protocol`         | `@aliaksei-raketski/pi-statusline-protocol`         | Shared source-aware structured status protocol.                                     | `pi install npm:@aliaksei-raketski/pi-statusline-protocol`         |
| `packages/taiga-ui-docs`               | `@aliaksei-raketski/pi-taiga-ui-docs`               | Taiga UI docs skill backed by a bundled helper script.                              | `pi install npm:@aliaksei-raketski/pi-taiga-ui-docs`               |
| `packages/tmux-bash`                   | `@aliaksei-raketski/pi-tmux-bash`                   | Tmux-backed command execution with bounded output and continuation gates.           | `pi install npm:@aliaksei-raketski/pi-tmux-bash`                   |
| `packages/tmux-bash-core`              | `@aliaksei-raketski/pi-tmux-bash-core`              | Compiled read-only scope, manifest, metadata, discovery, and attach helpers.        | `npm install @aliaksei-raketski/pi-tmux-bash-core`                 |

## Development

Install dependencies:

```bash
pnpm install
```

List Nx projects:

```bash
pnpm nx show projects
```

Inspect a project:

```bash
pnpm nx show project @aliaksei-raketski/pi-fast-mode --json
```

Run checks for all Pi packages:

```bash
pnpm nx run-many --projects='packages/*' -t lint,typecheck
```

Run all configured test targets:

```bash
pnpm nx run-many -t test --parallel=3
```

Explore the workspace graph:

```bash
pnpm nx graph
```

## Generating Pi packages and components

Use the local Nx generators instead of hand-rolling package metadata or Pi component folders.

Create a package container:

```bash
pnpm nx g @aliaksei-raketski/nx-pi:package my-package --dry-run --no-interactive
pnpm nx g @aliaksei-raketski/nx-pi:package my-package --no-interactive
```

Add components to an existing Pi package:

```bash
pnpm nx g @aliaksei-raketski/nx-pi:skill my-skill \
  --project=@aliaksei-raketski/pi-my-package \
  --no-interactive

pnpm nx g @aliaksei-raketski/nx-pi:prompt my-prompt \
  --project=@aliaksei-raketski/pi-my-package \
  --no-interactive

pnpm nx g @aliaksei-raketski/nx-pi:theme my-theme \
  --project=@aliaksei-raketski/pi-my-package \
  --no-interactive

pnpm nx g @aliaksei-raketski/nx-pi:extension my-extension \
  --project=@aliaksei-raketski/pi-my-package \
  --no-interactive
```

Check available options before applying changes:

```bash
pnpm nx g @aliaksei-raketski/nx-pi:package --help
pnpm nx g @aliaksei-raketski/nx-pi:skill --help
pnpm nx g @aliaksei-raketski/nx-pi:extension --help
```

## Trying extensions locally

```bash
pi -e ./packages/fast-mode
pi -e ./packages/statusline
```

## Syncing vendored skills

Some skill packages include their own maintenance scripts. For example:

```bash
pnpm --filter @aliaksei-raketski/pi-angular-developer sync
```

## Publishing

Packages are intended to be published to the public npm registry. Use Nx project metadata and package-level `package.json` versions as the source of truth during release automation.
