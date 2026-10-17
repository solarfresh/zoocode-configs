# ZooCode Agent Configurations

A structured repository for managing **ZooCode** Custom Modes (Agents) and Rule-based workflows as code. This setup provides modular agent personas with precise tool boundaries and standardized quality controls to streamline AI-assisted software engineering.

## 📁 Repository Structure

```text
.
├── LICENSE
├── README.md
└── zoocode/
    ├── custom-modes/        # Modular agent definitions in YAML
    │   ├── code-reviewer.yaml
    │   ├── task-decomposer.yaml
    │   └── unit-test-runner.yaml
    └── rules/               # Shared quality & workflow guidelines
        └── quality-workflow.md

```

---

## 🤖 Defined Custom Modes (Agents)

Each agent in `zoocode/custom-modes/` is tailored for a specific phase of the development lifecycle:

| Agent / Mode | Config File | Key Responsibilities & Capabilities |
| --- | --- | --- |
| **Task Decomposer** | `task-decomposer.yaml` | Breaks down feature requests or complex problems into structured, actionable implementation steps and architectural interfaces. |
| **Unit Test Runner** | `unit-test-runner.yaml` | Executes, analyzes, and generates unit tests to ensure high code coverage and fault isolation before merging. |
| **Code Reviewer** | `code-reviewer.yaml` | Conducts strict static reviews focused on code quality, security boundaries, error handling, and adherence to project conventions. |

---

## 📋 Rules & Workflows

Shared guidelines under `zoocode/rules/` act as system constraints or on-demand instructions referenced by agents:

* **`quality-workflow.md`**: Defines the snapshot-driven closed-loop governance SOP and the 4-stage pipeline across all custom agent modes.


---

## 🚀 Getting Started

### Prerequisites

* [VS Code](https://code.visualstudio.com/) with the **ZooCode** extension installed.

---

## 🛠️ Maintenance & Best Practices

* **Adding a New Agent**: Create a new `<agent-slug>.yaml` file under `zoocode/custom-modes/` following the existing structure (`slug`, `name`, `roleDefinition`, `groups`, and `customInstructions`).
* **Updating Rules**: Modify files in `zoocode/rules/` to refine constraints when agents make recurring errors or when project conventions evolve.
* **Version Control**: Keep all agent configurations and workflows under Git to ensure team-wide alignment and auditability.

---

## 📄 License

This project is licensed under the [LICENSE](./LICENSE) file included in the repository.