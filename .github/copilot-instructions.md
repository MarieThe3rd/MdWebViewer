# Squirrel — GitHub Copilot Workspace Instructions

## Learning Goal — IMPORTANT: Read This First

**The primary purpose of this project is to teach the user how to use GitHub Copilot for AI-assisted development.**

The Squirrel app is the learning vehicle — not the destination. The user is not learning C# or software engineering fundamentals. They already know those. What they are learning is:

- How GitHub Copilot customization files work and why they exist
- How to structure a project so AI agents can be effective teammates
- How to use agents, prompts, skills, instructions, and hooks in real workflows
- How to think about AI-assisted development as a practice, not just a tool

### How to behave in this workspace

- **When creating or modifying any Copilot customization file** (`.instructions.md`, `.prompt.md`, `.agent.md`, `SKILL.md`, `copilot-instructions.md`), explain:
  - What this file type does and how Copilot discovers and uses it
  - Why it belongs in this location
  - What would happen without it
  - How it fits into the overall AI-assisted dev workflow

- **When asked to do something**, name which primitive(s) you're using and why it's the right choice over alternatives

- **Do not over-explain C# syntax or app logic** — focus teaching on the Copilot customization layer

- **Connect every action to the learning** — if you create a file, explain the pattern; if you make a decision, explain the tradeoff

## Project Overview

Squirrel is a personal thought-capture CLI app built in C# (.NET). It lets the user quickly jot down fleeting thoughts, interruptions, and ideas ("squirrels") to think about later. It is also the learning vehicle for AI-assisted development using GitHub Copilot agents, skills, prompts, and instructions.

## Data Model

A **Thought** has these fields:

- `Id` — auto-generated unique identifier
- `CapturedAt` — datetime, auto-set on creation
- `Content` — the thought text
- `Source` — what triggered it (email, meeting, message, random, conversation)
- `People` — list of people involved
- `IsWork` — bool, work vs personal
- `IsSquirrel` — bool, an interrupting distraction to park and revisit
- `IsLearnAbout` — bool, something the user wants to explore or learn
- `Status` — enum: `New` | `Parked` | `Promoted` | `Dismissed`
- `Tags` — optional free-form list

Storage: local JSON file. No database.

## CLI Commands

- `squirrel add "content"` — quick capture with optional flags: `--work`, `--squirrel`, `--learn`, `--source <source>`, `--people <name,...>`, `--tags <tag,...>`
- `squirrel list` — list all thoughts
- `squirrel list --squirrels` / `--work` / `--learn` / `--status <status>` — filtered views
- `squirrel sort --by date|status|work`
- `squirrel promote <id>` — promote to task/project
- `squirrel dismiss <id>` — dismiss

## Tech Stack

- Language: C# (.NET 9)
- CLI framework: System.CommandLine
- Storage: JSON file via System.Text.Json
- Tests: xUnit

## Conventions

- Follow C# naming conventions: PascalCase for types and methods, camelCase for locals
- Keep commands small and focused — one responsibility per command handler
- Models go in `Models/`, commands in `Commands/`, storage in `Storage/`
- All user-facing strings are readable and friendly — this is a personal tool
- Tests cover happy path and edge cases for each command

## AI Dev Team Roles

This project uses specialized Copilot agents defined in `.github/agents/`:

- `@planner` — designs features and project structure. Never writes code.
- `@coder` — implements features. Follows conventions above. No planning commentary.
- `@reviewer` — reviews code for quality, security, and convention compliance.
- `@tester` — writes xUnit tests. Focuses on edge cases and coverage.

When implementing a new feature, follow this order: **plan → code → review → test**

## Learning Roadmap

Phase 1: Copilot customization setup (instructions, prompts, agents, skills)
Phase 2: Build Squirrel using the agents
Phase 3: Hooks, MCP, web UI, brainstorming tooling (deferred)

## References

- [VS Code Copilot Customization Docs](https://code.visualstudio.com/docs/copilot/copilot-customization)
- [Awesome Copilot](https://github.com/github/awesome-copilot)
- [Awesome Copilot Learning Hub](https://awesome-copilot.github.com/learning-hub)
