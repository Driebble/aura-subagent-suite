# Aura Subagent Suite

Or ASS for short... A multi-agent architecture for [OpenCode](https://opencode.ai) — a coordinated system of specialized AI agents that work together through a structured pipeline to build, review, and ship features.

**Author:** [Drie](https://github.com/driebble) · **License:** MIT · **Year:** 2026

---

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Agent Roster](#agent-roster)
- [Mode Selection](#mode-selection)
  - [Lightweight Mode](#lightweight-mode-plan--delegate--synthesize)
  - [Full Mode (5-Gate Pipeline)](#full-mode-5-gate-pipeline)
- [Why Aura?](#why-aura)
- [Philosophy Skills](#philosophy-skills)
- [Councilor Feature](#councilor-feature)
- [Artifact Contracts](#artifact-contracts)
- [Getting Started](#getting-started)
- [Uninstalling](#uninstalling)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Acknowledgments](#acknowledgments)
- [License](#license)

---

## Overview

The Aura Subagent Suite turns OpenCode from a single-agent assistant into a **coordinated engineering team**. A primary orchestrator agent (`aura`) decomposes user goals, delegates work to specialist subagents, and tracks progress across a configurable pipeline. Each subagent has strict boundaries — it can only do its specific job — which prevents scope creep and produces higher-quality results.

The system supports two modes of operation. For simple, well-scoped tasks, **Lightweight Mode** sends work directly to the right specialist. For complex features — anything requiring research, design, implementation, and verification — **Full Mode** runs a 5-gate pipeline that mirrors a real software development lifecycle.

Five philosophy skills define shared quality standards that implementing and reviewing agents must load before working, ensuring consistency across code, design, and content.

---

## Architecture

```
┌──────────────────────────────────────────────────────┐
│                    User Request                      │
└──────────────────────┬───────────────────────────────┘
                       │
               ┌───────▼──────────┐
               │       aura       │  Orchestrator — decomposes goal,
               │  (orchestrator)  │  selects mode, delegates work,
               └───────┬──────────┘  tracks progress, synthesizes results
                       │
          ┌────────────┼─────────────────┐
          │            │                 │
   ┌──────▼───────┐  ┌─▼───────────┐  ┌──▼───────┐
   │ Lightweight  │  │ Full Mode   │  │ Custom   │
   │ (single-hop) │  │ (5 gates)   │  │ Command  │
   └──────────────┘  └─────────────┘  └──────────┘
                           │
     ┌─────────────────────┼──────────────────────┐
     │                     │                      │
     ▼                     ▼                      ▼
  Explore (x2)         Design (x2)           Create (x3)
  +───────────+      +─────────────+      +──────────────+
  │ explore   │      │ architect   │      │ implementer  │
  │ researcher│      │ designer    │      │ writer       │
  +───────────+      +─────────────+      │ code-qa      │
                                          +──────────────+
          ▼
        Verify (x3)                     Synthesize
        +────────────────────────+       +───────────+
        │ code-qa (test results) │       │   aura    │
        │ content-qa             │       │ (results) │
        │ reviewer               │       +───────────+
        +────────────────────────+
```

The orchestrator (`aura`) never implements anything directly. It selects the appropriate mode, fans out work to subagents via the Task tool, and compresses context between pipeline gates to manage token usage. Each subagent produces a standardized artifact file, creating a clear audit trail from start to finish.

---

## Agent Roster

| Agent | Mode | Responsibility |
|---|---|---|
| `aura` | **primary** | Orchestrator — decomposes goals, selects mode, delegates, tracks gates, synthesizes results |
| `aura-explore` | subagent | Fast codebase exploration — reads project structure, finds patterns, maps terrain |
| `aura-researcher` | subagent | External research — web searches, API docs, design references, package documentation |
| `aura-architect` | subagent | High-level system design — architecture plans, schemas, tech stack decisions |
| `aura-designer` | subagent | Product design — UI/UX flows, wireframes, design tokens, component specs |
| `aura-implementer` | subagent | Code generation — writes source files, refactors, patches existing code |
| `aura-writer` | subagent | Content generation — documentation, marketing copy, changelogs, in-app text |
| `aura-council` | subagent | Multi-perspective advisory. Orchestrates 16 personality-based council members in parallel to produce a compiled markdown report with synthesized perspectives across 4 cognitive groups. |
| `aura-code-qa` | subagent | Test generation — unit/integration tests, edge-case analysis, test execution |
| `aura-content-qa` | subagent | Content quality — grammar, spelling, tone consistency, SEO, factual accuracy |
| `aura-reviewer` | subagent | Full-spectrum audit — security, correctness, conventions, design coherence, final sign-off |

A **custom slash command** is also available:

- `aura-review` — Runs code review via the reviewer agent. Supports reviewing specific files, staged changes (`git diff --cached`), or recent changes (`git diff HEAD~1`).

---

## Mode Selection

The orchestrator assesses each task before choosing a workflow. If the user doesn't specify, it asks.

### Lightweight Mode: Plan → Delegate → Synthesize

Best for **simple, well-scoped tasks** where a full pipeline is overkill — a single function change, a small bug fix, a quick content update.

1. **Plan** — The orchestrator optionally loads the `aura-plan-protocol` skill for multi-step tracking.
2. **Delegate** — Work is sent directly to the right specialist (e.g., `aura-implementer` for a code change, `aura-writer` for copy).
3. **Synthesize** — Results are collected and presented to the user.

Common lightweight task-to-agent delegations:

- Codebase analysis → `aura-explore`
- External research → `aura-researcher`
- System design → `aura-architect`
- UI/UX design → `aura-designer`
- Code implementation → `aura-implementer`
- Content/writing → `aura-writer`
- Tests → `aura-code-qa`
- Content quality review → `aura-content-qa`
- Final audit → `aura-reviewer`
- Multi-perspective analysis → `aura-council`

### Full Mode: 5-Gate Pipeline

Best for **complex features** requiring research, design, implementation, and verification — a new API endpoint, a landing page, a full feature module.

| Gate | Phase | Agents | Artifacts Produced |
|---|---|---|---|
| 0 | Setup | `aura-implementer` | Project directory created |
| 1 | **Explore** | `aura-explore` + `aura-researcher` (parallel) | `aura-RESEARCH-CODE.md`, `aura-RESEARCH-WEB.md` |
| 2 | **Design** | `aura-architect` + `aura-designer` (parallel) | `aura-ARCH.md`, `aura-SPEC.md` |
| 3 | **Create** | `aura-implementer` → `aura-writer` → `aura-code-qa` (sequenced) | Source files, content, tests, `aura-CHANGES.md`, `aura-TEST.md` |
| 4 | **Verify** | `aura-code-qa` → `aura-content-qa` → `aura-reviewer` (sequenced) | Test results, `aura-QA.md`, `aura-REVIEW.md` |
| — | **Synthesize** | `aura` | Final summary to user |

Each gate waits for all its subagents to complete before proceeding. Context is compressed between gates to stay within token limits. If a subagent fails, the orchestrator retries once, then falls back to the built-in `general` subagent, then reports to the user.

---

## Why Aura?

The Aura Subagent Suite was inspired by [**Kdco's OCX Workspace**](https://github.com/kdcokenny/opencode-workspace) — a feature-rich multi-agent harness that pioneered structured agent pipelines and philosophy-driven quality standards for OpenCode. OCX is powerful, but it comes with complexity: a separate CLI (`ocx`), package registries, profile management, model assignments, and a `opencode.jsonc` configuration.

In my own experience, already working within a customized OpenCode environment, I realized the real value lies in an orchestrator and subagent layer that functions natively out-of-the-box. That's why I made Aura Subagent Suite and designed it to do exactly what I was looking for.

**Aura takes a different approach:**

- **Plug-n-play native** — Clone the repo, drop the folders into `~/.config/opencode/`, restart OpenCode. That's it. No `ocx` to install, no registries, no profile setup, no config editing.
- **Zero configuration** — Aura integrates directly into your existing OpenCode setup without changing anything you've already configured. Your existing agents, skills, and commands stay exactly as they are — Aura's files just sit alongside them.
- **Lightweight and focused** — Aura is pure markdown. No plugins, no MCP servers, no npm dependencies. Just agent definitions, skills, and a command.
- **Highly customizable** — Since Aura is basically a collection of markdown files, every agent and skill is easy to tweak or extend. Everything follows the standard [OpenCode configuration](https://opencode.ai/docs) specifications.
- **No lock-in** — Removing or modifying Aura is as simple as deleting files from your config directory. It stays out of your way and respects your existing setup.

If OCX is a full operating system for your OpenCode environment, Aura is a well-made tool — it does what it needs to and stays out of your way.

---

## Philosophy Skills

Philosophy skills define shared quality standards that subagents load at runtime via the `skill` tool. They act as a **constitution** — agents must reference them before writing or reviewing specific categories of work.

| Skill | Applies To | Scope | Core Philosophy |
|---|---|---|---|
| `aura-code-philosophy` | `aura-implementer`, `aura-reviewer` | Backend/logic code, data flow, state management | **5 Laws of Elegant Defense** — guard clauses, parse don't validate, atomic predictability, fail fast/loud, intentional naming |
| `aura-frontend-philosophy` | `aura-implementer`, `aura-reviewer`, `aura-designer` | Frontend code, UI components, styling, design tokens | **5 Pillars of Intentional UI** — consistency, feedback, clarity, efficiency, delight |
| `aura-code-review` | `aura-reviewer` | All code audits | **4-Layer Review** — Correctness → Security → Performance → Style/Maintainability, with severity classification and ≥80% confidence threshold |
| `aura-plan-protocol` | `aura` (orchestrator) | Multi-step plans in lightweight mode | **Structured Plan Format** — goal, context/decisions table, phased tasks with status markers, citations, single `<- CURRENT` task |
| `aura-plan-review` | `aura-reviewer` | Auditing implementation plans | **Plan Quality Criteria** — citation completeness, actionability, goal clarity, status tracking correctness |

---

## Councilor Feature

The **Councilor** is a multi-perspective analysis engine that convenes a panel of 16 distinct personality types across 4 cognitive groups to examine any question, decision, or problem — delivering synthesized insights directly in the conversation.

### How It Works

1. **Invoke Councilor** — either directly (`@aura-council`) or by asking Aura to convene it.
2. **Councilor analyzes** your prompt and optionally asks clarifying questions.
3. **All 16 personality types are consulted in parallel** — each evaluating through their unique cognitive lens.
4. **Councilor compiles a structured report** with an executive summary, all 16 perspectives organized by group, key tensions, cross-group themes, and open questions — delivered directly in the conversation.

### The 16 Council Members

| Group | Types |
|---|---|
| **🟣 Analysts — Rational & Strategic** | The Architect (INTJ), The Logician (INTP), The Commander (ENTJ), The Debater (ENTP) |
| **🟢 Diplomats — Empathetic & People-Focused** | The Advocate (INFJ), The Mediator (INFP), The Protagonist (ENFJ), The Campaigner (ENFP) |
| **🔵 Sentinels — Practical & Reliable** | The Logistician (ISTJ), The Defender (ISFJ), The Executive (ESTJ), The Consul (ESFJ) |
| **🟠 Explorers — Spontaneous & Action-Oriented** | The Virtuoso (ISTP), The Adventurer (ISFP), The Entrepreneur (ESTP), The Entertainer (ESFP) |

### Invocation Examples

```
@aura-council Should we adopt microservices or stay monolithic?
@aura-council (subset: architect, debater) Analyze our deployment pipeline
@aura-council (subset: analysts) Strategy review for Q3 planning
@aura-council (save: ./council-report.md) Strategy review for Q3
```

### When to Use Councilor

- **Strategic decisions** — get rational analysis from Analysts and practical grounding from Sentinels
- **Team or people decisions** — get empathetic insight from Diplomats
- **Innovation or change** — get creative provocation from Explorers
- **Any ambiguous problem** — see it through 16 different cognitive lenses

---

## Artifact Contracts

Every pipeline gate produces a standardized artifact file. All artifacts live under `PROJ_DIR/.aura/`, creating a complete audit trail from research through review.

| Artifact | Producer | Contents |
|---|---|---|
| `aura-RESEARCH-CODE.md` | `aura-explore` | Codebase analysis — structure, patterns, relevant file paths |
| `aura-RESEARCH-WEB.md` | `aura-researcher` | External research — web findings, API docs, references |
| `aura-ARCH.md` | `aura-architect` | Architecture plan — system design, schemas, tech stack |
| `aura-SPEC.md` | `aura-designer` | Design specification — flows, wireframes, design tokens |
| `aura-CHANGES.md` | `aura-implementer` + `aura-writer` | Consolidated changes — `## Code Changes` + `## Content Changes` |
| `aura-COUNCIL.md` | `aura-council` | Councilor report (optional) — saved only when requested via `(save:)` syntax; otherwise delivered in conversation |
| `aura-TEST.md` | `aura-code-qa` | Test scaffold and execution results |
| `aura-QA.md` | `aura-content-qa` | Content quality report — grammar, tone, facts, SEO |
| `aura-REVIEW.md` | `aura-reviewer` | Final audit report — security, correctness, conventions, verdict |

---

## Getting Started

### Prerequisites

- [OpenCode](https://opencode.ai) — Aura runs as agent definitions within OpenCode's config directory.

### Installation

1. **Clone the repository:**

   ```bash
   git clone https://github.com/drie/aura-subagent-suite.git
   ```

2. **Copy the folders** into your OpenCode global config:

   ```bash
   cp -r aura-subagent-suite/agents ~/.config/opencode/
   cp -r aura-subagent-suite/skills ~/.config/opencode/
   cp -r aura-subagent-suite/commands ~/.config/opencode/
   ```

   On Windows (PowerShell):

   ```powershell
   Copy-Item -Recurse aura-subagent-suite/agents "$env:USERPROFILE\.config\opencode\"
   Copy-Item -Recurse aura-subagent-suite/skills "$env:USERPROFILE\.config\opencode\"
   Copy-Item -Recurse aura-subagent-suite/commands "$env:USERPROFILE\.config\opencode\"
   ```

3. **Restart OpenCode.** Aura and all its subagents are now available.

### Verifying the Setup

Press **Tab** in OpenCode to see the agent list — `aura` should appear as one of the available agents. Ask a simple question; the orchestrator will select the appropriate mode and delegate to the right specialist.

---

## Uninstalling

Since Aura is entirely markdown-based, uninstalling is as simple as removing the files:

```bash
rm -rf ~/.config/opencode/agents/aura*
rm -rf ~/.config/opencode/skills/aura-*
rm -rf ~/.config/opencode/commands/aura-*
```

On Windows (PowerShell):

```powershell
Remove-Item "$env:USERPROFILE\.config\opencode\agents\aura*" -Recurse -Force
Remove-Item "$env:USERPROFILE\.config\opencode\skills\aura-*" -Recurse -Force
Remove-Item "$env:USERPROFILE\.config\opencode\commands\aura-*" -Recurse -Force
```

This removes every file and folder that begins with `aura` from the `agents`, `skills`, and `commands` directories — nothing else is touched. Restart OpenCode and Aura is gone.

---

## Usage

### Basic Workflow

1. **Describe what you want built** — a feature, a fix, documentation, a design spec.
2. **The orchestrator assesses the task** and chooses Lightweight Mode or Full Mode.
3. **For simple tasks:** the orchestrator delegates directly to the appropriate subagent.
4. **For complex tasks:** the 5-gate pipeline runs automatically — explore, design, create, verify, synthesize.
5. **Review the results.** The `aura-reviewer` provides a structured audit report with severity-classified findings.

### Examples

| Task | Likely Mode | Key Agents |
|---|---|---|
| "Fix the login button color" | Lightweight | `aura-implementer` |
| "Add JWT authentication to the API" | Full (5-gate) | researcher → architect → implementer → code-qa → reviewer |
| "Write a changelog for v2.1.0" | Lightweight | `aura-writer` |
| "Build a new landing page with hero, features, and pricing sections" | Full (5-gate) | explore → designer → implementer → writer → reviewer |
| "Find all places we handle errors inconsistently" | Lightweight | `aura-explore` |
| "Get multi-perspective feedback through 16 cognitive lenses" | Lightweight (Council) | `aura-council` |

### Custom Commands

The `aura-review` slash command lets you run code reviews on demand:

- `/aura-review` — Reviews staged changes (`git diff --cached`)
- `/aura-review recent` — Reviews changes since the last commit
- `/aura-review src/auth.js` — Reviews a specific file or directory

---

## Project Structure

```
├── agents/                         # Agent definitions loaded by OpenCode
│   ├── aura.md                     # Primary orchestrator — manages pipeline
│   ├── aura-architect.md           # System architecture & design
│   ├── aura-code-qa.md             # Test generation & execution
│   ├── aura-content-qa.md          # Content quality auditing
│   ├── aura-designer.md            # UI/UX design specifications
│   ├── aura-explore.md             # Codebase exploration & pattern finding
│   ├── aura-implementer.md         # Code implementation & refactoring
│   ├── aura-researcher.md          # External research & documentation
│   ├── aura-reviewer.md            # Full-spectrum audit & sign-off
│   ├── aura-writer.md              # Content & documentation writing
│   ├── aura-council.md              # Councilor orchestrator (visible in @ menu)
│   ├── aura-council-intj.md         # The Architect (INTJ)
│   ├── aura-council-intp.md         # The Logician (INTP)
│   ├── aura-council-entj.md         # The Commander (ENTJ)
│   ├── aura-council-entp.md         # The Debater (ENTP)
│   ├── aura-council-infj.md         # The Advocate (INFJ)
│   ├── aura-council-infp.md         # The Mediator (INFP)
│   ├── aura-council-enfj.md         # The Protagonist (ENFJ)
│   ├── aura-council-enfp.md         # The Campaigner (ENFP)
│   ├── aura-council-istj.md         # The Logistician (ISTJ)
│   ├── aura-council-isfj.md         # The Defender (ISFJ)
│   ├── aura-council-estj.md         # The Executive (ESTJ)
│   ├── aura-council-esfj.md         # The Consul (ESFJ)
│   ├── aura-council-istp.md         # The Virtuoso (ISTP)
│   ├── aura-council-isfp.md         # The Adventurer (ISFP)
│   ├── aura-council-estp.md         # The Entrepreneur (ESTP)
│   └── aura-council-esfp.md         # The Entertainer (ESFP)
│
├── commands/                       # Custom slash commands
│   └── aura-review.md              # On-demand code review command
│
├── skills/                         # Philosophy & methodology skills
│   ├── aura-code-philosophy/       # 5 Laws of Elegant Defense
│   ├── aura-frontend-philosophy/   # 5 Pillars of Intentional UI
│   ├── aura-code-review/           # 4-layer code review methodology
│   ├── aura-plan-protocol/         # Structured plan format & tracking
│   └── aura-plan-review/           # Plan quality auditing criteria
│
├── LICENSE                         # MIT License
└── README.md                       # This file
```

---

## Acknowledgments

The philosophy skills in this repository — `aura-code-philosophy`, `aura-frontend-philosophy`, `aura-code-review`, `aura-plan-protocol`, and `aura-plan-review` — were heavily inspired by and forked from [**Kdco's OCX Workspace**](https://github.com/kdcokenny/opencode-workspace). Thank you to Kdco for establishing these quality standards and pioneering the multi-agent harness paradigm for OpenCode.

---

## License

MIT License · Copyright (c) 2026 Drie

See [LICENSE](./LICENSE) for the full text.

---

*Created by **Drie**. Built for OpenCode. Designed to ship better features, faster.*
