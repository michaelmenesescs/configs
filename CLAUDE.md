# configs

Dotfiles shared by Michael's work and personal machines. Homelab/agent-stack setup is in progress: read HANDOFF.md first.

- Secrets never get committed. `claude-code.json` is a reference only; real `~/.claude.json` stays local.
- This repo is also pulled on the Akamai work Mac. Nothing homelab-specific (Tailscale, Hermes, Orca, DeepSeek,
  llama.cpp, launchd jobs) may install or run there. Gate by hostname or keep it under `homelab/`.
- Ask before any `sudo`, `launchctl` load, or SSH to the Mac mini.
- No new tool unless it replaces one listed in SETUP.md.
- Direct, no filler. Propose the full plan before writing files.
