# plan-mode-research

Enhances Claude Code's plan mode with automated research capabilities. When Claude asks clarifying questions during planning, this skill adds a **"Research this further"** option that spawns a focused subagent to investigate the question and surface concrete recommendations.

## What it does

During plan mode, every `AskUserQuestion` call gets a "Research this further" option with a description tailored to the specific question being asked. When selected, a lightweight Haiku subagent runs targeted searches (using WebSearch and/or context7 docs) and returns a structured finding with a recommendation, rationale, and any remaining uncertainty.

Claude then acts on the result:
- **Clear winner found** → proceeds with the recommended approach
- **Multiple valid options** → re-poses the question with concrete choices surfaced by research
- **Contradictory findings** → flags the conflict and lets the user decide
- **New questions raised** → surfaces them with their own research options

## Example

When planning how to add authentication to a React + Express app, Claude might ask:

> *Which authentication approach would you like to use?*
> - JWT (stateless tokens)
> - Session-based (server-side)
> - OAuth / third-party provider
> - **Research this further** — *Research common authentication patterns (JWT vs session-based vs OAuth), check relevant framework docs, and look for community best practices.*

Selecting "Research this further" spawns a subagent that investigates the options and either picks a clear winner or returns the question with concrete, evidence-backed choices.

## Installation

Clone or copy this skill into `~/.claude/skills/` or `~/.agents/skills/`.

## Usage

This skill activates automatically in plan mode. No manual invocation needed — it's always included when Claude enters plan mode.

## Evals

The `evals/evals.json` file contains 6 test cases covering:
- Real-time features (WebSockets/SSE)
- Authentication strategies
- REST to GraphQL migration
- Scientific computing (Monte Carlo simulations)
- Large-scale data pipelines (HDF5)
- SwiftUI App Groups / WidgetKit data sharing
