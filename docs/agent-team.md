# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the custom agents that build Mona's Project Pulse dashboard.

- **Orchestrator** - **Claude Opus 5.5 (copilot)** - Coordinates the Planner, Coder, and Designer, divides work into phases, manages dependencies and file ownership, and verifies the integrated result. It does not implement the dashboard itself. Definition: [.github/agents/orchestrator.agent.md](../.github/agents/orchestrator.agent.md)
- **Planner** - **Claude Opus 5.5 (copilot)** - Researches the codebase and requirements, identifies edge cases and dependencies, and produces an ordered implementation plan with file assignments. Definition: [.github/agents/planner.agent.md](../.github/agents/planner.agent.md)
- **Coder** - **GPT-5.5 (copilot)** - Implements the assigned application code and support configuration, including the Project Pulse launch setup, while validating behavior and reporting results. Definition: [.github/agents/coder.agent.md](../.github/agents/coder.agent.md)
- **Designer** - **Gemini 3.7 Flash (copilot)** - Defines and implements the dashboard's UI/UX, accessibility, responsive layout, visual hierarchy, project cards, status badges, and priority treatment. Definition: [.github/agents/designer.agent.md](../.github/agents/designer.agent.md)
