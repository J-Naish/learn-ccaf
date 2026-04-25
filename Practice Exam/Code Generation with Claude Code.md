# Code Generation with Claude Code

You are using **Claude Code** to accelerate software development. Your team uses it for code generation, refactoring, debugging, and documentation. You need to integrate it into your development workflow with custom slash commands, CLAUDE.md configurations, and understand when to use plan mode vs direct execution.

## Question 1

Your `CLAUDE.md` has grown to over 400 lines containing coding standards, testing conventions, a detailed PR review checklist, deployment workflow instructions, and database migration procedures. You want Claude to always follow the coding standards and testing conventions, but only apply PR review, deployment, and migration guidance when you're actually performing those tasks. What's the most effective restructuring approach?

A) Keep universal standards in `CLAUDE.md` and create Skills for task-specific workflows (PR reviews, deployments, migrations) with trigger keywords
B) Move all guidance into separate Skills files organized by workflow type, keeping only a brief project description in `CLAUDE.md`
C) Split the `CLAUDE.md` into files in `.claude/rules/` with path-specific glob patterns so each rule loads only for matching file types
D) Keep all content in `CLAUDE.md` but use @import syntax to organize it into separately maintained files by category

Correct Answer: A
This is the most effective approach because `CLAUDE.md` content is loaded for every conversation, ensuring coding standards and testing conventions are always applied, while Skills are invoked on-demand when Claude detects relevant trigger keywords, making them ideal for task-specific workflows like PR reviews, deployments, and migrations.

## Question 2

Your team has been using Claude Code for several months. Recently, three developers report that Claude correctly follows your "always include comprehensive error handling" guideline, but a fourth developer who just joined reports Claude isn't following this guideline. All four developers are working in the same repository and have the latest code pulled. What's the most likely cause and appropriate fix?

A) Claude Code caches `CLAUDE.md` contents after first read. The original developers have cached versions while the new developer loaded after the file was modified. Have all developers clear their Claude Code cache.
B) Claude Code builds per-user preference models over time through repeated interactions. The new developer needs to repeatedly specify the error handling requirement until Claude learns their preferences.
C) The guideline exists in the original developers' `~/.claude/CLAUDE.md` files (user-level) instead of the project's .claude/CLAUDE.md. Move the instruction to the project-level file so all team members receive it.
D) The new developer's `~/.claude/CLAUDE.md` contains conflicting instructions that override the project settings. Have them remove the conflicting section from their user-level configuration.

Correct Answer: C
This is the most likely cause: if the error handling guideline was added to each original developer's user-level `~/.claude/CLAUDE.md` rather than the project's .claude/CLAUDE.md, new team members would not receive it. Moving the instruction to the project-level configuration file ensures all current and future team members automatically receive the guideline.

## Question 3

You've asked Claude Code to implement a function that transforms API responses into a normalized internal format. After two iterations, the output structure still doesn't match expectations—some fields are nested differently and timestamps aren't formatted correctly. You've been describing the requirements in prose, but Claude seems to interpret them differently each time. What's the most effective approach for the next iteration?

A) Ask Claude to explain its current interpretation of the requirements so you can identify where understanding diverges.
B) Write a JSON schema defining the expected output structure and validate Claude's output against it after each iteration.
C) Rewrite your requirements with greater technical precision, specifying exact field mappings, nesting rules, and timestamp format strings.
D) Provide 2-3 concrete input-output examples showing the expected transformation for representative API responses.

Correct Answer: D
Providing concrete input-output examples is the most effective approach because it eliminates the ambiguity inherent in prose descriptions by showing Claude exactly what the expected transformation looks like. This directly addresses the root cause—misinterpretation of prose requirements—by giving unambiguous, concrete targets for field nesting and timestamp formatting.

## Question 4

Your team has created a `/migration` skill that generates database migration files. The skill accepts a migration name via `$ARGUMENTS` . In production, you're seeing three issues: (1) developers often invoke the skill without arguments, resulting in poorly-named files, (2) the skill sometimes incorporates database schema details from unrelated earlier conversations, and (3) a developer accidentally triggered destructive test cleanup when the skill had broad tool access. Which configuration approach addresses all three issues?

A) Add argument-hint frontmatter to prompt for required parameters, use context: fork to isolate execution, and restrict allowed- tools to file write operations.
B) Split into separate `/migration`-create and `/migration`-apply skills, add instructions in each SKILL.md to request a migration name if not provided, and use different allowed-tools scopes for each skill.
C) Use positional parameters $1 and $2 instead of `$ARGUMENTS` to enforce specific inputs, include explicit schema file references via @ syntax to control context, and add description frontmatter warning about destructive operations.
D) Include validation instructions in the skill''s SKILL.md that direct Claude to verify `$ARGUMENTS` contains a valid name, add prompts to ignore prior conversation context, and list forbidden operations Claude should avoid.

Correct Answer: A
This approach correctly uses three distinct skill configuration features to address each issue: 'argument-hint' frontmatter shows expected parameters during autocomplete (addressing missing arguments), 'context: fork isolates execution in a subagent context separate from conversation history (preventing context bleeding from earlier conversations), and 'allowed-tools restricts tool access to only file write operations (preventing destructive actions).

