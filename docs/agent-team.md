# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the custom agent
team building Mona's Project Pulse dashboard.

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 5.5 | Coordinates the specialists, divides work into phases with clear file ownership, decides what can run in parallel, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 5.5 | Researches the repository, documentation, dependencies, and edge cases, then produces an ordered implementation plan with file assignments, dependencies, and validation expectations. | `.github/agents/planner.agent.md` |
| **Designer** | Gemini 3.7 Flash | Shapes the dashboard's user experience, accessibility, information hierarchy, responsive behavior, and visual design, including polished project cards and status and priority treatments. | `.github/agents/designer.agent.md` |
| **Coder** | GPT-5.5 | Implements the assigned dashboard logic and runnable-app support, following repository patterns and validating the changes. | `.github/agents/coder.agent.md` |

The Orchestrator delegates planning to the Planner, then coordinates the
Designer and Coder with explicit scopes so their work can be integrated cleanly.
