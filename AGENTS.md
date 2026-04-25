# Claude Certified Architect Foundations

A repository for preparing for the Claude Certified Architect – Foundations (CCAF) exam.

It consolidates the exam guide, local mirrors of the official Claude API / Claude Code / MCP documentation, and transcripts of Anthropic Academy courses.

## Exam Content

[`./Official Exam Guide/Foundations Certification Exam Guide.md`](./Official%20Exam%20Guide/Foundations%20Certification%20Exam%20Guide.md) — The exam guide itself (Markdown version). Includes 5 domains and task statements, 12 sample questions with explanations, preparation exercises, and in-scope / out-of-scope topics.

### Exam Domains (Weights)

1. Agentic Architecture & Orchestration (27%)
2. Tool Design & MCP Integration (18%)
3. Claude Code Configuration & Workflows (20%)
4. Prompt Engineering & Structured Output (20%)
5. Context Management & Reliability (15%)

## Directory Structure

### Top-level Layout

| Path | Description |
|---|---|
| [`Official Exam Guide/`](./Official%20Exam%20Guide/) | Official exam package — `Welcome & what to expect.md`, `Foundations Certification Exam Guide.md` / `.pdf`, `Frequently Asked Questions.pdf` |
| [`Anthropic Academy/`](./Anthropic%20Academy/) | Transcripts of Anthropic Academy courses (`.txt`) plus `Anthropic Academy Course Catalog.pdf` |
| [`Practice Exam/`](./Practice%20Exam/) | Original screenshots of the Practice Exam, plus per-scenario Markdown transcribing the questions, choices, correct answers, and explanations |
| [`docs/`](./docs/) | Local mirror of the official Claude API / Claude Code / MCP documentation |
| [`.claude/`](./.claude/) | Claude Code project configuration — `settings.json` and `skills/` (`practice-exam`, `mock-exam`) |

---

### Official Exam Guide

Official exam package distributed by Anthropic.

| File | Description |
|---|---|
| [`Official Exam Guide/Welcome & what to expect.md`](./Official%20Exam%20Guide/Welcome%20&%20what%20to%20expect.md) | Welcome message and overview of the certification exam |
| [`Official Exam Guide/Foundations Certification Exam Guide.md`](./Official%20Exam%20Guide/Foundations%20Certification%20Exam%20Guide.md) | The exam guide itself (converted from PDF to Markdown). Contains domain definitions, task statements, 12 sample questions with explanations, preparation exercises, and appendices (in-scope / out-of-scope) |
| `Official Exam Guide/Foundations Certification Exam Guide.pdf` | Original PDF of the exam guide |
| `Official Exam Guide/Frequently Asked Questions.pdf` | Official exam FAQ |

---

### Practice Exam

Stores the questions for each Practice Exam scenario.

| File / Directory | Description |
|---|---|
| [`Practice Exam/Claude Code for Continuous Integration.md`](./Practice%20Exam/Claude%20Code%20for%20Continuous%20Integration.md) | Questions, choices, correct answers, and explanations for the Claude Code for Continuous Integration scenario |
| [`Practice Exam/Code Generation with Claude Code.md`](./Practice%20Exam/Code%20Generation%20with%20Claude%20Code.md) | Questions, choices, correct answers, and explanations for the Code Generation with Claude Code scenario |
| [`Practice Exam/Customer Support Resolution Agent.md`](./Practice%20Exam/Customer%20Support%20Resolution%20Agent.md) | Questions, choices, correct answers, and explanations for the Customer Support Resolution Agent scenario |
| [`Practice Exam/Multi-Agent Research System.md`](./Practice%20Exam/Multi-Agent%20Research%20System.md) | Questions, choices, correct answers, and explanations for the Multi-Agent Research System scenario |

---

### Anthropic Academy

Course transcript text files.

| File | Description |
|---|---|
| [`Anthropic Academy/Building with the Claude API.txt`](./Anthropic%20Academy/Building%20with%20the%20Claude%20API.txt) | Introduction to building applications with the Claude API |
| [`Anthropic Academy/Claude Code in Action.txt`](./Anthropic%20Academy/Claude%20Code%20in%20Action.txt) | Practical usage of Claude Code |
| [`Anthropic Academy/Introduction to Model Context Protocol.txt`](./Anthropic%20Academy/Introduction%20to%20Model%20Context%20Protocol.txt) | Overview course on MCP |
| [`Anthropic Academy/Model Context Protocol: Advanced Topics.txt`](./Anthropic%20Academy/Model%20Context%20Protocol:%20Advanced%20Topics.txt) | Advanced topics on MCP |
| `Anthropic Academy/Anthropic Academy Course Catalog.pdf` | Catalog of available Anthropic Academy courses |