## Question 5

Your team created an /analyze-codebase skill that performs comprehensive code analysis—dependency scanning, test coverage calculation, and code quality metrics. After running this command, team members report that Claude becomes less responsive in the session and loses track of their original task. What's the most effective way to address this while preserving full analysis capability?

A) Add context: fork to the skill's frontmatter to run the analysis in an isolated sub-agent context
B) Add instructions to the skill to compress all outputs into a brief summary before displaying
C) Add model: haiku to the frontmatter to use a faster, more efficient model for the analysis
D) Split the skill into three smaller skills that each generate less output

Correct Answer: A
Using context: fork' in the skill's frontmatter runs the analysis in an isolated sub-agent context, which prevents the verbose output from polluting the main conversation's context window and causing Claude to lose track of the original task. This preserves full analysis capability while keeping the main session responsive.

## Question 6

You're adding error handling wrappers to external API calls across a 120-file codebase. The task has three phases: (1) discovering all API call locations and patterns, (2) designing the error handling approach collaboratively, and (3) implementing wrappers consistently. During Phase 1, Claude generates verbose output listing hundreds of call sites with context. Your context window is filling rapidly before you've finished discovery. What's the most effective approach to complete this while maintaining implementation consistency?

A) Use the Explore subagent for Phase 1 to isolate verbose output and return a summary, then continue Phases 2-3 in the main conversation.
B) Define your error handling pattern in `CLAUDE.md`, then process files in batches across multiple sessions, relying on the shared memory file for consistency.
C) Switch to headless mode with --continue, passing explicit context summaries between batch invocations to maintain continuity.
D) Continue all phases in the main conversation, using /compact periodically to reduce context usage as you progress through the files.

Correct Answer: A
Using the Explore subagent for Phase 1 is ideal because it isolates the verbose discovery output in a separate context, returning only a concise summary to the main conversation. This preserves the main context window for the collaborative design and consistent implementation phases where retained context is most valuable.

## Question 7

Your team's `CLAUDE.md` file has grown to over 500 lines, mixing TypeScript conventions, testing guidelines, API patterns, and deployment procedures. Developers find it difficult to locate and update relevant sections. What approach does Claude Code support for organizing project-level instructions into focused, topic-specific modules?

A) Create multiple files named `CLAUDE.md` at different levels of the directory tree, each one overriding the parent's instructions
B) Create separate markdown files in `.claude/rules/` , each covering one topic (e.g., testing.md , api-conventions.md )
C) Definea .claude/config.yaml file that maps file patterns to specific sections within `CLAUDE.md`
D) Split instructions into README.md files in relevant subdirectories, which Claude automatically loads as instructions

Correct Answer: B
This is correct. Claude Code supports a .claude/rules/ directory where you can create separate markdown files for topic-specific guidelines (e.g., testing.md, 'api-conventions.md' ), allowing teams to organize large instruction sets into focused, maintainable modules.

## Question 8

You want to create acustom /review slash command that runs your team's standard code review checklist. This command should be available to every developer when they clone or pull the repository. Where should you create this command file?

A) In the `CLAUDE.md` file at the project root
B) In `~/.claude/commands/` in each developer's home directory
C) In a .claude/config.json file with a commands array
D) In the `.claude/commands/` directory in the project repository

Correct Answer: D
Placing custom slash commands in the .claude/commands/ directory within the project repository is correct because these files are version-controlled and automatically available to every developer who clones or pulls the repo. This is the designated location for project- scoped custom commands in Claude Code.

## Question 9

You're creating acustom /explore-alternatives skill that your team uses to brainstorm and evaluate different implementation approaches before committing to one. However, developers report that after running this skill, Claude's subsequent responses are influenced by the exploration discussion—sometimes referencing abandoned approaches or maintaining exploratory context that confuses actual implementation work. What's the most effective way to configure this skill?

A) Add context: fork to the skill's frontmatter.
B) Use the ! prefix inthe skill to execute the exploration logic as a bash subprocess.
C) Split the skill into two separate skills— /explore-start and /explore-end —to demarcate when exploration context should be discarded.
D) Create the skill in `~/.claude/skills/` instead of `.claude/skills/` .

Correct Answer: A
The 'context: fork' frontmatter option runs the skill in an isolated sub-agent context, so the exploration discussion does not pollute the main conversation history. This prevents abandoned approaches and exploratory context from influencing subsequent implementation work.

## Question 10

You've created a /commit skill in `.claude/skills/`commit/SKILL.md that your team uses. One developer wants to customize it for their personal workflow (different commit message format, additional checks) without affecting teammates. What should you recommend?

A) Create a personal version at `~/.claude/skills/`commit/SKILL.md with the same name
B) Create a personal versionin `~/.claude/skills/` with a different name like /my-commit
C) Add username-based conditional logic to the project skill's frontmatter
D) Set override: true inthe personal skill's frontmatter to take precedence over the project version

Correct Answer: B
This is correct. Since project skills take precedence over personal skills with the same name, the developer must use a different skill name (like /my-commit ) in their personal `~/.claude/skills/` directory to ensure their custom version is accessible alongside the team's project skill.

