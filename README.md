# CLI Proxy API

A proxy server that provides OpenAI/Gemini/Claude/Codex compatible API interfaces for CLI.

It now also supports OpenAI Codex (GPT models) and Claude Code via OAuth.

So you can use local or multi-account CLI access with OpenAI(include Responses)/Gemini/Claude-compatible clients and SDKs.

## Overview

- OpenAI/Gemini/Claude compatible API endpoints for CLI models
- OpenAI Codex support (GPT models) via OAuth login
- Claude Code support via OAuth login
- Qwen Code support via OAuth login
- iFlow support via OAuth login
- Amp CLI and IDE extensions support with provider routing
- Streaming and non-streaming responses
- Function calling/tools support
- Multimodal input support (text and images)
- Multiple accounts with round-robin load balancing (Gemini, OpenAI, Claude, Qwen and iFlow)
- Simple CLI authentication flows (Gemini, OpenAI, Claude, Qwen and iFlow)
- Generative Language API Key support
- AI Studio Build multi-account load balancing
- Gemini CLI multi-account load balancing
- Claude Code multi-account load balancing
- Qwen Code multi-account load balancing
- iFlow multi-account load balancing
- OpenAI Codex multi-account load balancing
- OpenAI-compatible upstream providers via config (e.g., OpenRouter)
- Reusable Go SDK for embedding the proxy (see `docs/sdk-usage.md`)

## Getting Started

### Run locally
```bash
# build binary
GOOS=linux GOARCH=amd64 go build -o CLIProxyAPI ./cmd/server

# run with default config (config.yaml in repo root)
./CLIProxyAPI
```

### Run with Docker
```bash
./docker-build.sh
# start container
docker compose up -d
```

### Account logins (in running container)
```bash
docker exec -it cli-proxy-api /CLIProxyAPI/CLIProxyAPI --claude-login # Claude
docker exec -it cli-proxy-api /CLIProxyAPI/CLIProxyAPI --codex-login # Codex
docker exec -it cli-proxy-api /CLIProxyAPI/CLIProxyAPI --login   # Gemini
```

## Syncing Your Fork With Upstream

Goal: pull upstream changes without losing your edits.

Mental model:
- `origin` = your fork
- `upstream` = original repo
- `main` mirrors upstream; your work lives on `feature/*` branches

One-time setup (add upstream remote):
```bash
git remote add upstream https://github.com/router-for-me/CLIProxyAPI.git
git remote -v
```

Standard workflow (safe and repeatable):
1) Make sure your work is committed:
```bash
git status
git add .
git commit -m "My changes"
```

2) Update `main` to match upstream:
```bash
git checkout main
git fetch upstream
git merge upstream/main
git push origin main
```

3) Reapply your work on top:
```bash
git checkout feature/my-changes
git rebase main
```

If conflicts appear:
```bash
git status
# fix conflicts
git add .
git rebase --continue
```

Notes:
- Do not commit directly to `main`.
- Build Docker images from your feature branch when you want your edits included.

## Claude Code Integration

To use this proxy with `claude-code`, edit your settings file at `~/.claude/settings.json` and add the following environment variables:

```json
"env": {
    "ANTHROPIC_BASE_URL": "http://127.0.0.1:8317",
    "ANTHROPIC_AUTH_TOKEN": "proxy",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "gemini-2.5-pro",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "gpt-5.1-codex-max"
  },
```

This will configure `claude-code` to send requests to the proxy.

## SDK Docs
- Usage: [docs/sdk-usage.md](docs/sdk-usage.md)
- Advanced (executors & translators): [docs/sdk-advanced.md](docs/sdk-advanced.md)
- Access: [docs/sdk-access.md](docs/sdk-access.md)
- Watcher: [docs/sdk-watcher.md](docs/sdk-watcher.md)
- Custom Provider Example: `examples/custom-provider`