---

### docs/ — Local Mirror of Official Documentation

#### docs/Claude API/

Claude API documentation. Base URL: `https://docs.claude.com/`.

| Path | Description |
|---|---|
| [`docs/Claude API/API usage primer for Claude.md`](./docs/Claude%20API/API%20usage%20primer%20for%20Claude.md) | API usage primer for Claude models |
| [`docs/Claude API/llms.txt`](./docs/Claude%20API/llms.txt) | Documentation index (llms.txt) |
| `docs/Claude API/API Reference/` | API reference — Messages / Completions / Models / Admin / Beta / Claude Code / Using the API / Support & configuration |
| `docs/Claude API/Admin/` | Organizational operations — 3rd-party platforms (Bedrock / Vertex) / Administration / Monitoring |
| `docs/Claude API/Build/` | Implementation guides — Building with Claude, Prompt engineering, Tools, MCP, Skills, Context management, Guardrails, Evals, Files, etc. |
| `docs/Claude API/Client SDKs/` | Official SDKs and provider compatibility |
| `docs/Claude API/Models & pricing/` | Model list and pricing |

#### docs/Claude Code/

All Claude Code documentation. Base URL: `https://docs.claude.com/en/docs/claude-code/`.

| Path | Description |
|---|---|
| `docs/Claude Code/Getting started/` | Getting started / Core concepts / Platforms and integrations (desktop, web, Code review & CI-CD) / Use Claude Code |
| `docs/Claude Code/ Configuration/` | Configuration — Interface / Model and responses / **Settings and permissions** (`Settings.md`, `Permissions.md`, `Sandboxing.md`) |
| `docs/Claude Code/Build with Claude Code/` | Extensions — Tools and plugins / Agents / Automation / Troubleshooting |
| `docs/Claude Code/Agent SDK/` | Full Agent SDK documentation set (the same content also exists under a separate `Agent SDK/`) |
| `docs/Claude Code/Administration/` | Setup and access / Security and data / Usage and costs / Plugin distribution |
| `docs/Claude Code/Deployment/` | Deployment configurations |
| `docs/Claude Code/Reference/` | CLI and settings reference |
| `docs/Claude Code/Resources/` | Supplementary resources |
| `docs/Claude Code/What's New/` | Release notes |

#### docs/Model Context Protocol/

MCP specification and related documentation. Base URL: `https://modelcontextprotocol.io/`.

| Path | Description |
|---|---|
| `docs/Model Context Protocol/Documentation/` | About MCP / Get started / Develop with MCP / Developer tools / Examples |
| `docs/Model Context Protocol/Specification/` | [`Specification.md`](./docs/Model%20Context%20Protocol/Specification/Specification.md), [`Architecture.md`](./docs/Model%20Context%20Protocol/Specification/Architecture.md), [`Schema Reference.md`](./docs/Model%20Context%20Protocol/Specification/Schema%20Reference.md), [`Key Changes.md`](./docs/Model%20Context%20Protocol/Specification/Key%20Changes.md), Base Protocol / Client Features / Server Features |
| `docs/Model Context Protocol/Extensions/` | [`Extensions Overview.md`](./docs/Model%20Context%20Protocol/Extensions/Extensions%20Overview.md), [`Extension Support Matrix.md`](./docs/Model%20Context%20Protocol/Extensions/Extension%20Support%20Matrix.md), Authorization Extensions, MCP Apps |
| `docs/Model Context Protocol/Registry/` | [`About.md`](./docs/Model%20Context%20Protocol/Registry/About.md), [`Quickstart: Publish a Server.md`](./docs/Model%20Context%20Protocol/Registry/Quickstart:%20Publish%20a%20Server.md), [`FAQ.md`](./docs/Model%20Context%20Protocol/Registry/FAQ.md), Consuming / Publishing, [`Terms of Service.md`](./docs/Model%20Context%20Protocol/Registry/Terms%20of%20Service.md) |