## Question 11

You've been assigned to restructure the team's monolithic application into microservices. This will involve changes across dozens of files and requires decisions about service boundaries and module dependencies. Which approach should you take?

A) Begin in direct execution mode and only switch to plan mode if you encounter unexpected complexity during implementation.
B) Start with direct execution and make changes incrementally, letting the implementation reveal the natural service boundaries.
C) Use direct execution with comprehensive upfront instructions detailing exactly how each service should be structured.
D) Enter plan mode to explore the codebase, understand dependencies, and design an implementation approach before making changes.

Correct Answer: D
Using plan mode to explore the codebase, understand dependencies, and design an approach before making changes is the correct strategy for a complex architectural restructuring like breaking apart a monolith. This allows safe exploration and informed decision-making about service boundaries before committing to potentially costly changes across dozens of files.

## Question 12

You need to add Slack as a new notification channel. The existing codebase has clear, consistent patterns for email, SMS, and push channels. However, the Slack API offers fundamentally different integration approaches—incoming webhooks (simple, one-way only), bot tokens (enables delivery confirmation and programmatic control), or Slack Apps (bidirectional events, requires workspace approval). Your ticket says "add Slack support" without specifying which integration method or whether advanced features like delivery tracking will be needed. How should you approach this task?

A) Start direct execution using incoming webhooks to match the existing one-way notification pattern.
B) Start direct execution using the bot token approach to enable delivery confirmation capabilities.
C) Start direct execution to scaffold the Slack channel class following existing patterns, deferring the integration method decision until later.
D) Enter plan mode to explore the integration options and their architectural implications, then present a recommendation before implementing.

Correct Answer: D
This is correct because the Slack integration involves multiple valid approaches with significantly different architectural implications, and the requirements are ambiguous. Using plan mode to explore trade-offs between webhooks, bot tokens, and Slack Apps allows for an informed recommendation and team alignment before committing to an implementation path.

## Question 13

You've found that including 2-3 full exemplar endpoint implementations as context significantly improves consistency when generating new API endpoints. However, this context is only useful for creating new endpoints—not for bug fixes, code reviews, or other API directory work. What's the most efficient configuration approach?

A) Configure path-specific rules in `.claude/rules/`api/ that include the exemplar code and activate when working in the API directory.
B) Reference the exemplar endpoints manually in each generation request by copying relevant code into your prompt.
C) Add the exemplar endpoint code with pattern documentation to the project `CLAUDE.md` file so it's automatically available.
D) Create a skill that references the exemplar endpoints and includes pattern-following instructions, invoked on-demand via slash command.

Correct Answer: D
Creating a skill with the exemplar endpoints and pattern-following instructions allows on-demand invocation via a slash command, ensuring the context is loaded only when generating new endpoints and not during unrelated tasks like bug fixes or code reviews.

## Question 14

Your codebase has distinct areas with different coding conventions: React components use functional style with hooks, API handlers use async/await with specific error handling, and database models follow a repository pattern. Test files are spread throughout the codebase alongside the code they test (e.g., Button.test.tsx next to Button.tsx ),and you want all tests to follow the same conventions regardless of location. What's the most maintainable way to ensure Claude automatically applies the correct conventions when generating code?

A) Create skills in `.claude/skills/` for each code type that include the relevant conventions in their SKILL.md files
B) Place a separate `CLAUDE.md` file in each subdirectory containing that area's specific conventions
C) Consolidate all conventions in the root `CLAUDE.md` file under headers for each area, relying on Claude to infer which section applies
D) Create rule files in `.claude/rules/` with YAML frontmatter specifying glob patterns to conditionally apply conventions based on file paths

Correct Answer: D
Using rule files in .claude/rules/ with YAML frontmatter and glob patterns (e.g., '/.test.tsx, ' src/api//.ts ) allows conventions to be automatically and deterministically applied based on file paths, regardless of where those files are located in the directory structure. This is the most maintainable approach because it handles cross-cutting concerns like test files spread throughout the codebase without requiring duplication or manual intervention.

## Question 15

Your team wants to add a GitHub MCP server to enable PR lookups and CI status checks through Claude Code. Each of the six developers has their own GitHub personal access token. You want consistent tooling across the team without committing credentials to version control. What's the most effective configuration approach?

A) Add the server to a project-scoped `.mcp.json` with environment variable expansion ( `${GITHUB_TOKEN}` ) for authentication, and document the required environment variable in your project README.
B) Have each developer configure the server in user scope with claude mcp add --scope user,
C) Create an MCP server wrapper that reads tokens from a .env file and proxies requests to the GitHub API, then add this wrapper to your project `.mcp.json`.
D) Configure the server in project scope with a placeholder token value, then instruct developers to override it in their local scope configuration.

Correct Answer: A
Using a project-scoped .mcp.json with environment variable expansion ( `${GITHUB_TOKEN}`') is the idiomatic approach—it provides a single, version-controlled source of truth for the team's MCP configuration while allowing each developer to supply their own credentials through environment variables. Documenting the required variable in the README ensures easy onboarding without ever committing secrets.
