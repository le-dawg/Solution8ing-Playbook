# Agentic Browser Automation (Bowser Architecture)

## Overview

Agentic browser automation represents a shift in how we approach UI testing and web-based workflows. Rather than relying solely on deterministic scripts, engineers can assemble a reusable stack of skills, subagents, and prompts to offload entire classes of work—such as multi-browser UI validation and complex data extraction—to agents.

The **Bowser Architecture** is an opinionated, four-layer design for building repeatable and scalable agentic browser automation.

## The Four-Layer Architecture

### 1. Skill Layer: Foundational Capabilities

The skill layer defines the low-level capabilities available to an agent. At Solution8, we prioritize technologies that are token-efficient and flexible:

- **Claude + Chrome:** Using the `--d-chrome` flag to inject browser access directly into a Claude instance.
- **Playwright CLI:** Preferred over MCP servers for its efficiency and ability to handle parallel headless sessions and persistent login profiles.

### 2. Agent Layer: Specialized Subagents

Skills are lifted into specialized agents designed for specific tasks. For example:

- **Browser QA Agent:** This agent parses a user story into concrete steps, creates project directories, takes screenshots at each stage, and reports a clear pass/fail status.
- **Playwright Browser Agent:** Used for arbitrary, non-interactive automation.
- **Claude Browser Agent:** Used for interactive, human-in-the-loop browser control.

### 3. Orchestration Layer: Team Workflows

The orchestration layer composes agents and skills into end-to-end workflows using custom commands and reusable prompts.

- **`jui review`:** A command that kicks off multi-browser UI tests by spawning subagents to validate user stories in parallel.
- **Team Orchestration:** A primary agent acts as a "manager," spawning subagents as teammates and giving them explicit prompts for specific tasks. Results are then collected, merged, and reported as a unified summary.

### 4. Reusability Layer: Just Files and Command Runners

The top layer focuses on making these complex workflows easily accessible and repeatable.

- **`just` files:** We use `just` as a single entry point to catalog and run preconfigured commands. This exposes workflow permutations and variables for rapid invocation by both engineers and agents.
- **Higher-Order Prompts (hop):** A pattern where a prompt accepts another prompt as a parameter, wrapping workflows in a consistent, reusable routine.
- **Automated Workflows:** Storing automations (like "Amazon checkout" or "blog aggregation") as files in a dedicated directory allows browser tasks to scale from individual skills into robust, repeatable assets.

## Why Agentic UI Testing Scales

Agentic testing acts like a real user, which offers several advantages over conventional frameworks:

- **Reduced Overhead:** Removes much of the configuration and maintenance burden associated with deterministic test suites.
- **Rapid Creation:** Enables the quick generation of arbitrary workflows and user stories.
- **Traceability:** Agents can produce comprehensive UI summaries with screenshots for every step, ensuring clear audit trails.

**Note:** An optimal strategy often involves a hybrid of deterministic code tests (for critical logic) and agentic, non-deterministic testing (for broad UI validation).

## Risks and Mastery

While agentic automation provides a decisive advantage, it comes with specific responsibilities:

- **Retaining Control:** Engineers must build and understand their own skills and subagents rather than relying solely on external plugins or "borrowed" prompts. This is essential for differentiation and security.
- **Security Risks:** Outsourcing agent competence without mastery introduces risks like prompt injection and maintenance debt.
- **Specialization:** The ultimate advantage in the age of agents is the ability to combine scale with deep specialization in agent orchestration.
