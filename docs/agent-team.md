# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the custom agent team building Mona's Project Pulse dashboard.

- **Orchestrator** (Claude Opus 5.5): Coordinates specialists, assigns work by phase and file scope, and integrates and verifies their results. Definition: `.github/agents/orchestrator.agent.md`.
- **Planner** (Claude Opus 5.5): Researches the repository and dependencies, identifies risks and edge cases, and creates an implementation plan; does not write code. Definition: `.github/agents/planner.agent.md`.
- **Designer** (Gemini 3.7 Flash): Shapes the dashboard's UI/UX, accessibility, information hierarchy, responsive behavior, and visual design. Definition: `.github/agents/designer.agent.md`.
- **Coder** (GPT-5.5): Implements assigned application logic and support files, follows repository patterns, and validates changes. Definition: `.github/agents/coder.agent.md`.
