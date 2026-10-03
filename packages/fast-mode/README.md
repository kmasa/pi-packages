# @aliaksei-raketski/pi-fast-mode

A Pi extension that enables fast mode for supported models with one command and shortcut:

- **`/fast`**: toggles fast mode on/off.
- **`F3`**: toggles fast mode on/off.
- No arguments. `/fast` always toggles.

The current model's API and ID determine what gets injected. Any provider that speaks the model's API is supported, including proxies such as LiteLLM that prefix IDs with `anthropic/` or `openai/`.

- **Claude Opus 4.6 / 4.8 / 5 / 5.5** (`anthropic-messages`)
  - Adds `speed: "fast"`
  - Adds required header `anthropic-beta: fast-mode-2026-02-01`
- **GPT-5.4 / GPT-5.5 / GPT-5.6 Luna, Sol, and Terra / GPT-6 Astra, Sol, and Luna / GPT-6.1 Sol**
  - Adds `service_tier: "priority"`
  - `openai-codex-responses` requires ChatGPT/OAuth auth (API-key models are skipped)
  - `openai-responses` works with API-key auth

## kmasa fork

This fork adds `gpt-6.1-sol` support for OpenAI Responses and OAuth-backed Codex Responses.
The repository root exposes only fast-mode; the other monorepo resources are not loaded.

```bash
pi install git:github.com/kmasa/pi-packages
```

Remove `npm:pi-fast` before using this package to avoid duplicate `/fast` commands and request hooks.
Restart Pi or run `/reload` after installation. Use `/fast` or F3 to toggle; start with `pi --fast` to enable it in a new session.

To update after changes are pushed to the fork:

```bash
pi update git:github.com/kmasa/pi-packages
# or update all installed packages
pi update --extensions
```

Then run `/reload` in Pi. No local workspace install or build is required.
If migrating from a local-path installation, remove that package declaration to avoid loading fast-mode twice.

## Install (upstream npm package)

```bash
pi install npm:@aliaksei-raketski/pi-fast-mode
# or project-local
pi install -l npm:@aliaksei-raketski/pi-fast-mode
```

Try locally from the repository root:

```bash
pi -e ./packages/fast-mode
```

This package ships the extension as TypeScript source. Pi loads extension entrypoints with its runtime TypeScript loader, so no JavaScript build output is required for this package.

## Package contents

- `extensions/fast-mode/index.ts` Pi extension entrypoint.
- Fast-mode request hooks and session state helpers.

## Behavior

- Start Pi with fast mode enabled:

```bash
pi --fast
```

- Footer status is always visible as one of:

  - `fast on` (accent color)
  - `fast off` (gray)

When fast mode is enabled but the current model is not supported, status is:

- `no fast` in the warning color (enabled, but inactive for the current model)

When you switch to a supported model, fast mode is applied automatically if it is enabled.

The fast mode toggle is stored in the current session, so it survives `/reload`, resume, and branch navigation.
