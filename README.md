# openclaw-sandbox

OpenClaw (formerly Moltbot) running inside a Cloudflare Sandbox Worker: a Worker that manages container lifecycle, a Dockerfile for the sandbox image, and a Vite front end.

This cluster is different from the others in the estate: it is **not a multi-app monorepo**. Five repositories carried a byte-identical README, the same `wrangler.jsonc` + `Dockerfile` + `vite.config.ts` layout, and sizes within 10 KB of each other. The only real variation was per-deployment identity — and identity is configuration, not code. So this repository is one codebase plus one `envs/` folder per deployment.

> **Status: scaffold — code import pending.** This repository currently contains this README, a `.gitignore` and empty target directories. Populate the tree with `tools/import_monorepos.sh --cluster openclaw-sandbox`, then correct any command against the project's own files.

## Alternative implementations compared

They are not alternatives — they are copies:

| Repository | What it actually is | Size | Disposition |
|---|---|---|---|
| **`openclawbot-sandbox`** | The codebase. Canonical by naming and by being the one referenced elsewhere | 613 KB | **canonical** — seeds the repo root |
| `moltbot-sandbox` | Same code, older bot name | 607 KB | deleted — `envs/moltbot/wrangler.jsonc` extracted first |
| `moltbot-sandbox1` | Same code, second deployment | 613 KB | deleted — `envs/moltbot-1/wrangler.jsonc` |
| `jude-claw` | Same code, named for a person | 613 KB | deleted — `envs/jude/wrangler.jsonc` |
| `irishstu` | Same code, named for a person | 614 KB | deleted — `envs/irishstu/wrangler.jsonc` |

## Which one is primary

**`openclawbot-sandbox` is the primary implementation** — trivially, since all five are the same repository. Keep exactly one copy of the code and express the four retired deployments as configuration under `envs/`.

**The only thing worth extracting before deletion** is each copy's `wrangler.jsonc` (and any committed `wrangler.toml` secrets): those are the per-deployment identities — Worker name, KV/R2/D1 bindings, container settings. Everything else in the four copies is identical bytes.

This is the highest-value, lowest-risk action in the whole consolidation: five repos to one, with no code to reconcile.

## Layout

```
openclaw-sandbox/
├── src/                       # Worker: container lifecycle
├── Dockerfile                 # sandbox image
├── wrangler.jsonc             # base config
├── vite.config.ts             # front-end build
└── envs/
    ├── moltbot/wrangler.jsonc      # retired deployment configs
    ├── moltbot-1/wrangler.jsonc
    ├── jude/wrangler.jsonc
    └── irishstu/wrangler.jsonc
```

## Setup

From the repository root:

1. `npm install`
2. `npx wrangler dev` — the Worker and sandbox container locally. Sandbox features need Docker running on the host.
3. `npx wrangler deploy` to publish; container images are pushed by Wrangler from the `Dockerfile`.
4. `npm run build` / `vite` for the front end alone.

Per-deployment setup is picking an `envs/<name>/wrangler.jsonc` — copy it over the base config (or point `--config` at it) before `wrangler deploy`. **Secrets** (bot tokens, API keys) are never committed: `npx wrangler secret put <NAME>`. If you restore a retired deployment, rotate whatever its old config referenced, because those values were committed in four histories.

## Import

```bash
tools/import_monorepos.sh --cluster openclaw-sandbox
```

Before deleting the four copies, mine their `wrangler.jsonc` files into `envs/` — that is the acceptance test for this cluster.
