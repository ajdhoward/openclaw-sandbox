# openclaw-sandbox

OpenClaw (formerly Moltbot) running inside a Cloudflare Sandbox Worker.

Consolidation target for 5 former repositories, per `migration-manifest.csv`. This cluster collapses to
a **single app**, not a multi-app monorepo: five byte-identical copies differed only in per-deployment
identity, which is configuration.

## What lands here

| Source repository | Becomes |
|---|---|
| `openclawbot-sandbox` | repo root (seed: `src/`, `Dockerfile`, `wrangler.jsonc`, `vite.config.ts`) |
| `moltbot-sandbox` | `envs/moltbot/wrangler.jsonc` extracted, then deleted |
| `moltbot-sandbox1` | `envs/moltbot-1/wrangler.jsonc` extracted, then deleted |
| `jude-claw` | `envs/jude/wrangler.jsonc` extracted, then deleted |
| `irishstu` | `envs/irishstu/wrangler.jsonc` extracted, then deleted |

## Layout

- `src/` the Worker
- `envs/` one directory per deployment identity, config only

## Before the four copies go

Diff each copy's `wrangler.jsonc` (and any committed `wrangler.toml`) against the seed. That config is
the only thing worth extracting; the code is the same file for file.

## Status

Scaffold only. Nothing imported. This repository is public because every carried source repository is
public — keep it free of secrets and deployment-specific values.
