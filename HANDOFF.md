# Handoff: Homelab + Agent Stack Setup

Context carried over from a claude.ai planning session (2026-10-22). Start here.

## Kickoff prompt (paste into Claude Code from the root of this repo)

> Read HANDOFF.md and CLAUDE.md. Ask me every open question in one batch, then propose the full
> repo layout and every file you will create or change before writing anything. After I approve,
> build the deliverables MBP-first. Generate the Mac mini pieces but do not run anything over SSH,
> load any launchd job, or use sudo until I say so.

## Goal

A steady setup that just works day to day. Time goes to real problems, not tinkering.
Snappy over clever. No new tool unless it replaces one already in SETUP.md.

## This repo today

`configs` is a copy-based dotfiles repo shared by work and personal machines: `.zshrc`, `starship.toml`,
`tmux.conf`, `config.ghostty`, a reference `claude-code.json`, README with `cp` install steps, and
`.zshrc.local` for per-machine overrides. Keep it working on the Akamai work Mac (see Hard rules).

## Machines

| Host | Role |
|---|---|
| MacBook Pro M1 Max 64GB (primary) | Where I work. Orca (ADE) with Claude Code / Codex / Pi inside it. Two llama.cpp servers. |
| Mac mini M4 16GB (`michaels-mac-mini-local`) | Always-on headless brain. Hermes Agent on DeepSeek V4.1 Flash. No local models (16GB is too tight). Already has `~/.hermes`, `~/.pi`, `~/.codex` — inspect before reinstalling anything. |
| 2011 Mac mini (Linux) | Boring utilities: DNS blocking, Uptime Kuma, restic backup target. |
| iPhone 14 | Remote control only: Hermex → Hermes, Orca mobile companion. |
| Akamai work MBP (M4 Pro 24GB) | OUT OF SCOPE for the homelab. Shares shell/editor configs only. |

All personal machines on one Tailscale tailnet.

## Locked decisions

- **ADE:** Orca (stablyai/orca) on the MBP, brew cask for now (`brew install --cask stablyai/orca/orca`).
  Building from source is optional later: Node 24 + Corepack (honors the pinned pnpm), `pnpm install`,
  `pnpm dev` / `pnpm build` / `pnpm run dist`. Disable the auto-updater in a self-built fork.
- **Orchestrator:** Hermes Agent on the Mac mini as a launchd daemon.
  - Default model: DeepSeek V4.1 Flash (`deepseek/deepseek-v4.1-flash` on OpenRouter, ~$0.15–0.30/M in,
    $0.60–1.20/M out, 1M context, tool calling). Pin a US provider (DeepInfra/Novita) or use DeepSeek direct — TBD.
  - Local providers: MBP llama.cpp at `http://<mbp-tailscale>:8080/v1` (fast) and `:8081/v1` (quality).
  - Fallback to DeepSeek when the MBP is asleep.
- **herdr: dropped.** Hermes daemon + Orca cover session persistence and phone steering. Revisit only if sessions get lost.
- **Local models (MBP, llama.cpp, not Ollama):**
  - `:8080` fast — Qwen3.6-35B-A3B abliterated, Q4_K_M (~20GB), e.g. `wangzhang/Qwen3.6-35B-A3B-abliterated-GGUF`. Default for Pi / routine work.
  - `:8081` quality — Qwen3.8-27B abliterated, Q4_K_M (dense, slower). Repo TBD.
  - Flags: `--jinja -ngl 99 -c 32768`, bind to the Tailscale IP, not `0.0.0.0`.
  - Metal cap at boot: `sysctl iogpu.wired_limit_mb=52000`. If memory pressure appears, make `:8081` on-demand.
- **Power:** both Macs `pmset -c sleep 0 disksleep 0 displaysleep 15 womp 1`; mini also `pmset autorestart 1`.
  MBP lid-closed needs an external display (clamshell) or it sleeps.
- **Mac mini lag:** diagnose before changing anything — `tailscale ping` (direct vs DERP), memory pressure, then shell latency.

## Hard rules

- Secrets never in this repo. Read from macOS Keychain (`security find-generic-password`) or `.zshrc.local`.
- Ask before any `sudo`, any `launchctl load/bootstrap`, or anything over SSH to the mini.
- Work Mac: no tailnet membership, no personal agent subscriptions, no DeepSeek, no Hermes, no Orca until
  Akamai InfoSec approves in writing. Anything homelab-specific must not install or run on the work Mac
  when it pulls this repo — gate by hostname or keep it under `homelab/` with its own bootstrap.

## Deliverables

1. Repo layout decision: keep `cp`-based + `homelab/` subdir, or migrate to chezmoi with hostname templates
   (mbp, mini, linux-mini, work). Recommend, then wait for my call.
2. Brewfile per host.
3. launchd plists: llama-server `:8080` and `:8081` (RunAtLoad, KeepAlive), sysctl boot job, Hermes daemon on mini.
4. Idempotent bootstrap script per host: pmset, Brewfile, plists.
5. Hermes provider config: DeepSeek default + two local endpoints + fallback.
6. `SETUP.md`: every tool — machine, purpose, how to restart, where config lives. The one page I read when I forget.
7. `healthcheck.sh`: both llama servers up, Hermes up, Tailscale direct (not DERP).
8. Update README to match whatever layout is chosen.

## Open questions (ask all at once)

- 2011 Mac mini: CPU, RAM, disk type, current distro?
- Acer Chromebook (CB315, Celeron): still in use or retired?
- DeepSeek via OpenRouter (which provider) or DeepSeek direct?
- Which Qwen3.8-27B abliterated GGUF repo?
- Layout: stay `cp`-based or move to chezmoi?

## About me

SRE (Python, Go, Bash, Linux, Kubernetes). Direct, no filler. Full plan upfront, not drip-fed.
