# Agent team

For Mona's Project Pulse dashboard, I will use a custom specialist team orchestrated through GitHub Copilot CLI in a Codespace. The definitions live under the repository's `.github/agents/` folder and each agent is scoped to a clear responsibility.

| Agent | Target model | Responsibility | Agent definition |
| --- | --- | --- | --- |
| Planner | Claude Opus 5.5 (copilot) | Researches the repo, evaluates dependencies and edge cases, and produces a practical implementation plan before any code is written. | `.github/agents/planner.agent.md` |
| Designer | Gemini 3.7 Flash (copilot) | Focuses on UI/UX, information hierarchy, accessibility, interaction flow, and the visual polish needed for a dashboard experience. | `.github/agents/designer.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements the assigned code changes, keeps behavior deterministic and testable, and validates the work within the approved file scope. | `.github/agents/coder.agent.md` |
| Orchestrator | Claude Opus 5.5 (copilot) | Coordinates the Planner, Designer, and Coder, breaks the work into phases, delegates file ownership, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |

This setup gives the project a clear handoff model: the Planner defines the work, the Designer shapes the dashboard experience, the Coder builds the implementation, and the Orchestrator coordinates the execution in the GitHub Copilot CLI environment running in the Codespace.
