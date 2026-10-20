<h1 align="center">T-CRYPT <sub><sup>// SYSTEMS LAB</sup></sub></h1>

<p align="center">
  <b>Senior Systems Engineer</b> · MSP delivery · 10 years in IT (USAF) · Colorado
  <br>
  <sub>Linux desktop, infrastructure visibility, human-gated AI development, local agent compute</sub>
</p>

<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&pause=1200&color=29E8F7&center=true&vCenter=true&width=620&height=30&lines=build+%E2%86%92+operate+%E2%86%92+compute;the+human+keeps+the+approval+gate;inference+runs+on+my+own+hardware" alt="build, operate, compute" />
</p>

I design and deliver infrastructure for more than 80 client environments by day. Off the clock I build the systems I run on: a desktop that understands the machine under it, a control plane that keeps AI agents on a leash, a live view of my lab, and an agent stack that runs on my own GPU. These are not four side projects. They are four layers of one environment.

## Layers

| Layer | System | Status |
| :-- | :-- | :-- |
| BUILD | [Aphotic-Hypr](https://github.com/T-Crypt/aphotic-hypr) | public, beta |
| BUILD | [GATE](https://github.com/T-Crypt/GATE) | public, early |
| OPERATE | AETHER | private, live lab |
| COMPUTE | Claude-Local | local architecture |

## Featured

The four projects people actually use. Star counts update on their own.

| | Project | What it is |
| :-- | :-- | :-- |
| 1 | [berth](https://github.com/T-Crypt/berth) · ![berth stars](https://img.shields.io/github/stars/T-Crypt/berth?style=flat-square&label=stars) | Decides who gets your GPU. Chat, image, and voice engines lease the card in turn instead of grabbing it, so two of them never load at once and crash each other. One Go binary. |
| 2 | [aphotic-hypr](https://github.com/T-Crypt/aphotic-hypr) · ![aphotic-hypr stars](https://img.shields.io/github/stars/T-Crypt/aphotic-hypr?style=flat-square&label=stars) | Four identities, one desktop. Modular plugins, live hot-swapping, resource-aware. |
| 3 | [oh-my-pocket](https://github.com/T-Crypt/oh-my-pocket) · ![oh-my-pocket stars](https://img.shields.io/github/stars/T-Crypt/oh-my-pocket?style=flat-square&label=stars) | Pi Pocket tuned for local models, running on your machine or on your phone. `fork` |
| 4 | [oh-my-epiq](https://github.com/T-Crypt/oh-my-epiq) · ![oh-my-epiq stars](https://img.shields.io/github/stars/T-Crypt/oh-my-epiq?style=flat-square&label=stars) | Local-first, git-backed issue tracker with a terminal, a browser board, and an agent MCP. |

## Active projects

Everything else pushed in the last 60 days, grouped by what it does. These are the fun experiments I am building now. 🔒 marks a private repo, which has no public link.

### Aphotic ecosystem

- [aphotic-plugins](https://github.com/T-Crypt/aphotic-plugins) ships the official plugins for Aphotic-Hypr. `public`
- [aphotic-themes](https://github.com/T-Crypt/aphotic-themes) ships the official theme pack for Aphotic-Hypr. `public`
- [aphotic-pets](https://github.com/T-Crypt/aphotic-pets) lists themable pets for Aphotic-Hypr. `public`

### Agent and dev tooling

- [GATE](https://github.com/T-Crypt/GATE) is a localhost control plane where a human decides every gate. `public, early`
- 🔒 aether-harness is a local-first agent harness that keeps its log through a crash or a dropped terminal. `private, early`
- [ModelHelm](https://github.com/T-Crypt/ModelHelm) delegates work across local and cloud models over MCP. `public`
- [llm-tune](https://github.com/T-Crypt/llm-tune) is an agent skill that tunes local LLM settings against measured evidence. `public`

### Lab and infrastructure

- 🔒 homelab keeps the inventory, runbooks, and decisions that operate the lab, with a public journey log in `public/`. `private`
- 🔒 aether-grid is a live dashboard for local inference, fed by Speculum. `private`
- 🔒 ai-blog is the AETHER public site, private until launch. `private`
- 🔒 BlastRadius maps which services and devices a proposed infrastructure change affects. `private, concept`
- 🔒 InfraSuite is a design for AI-assisted infrastructure operations where an enforcement layer, not the model, decides what may run. `private, concept`
- 🔒 M365-Tools collects PowerShell tools for Microsoft 365 administration, migration, and security hygiene. `private`

### GPU, inference, and models

- 🔒 berth-private drains the card on handoff and reports VRAM squatters. `private`
- [speculum](https://github.com/T-Crypt/speculum) watches a local LLM runtime and draws it on one page. `public`
- 🔒 spectra explains why a GPU workload performs the way it does, with the evidence attached. `private`
- 🔒 AdaScope ranks the likely bottlenecks in GPU inference and proposes the smallest experiment to confirm them. `private`
- 🔒 SMTrace fingerprints CUDA workloads and proposes optimization experiments. `private`
- [ninfer-4090](https://github.com/T-Crypt/ninfer-4090) runs Qwen3.8-27B on one RTX 4090 with a full 262K context. `public, fork`
- [strata](https://github.com/T-Crypt/strata) installs Qwen3.8-Flash-Next on consumer hardware in one step. `public, fork`
- [llama-swap](https://github.com/T-Crypt/llama-swap) swaps models behind any local OpenAI or Anthropic compatible server. `public, fork`
- [local-ai-registry](https://github.com/T-Crypt/local-ai-registry) stores one validated recipe per machine, with the evidence attached. `public, fork`
- [llama.cpp](https://github.com/T-Crypt/llama.cpp) is the GGUF inference runtime behind the rest. `public, fork`
- [ComfyUI-QwenVL-Mod](https://github.com/T-Crypt/ComfyUI-QwenVL-Mod) adds Qwen-VL with GGUF support to ComfyUI. `public, fork`

### Security

- [Strix-Recon](https://github.com/T-Crypt/Strix-Recon) automates reconnaissance for penetration tests with an LLM. `public`

<details>
<summary><b>Archive, older and inactive projects</b></summary>

- [worldsimulator](https://github.com/T-Crypt/worldsimulator) renders a browser geospatial intelligence prototype. `public, 2026-02`
- [blackarch](https://github.com/T-Crypt/blackarch) tracks the Arch-based distribution for penetration testers. `public, fork`
- [Meraki-EOL-Manager](https://github.com/T-Crypt/Meraki-EOL-Manager) manages Meraki hardware lifecycles. `public`
- [hyprdots](https://github.com/T-Crypt/hyprdots) collects dotfiles for Arch and Hyprland. `public, fork`
- [secrets](https://github.com/T-Crypt/secrets) collects ffuf and fuzzing notes. `public`
- [hyprland-layoutswitch](https://github.com/T-Crypt/hyprland-layoutswitch) switches keyboard layouts under Hyprland. `public`
- [Hacktoberfest2023](https://github.com/T-Crypt/Hacktoberfest2023) holds a Hacktoberfest pull request. `public, fork`
- [github-readme-stats](https://github.com/T-Crypt/github-readme-stats) generates README stats. `public, fork`

</details>

<details>
<summary><b>Inference stack, workstation, and tooling</b></summary>

| Layer | Tool | Role |
| :-- | :-- | :-- |
| Orchestration | Claude Code | agent harness, subagents, tool execution |
| Local and cloud split | claude-router | local roles to llama-swap, cloud traffic passed through |
| Routing | llama-swap | role aliases, swap groups, load and unload lifecycle |
| Runtime | llama.cpp and llama-server | GGUF inference, pinned builds, slot, context, and reasoning-budget config |
| Serving (alternative) | Ollama, LM Studio | quick model management, desktop serving |
| Training | Unsloth, PyTorch | fine-tuning experiments |
| Compute | RTX 4090, CUDA | 24GB VRAM |

**Working knowledge.** GGUF and quantized inference (IQ and Q4 quants, MoE versus dense), local model serving, GPU inference and VRAM budgeting, model routing, runtime configuration for slots and context, context management, agent orchestration, multi-agent workflows, local coding agents, local and cloud interoperability over OpenAI-compatible APIs, and task-level evals that pick which model earns which role.

**Workstation.** An Intel Core i9-14900K, an RTX 4090 with 24GB VRAM, 64GB RAM, and a custom water loop. It boots Arch Linux for daily use, Aphotic development, and the Claude-Local host, plus Windows 11 when the job needs it. Both boots share the same llama-swap stack.

</details>

<details>
<summary><b>Infrastructure and enterprise stack</b></summary>

<p>
  <img alt="Microsoft 365" src="https://img.shields.io/badge/Microsoft_365-D83B01?style=flat-square&logo=microsoft365&logoColor=white" />
  <img alt="Entra ID" src="https://img.shields.io/badge/Entra_ID-0078D4?style=flat-square&logo=microsoftazure&logoColor=white" />
  <img alt="JumpCloud" src="https://img.shields.io/badge/JumpCloud-F74F3F?style=flat-square&logo=jumpcloud&logoColor=white" />
  <img alt="Cisco Meraki" src="https://img.shields.io/badge/Cisco_Meraki-67B346?style=flat-square&logo=ciscomeraki&logoColor=white" />
  <img alt="Palo Alto Networks" src="https://img.shields.io/badge/Palo_Alto_Networks-FA582D?style=flat-square&logo=paloaltosoftware&logoColor=white" />
  <img alt="Datto RMM" src="https://img.shields.io/badge/Datto_RMM-01A6E4?style=flat-square" />
  <img alt="LogicMonitor" src="https://img.shields.io/badge/LogicMonitor-1F1F1F?style=flat-square" />
  <img alt="IT Glue" src="https://img.shields.io/badge/IT_Glue-0072CE?style=flat-square" />
  <img alt="Proxmox" src="https://img.shields.io/badge/Proxmox-E57000?style=flat-square&logo=proxmox&logoColor=white" />
  <img alt="Docker" src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white" />
  <img alt="TrueNAS" src="https://img.shields.io/badge/TrueNAS-0095D5?style=flat-square&logo=truenas&logoColor=white" />
  <img alt="Tailscale" src="https://img.shields.io/badge/Tailscale-000000?style=flat-square&logo=tailscale&logoColor=white" />
  <img alt="Grafana" src="https://img.shields.io/badge/Grafana-F46800?style=flat-square&logo=grafana&logoColor=white" />
  <img alt="Prometheus" src="https://img.shields.io/badge/Prometheus-E6522C?style=flat-square&logo=prometheus&logoColor=white" />
  <img alt="Arch Linux" src="https://img.shields.io/badge/Arch_Linux-1793D1?style=flat-square&logo=arch-linux&logoColor=white" />
  <img alt="Hyprland" src="https://img.shields.io/badge/Hyprland-58E1FF?style=flat-square&logo=wayland&logoColor=black" />
  <img alt="Quickshell" src="https://img.shields.io/badge/Quickshell-41CD52?style=flat-square&logo=qt&logoColor=white" />
  <img alt="Python" src="https://img.shields.io/badge/Python-14354C?style=flat-square&logo=python&logoColor=white" />
  <img alt="PowerShell" src="https://img.shields.io/badge/PowerShell-5391FE?style=flat-square&logo=powershell&logoColor=white" />
  <img alt="Bash" src="https://img.shields.io/badge/Bash-121011?style=flat-square&logo=gnu-bash&logoColor=white" />
  <img alt="JavaScript" src="https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black" />
  <img alt="QML" src="https://img.shields.io/badge/QML-41CD52?style=flat-square&logo=qt&logoColor=white" />
  <img alt="Go" src="https://img.shields.io/badge/Go-00ADD8?style=flat-square&logo=go&logoColor=white" />
  <img alt="SQLite" src="https://img.shields.io/badge/SQLite-07405E?style=flat-square&logo=sqlite&logoColor=white" />
</p>

</details>

<details>
<summary><b>Security, Hack The Box</b></summary>

<p align="center">
  <a href="https://app.hackthebox.com/profile/204903"><img src="https://www.hackthebox.com/badge/image/204903" alt="Hack The Box" /></a>
</p>

Hack The Box as `5H3LLKiller`. Kali and BlackArch tooling, plus the Aphotic Security profile for offensive-research sublayers. **Hack → Scream → Repeat.**

</details>

<div align="center">
  <a href="https://linkedin.com/in/trevin-tindall-483883213">LinkedIn</a> ·
  <a href="https://app.hackthebox.com/profile/204903">Hack The Box</a> ·
  <a href="https://www.hackerrank.com/trevintindall">HackerRank</a>
  <br>
  <sub>Ask me about Linux, Proxmox, MSP infrastructure, Hack The Box, or local inference. Help on Aphotic-Hypr is always welcome.</sub>
  <br>
  <img src="https://komarev.com/ghpvc/?username=t-crypt&label=profile%20views&color=0e75b6&style=flat" alt="profile views" />
</div>
