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

Why B is incorrect:
Moving all guidance into Skills files means that universal coding standards and testing conventions won't be automatically loaded into every conversation. Since these standards should always be applied, they belong in CLAUDE.md where they are included by default.

Why C is incorrect:
Path-specific rules in `.claude/rules/` are designed to apply rules based on file types being edited, not based on workflow tasks like PR reviews or deployments. This approach misapplies file-path matching to workflow-based distinctions that don't correspond to specific file types.

Why D is incorrect:
While using @import syntax to organize files improves maintainability, all imported content would still be loaded into every conversation. This does not solve the core requirement of conditionally loading PR review, deployment, and migration guidance only when those tasks are being performed.

Study Area: Code Generation with Claude Code — review Skills vs CLAUDE.md Scope concepts in the exam study guide.

## Question 2

Your team has been using Claude Code for several months. Recently, three developers report that Claude correctly follows your "always include comprehensive error handling" guideline, but a fourth developer who just joined reports Claude isn't following this guideline. All four developers are working in the same repository and have the latest code pulled. What's the most likely cause and appropriate fix?

A) Claude Code caches `CLAUDE.md` contents after first read. The original developers have cached versions while the new developer loaded after the file was modified. Have all developers clear their Claude Code cache.

B) Claude Code builds per-user preference models over time through repeated interactions. The new developer needs to repeatedly specify the error handling requirement until Claude learns their preferences.

C) The guideline exists in the original developers' `~/.claude/CLAUDE.md` files (user-level) instead of the project's .claude/CLAUDE.md. Move the instruction to the project-level file so all team members receive it.

D) The new developer's `~/.claude/CLAUDE.md` contains conflicting instructions that override the project settings. Have them remove the conflicting section from their user-level configuration.

Correct Answer: C
This is the most likely cause: if the error handling guideline was added to each original developer's user-level `~/.claude/CLAUDE.md` rather than the project's .claude/CLAUDE.md, new team members would not receive it. Moving the instruction to the project-level configuration file ensures all current and future team members automatically receive the guideline.

Why A is incorrect:
Claude Code reads CLAUDE.md files fresh at the start of each session rather than caching them across sessions. There is no cross-session caching mechanism that would cause different developers to see different versions of the same configuration file.

Why B is incorrect:
Claude Code does not build persistent per-user preference models through repeated interactions across sessions. Each session starts fresh using the instructions from CLAUDE.md files and the current conversation context, so repeatedly specifying a requirement would not cause Claude to "learn" it permanently.

Why D is incorrect:
While conflicting user-level instructions could theoretically cause issues, it is unlikely that a newly joined developer would already have pre-existing conflicting configurations in their user-level CLAUDE.md file. This scenario doesn't explain why the three existing developers all have the guideline working consistently.

Study Area: Code Generation with Claude Code — review CLAUDE.md Configuration Hierarchy concepts in the exam study guide.

## Question 3

You've asked Claude Code to implement a function that transforms API responses into a normalized internal format. After two iterations, the output structure still doesn't match expectations—some fields are nested differently and timestamps aren't formatted correctly. You've been describing the requirements in prose, but Claude seems to interpret them differently each time. What's the most effective approach for the next iteration?

A) Ask Claude to explain its current interpretation of the requirements so you can identify where understanding diverges.

B) Write a JSON schema defining the expected output structure and validate Claude's output against it after each iteration.

C) Rewrite your requirements with greater technical precision, specifying exact field mappings, nesting rules, and timestamp format strings.

D) Provide 2-3 concrete input-output examples showing the expected transformation for representative API responses.

Correct Answer: D
Providing concrete input-output examples is the most effective approach because it eliminates the ambiguity inherent in prose descriptions by showing Claude exactly what the expected transformation looks like. This directly addresses the root cause—misinterpretation of prose requirements—by giving unambiguous, concrete targets for field nesting and timestamp formatting.

Why A is incorrect:
Asking Claude to explain its interpretation is a useful diagnostic step but doesn't directly solve the problem. After identifying the misunderstanding, you would still need another method—such as providing concrete examples—to communicate the correct transformation, making this an indirect and incomplete approach.

Why B is incorrect:
A JSON schema can validate the output structure but doesn't help Claude understand the actual transformation logic needed to produce correct results. This approach addresses verification rather than comprehension, meaning Claude would still need to implement the mapping requirements to generate correct output in the first place.

Why C is incorrect:
While more precise prose descriptions might help marginally, this approach continues relying on the same communication method that has already failed twice. Prose, no matter how technically precise, is still subject to interpretation differences, whereas concrete examples would eliminate ambiguity entirely.

Study Area: Code Generation with Claude Code — review Iterative Refinement concepts in the exam study guide.

## Question 4

Your team has created a `/migration` skill that generates database migration files. The skill accepts a migration name via `$ARGUMENTS` . In production, you're seeing three issues: (1) developers often invoke the skill without arguments, resulting in poorly-named files, (2) the skill sometimes incorporates database schema details from unrelated earlier conversations, and (3) a developer accidentally triggered destructive test cleanup when the skill had broad tool access. Which configuration approach addresses all three issues?

A) Add argument-hint frontmatter to prompt for required parameters, use context: fork to isolate execution, and restrict allowed- tools to file write operations.

B) Split into separate `/migration`-create and `/migration`-apply skills, add instructions in each SKILL.md to request a migration name if not provided, and use different allowed-tools scopes for each skill.

C) Use positional parameters $1 and $2 instead of `$ARGUMENTS` to enforce specific inputs, include explicit schema file references via @ syntax to control context, and add description frontmatter warning about destructive operations.

D) Include validation instructions in the skill''s SKILL.md that direct Claude to verify `$ARGUMENTS` contains a valid name, add prompts to ignore prior conversation context, and list forbidden operations Claude should avoid.

Correct Answer: A
This approach correctly uses three distinct skill configuration features to address each issue: 'argument-hint' frontmatter shows expected parameters during autocomplete (addressing missing arguments), 'context: fork isolates execution in a subagent context separate from conversation history (preventing context bleeding from earlier conversations), and 'allowed-tools restricts tool access to only file write operations (preventing destructive actions).

Why B is incorrect:
Splitting into separate skills doesn't inherently solve the context isolation problem since both skills would still share conversation history without `context: fork`, and relying on SKILL.md instructions to request a migration name if not provided lacks the autocomplete hints at invocation time.

Why C is incorrect:
Positional parameters like `$1` and `$2` don't enforce argument presence since they simply resolve to empty strings if not provided, `@` file references control what's explicitly included but don't prevent context bleeding from prior conversation turns, and `description` frontmatter only appears in help text without actually restricting tool access to prevent destructive operations.

Why D is incorrect:
This approach relies entirely on prompt-based instructions, which are unreliable for enforcement—telling Claude to "ignore prior context" doesn't actually isolate execution context, and listing forbidden operations doesn't guarantee Claude won't make a mistake. Skills provide configuration features specifically designed for these problems, and using them is more reliable than prompt-based instructions.

Study Area: Code Generation with Claude Code — review Custom Slash Commands concepts in the exam study guide.

## Question 5

Your team created an /analyze-codebase skill that performs comprehensive code analysis—dependency scanning, test coverage calculation, and code quality metrics. After running this command, team members report that Claude becomes less responsive in the session and loses track of their original task. What's the most effective way to address this while preserving full analysis capability?

A) Add context: fork to the skill's frontmatter to run the analysis in an isolated sub-agent context

B) Add instructions to the skill to compress all outputs into a brief summary before displaying

C) Add model: haiku to the frontmatter to use a faster, more efficient model for the analysis

D) Split the skill into three smaller skills that each generate less output

Correct Answer: A
Using context: fork' in the skill's frontmatter runs the analysis in an isolated sub-agent context, which prevents the verbose output from polluting the main conversation's context window and causing Claude to lose track of the original task. This preserves full analysis capability while keeping the main session responsive.

Why B is incorrect:
Compressing all outputs into a brief summary would sacrifice the full analysis capability that the question requires to be preserved. While it might reduce context pollution, it fundamentally undermines the purpose of comprehensive code analysis by discarding detailed results.

Why C is incorrect:
Switching to a faster model addresses speed and cost but does not solve the actual issue, which is context window pollution from verbose analysis output causing Claude to lose track of the user's original task. The responsiveness problem stems from context overload, not model performance.

Why D is incorrect:
Splitting into three smaller skills doesn't solve the core problem, because running them sequentially still produces the same total volume of output in the main conversation context. The combined output from three skills would pollute the context window just as much as one large skill.

Study Area: Code Generation with Claude Code — review Custom Slash Commands concepts in the exam study guide.

## Question 6

You're adding error handling wrappers to external API calls across a 120-file codebase. The task has three phases: (1) discovering all API call locations and patterns, (2) designing the error handling approach collaboratively, and (3) implementing wrappers consistently. During Phase 1, Claude generates verbose output listing hundreds of call sites with context. Your context window is filling rapidly before you've finished discovery. What's the most effective approach to complete this while maintaining implementation consistency?

A) Use the Explore subagent for Phase 1 to isolate verbose output and return a summary, then continue Phases 2-3 in the main conversation.

B) Define your error handling pattern in `CLAUDE.md`, then process files in batches across multiple sessions, relying on the shared memory file for consistency.

C) Switch to headless mode with --continue, passing explicit context summaries between batch invocations to maintain continuity.

D) Continue all phases in the main conversation, using /compact periodically to reduce context usage as you progress through the files.

Correct Answer: A
Using the Explore subagent for Phase 1 is ideal because it isolates the verbose discovery output in a separate context, returning only a concise summary to the main conversation. This preserves the main context window for the collaborative design and consistent implementation phases where retained context is most valuable.

Why B is incorrect:
Splitting work across multiple sessions loses the in-session context about nuanced decisions and edge cases encountered during discovery, and CLAUDE.md alone is insufficient to capture all the detailed rationale needed for consistent implementation across 120 files.

Why C is incorrect:
Headless mode with --continue is designed for automation rather than interactive collaborative work, and manually passing context summaries between invocations adds significant overhead that subagents handle automatically and far better.

Why D is incorrect:
While /compact can reclaim context space, it is a lossy compression that may discard important pattern details and edge cases discovered during Phase 1, undermining the consistency needed for Phases 2 and 3.

Study Area: Code Generation with Claude Code — review Subagent Delegation Strategy concepts in the exam study guide.

## Question 7

Your team's `CLAUDE.md` file has grown to over 500 lines, mixing TypeScript conventions, testing guidelines, API patterns, and deployment procedures. Developers find it difficult to locate and update relevant sections. What approach does Claude Code support for organizing project-level instructions into focused, topic-specific modules?

A) Create multiple files named `CLAUDE.md` at different levels of the directory tree, each one overriding the parent's instructions

B) Create separate markdown files in `.claude/rules/` , each covering one topic (e.g., testing.md , api-conventions.md )

C) Define a .claude/config.yaml file that maps file patterns to specific sections within `CLAUDE.md`

D) Split instructions into README.md files in relevant subdirectories, which Claude automatically loads as instructions

Correct Answer: B
This is correct. Claude Code supports a .claude/rules/ directory where you can create separate markdown files for topic-specific guidelines (e.g., testing.md, 'api-conventions.md' ), allowing teams to organize large instruction sets into focused, maintainable modules.

Why A is incorrect:
While Claude Code does support placing CLAUDE.md files at different levels of the directory tree, this feature provides context scoped to different parts of the codebase rather than solving the problem of organizing a single project's many guidelines into topic-specific modules. Multiple CLAUDE.md files also do not override parent instructions; they supplement them.

Why C is incorrect:
A `.claude/config.yaml` file that maps file patterns to specific sections within CLAUDE.md does not exist as a Claude Code feature. There is no supported mechanism for conditionally loading sections of the CLAUDE.md file based on file pattern matching through a config file.

Why D is incorrect:
README.md files serve as human-facing documentation and are not automatically loaded by Claude Code as project instructions. Only CLAUDE.md files and files in the `.claude/rules/` directory are recognized as instruction sources for Claude.

Study Area: Code Generation with Claude Code — review CLAUDE.md Modular Organization concepts in the exam study guide.

## Question 8

You want to create acustom /review slash command that runs your team's standard code review checklist. This command should be available to every developer when they clone or pull the repository. Where should you create this command file?

A) In the `CLAUDE.md` file at the project root

B) In `~/.claude/commands/` in each developer's home directory

C) In a .claude/config.json file with a commands array

D) In the `.claude/commands/` directory in the project repository

Correct Answer: D
Placing custom slash commands in the .claude/commands/ directory within the project repository is correct because these files are version-controlled and automatically available to every developer who clones or pulls the repo. This is the designated location for project- scoped custom commands in Claude Code.

Why A is incorrect:
The `CLAUDE.md` file at the project root is used for project instructions, context, and conventions that guide Claude's behavior, not for defining custom slash commands. Command definitions require their own dedicated files in the appropriate commands directory.

Why B is incorrect:
Storing commands in `~/.claude/commands/` in each developer's home directory creates personal, user-scoped commands that are not shared via version control. This approach would require each developer to manually set up the command, defeating the goal of automatic availability upon cloning or pulling the repository.

Why C is incorrect:
A `.claude/config.json` file with a `commands` array is not a valid mechanism for defining custom slash commands in Claude Code. This configuration format does not exist, and commands must be created as individual files in the appropriate commands directory.

Study Area: Code Generation with Claude Code — review Custom Slash Commands concepts in the exam study guide.

## Question 9

You're creating a custom /explore-alternatives skill that your team uses to brainstorm and evaluate different implementation approaches before committing to one. However, developers report that after running this skill, Claude's subsequent responses are influenced by the exploration discussion—sometimes referencing abandoned approaches or maintaining exploratory context that confuses actual implementation work. What's the most effective way to configure this skill?

A) Add context: fork to the skill's frontmatter.

B) Use the ! prefix inthe skill to execute the exploration logic as a bash subprocess.

C) Split the skill into two separate skills— /explore-start and /explore-end —to demarcate when exploration context should be discarded.

D) Create the skill in `~/.claude/skills/` instead of `.claude/skills/` .

Correct Answer: A
The 'context: fork' frontmatter option runs the skill in an isolated sub-agent context, so the exploration discussion does not pollute the main conversation history. This prevents abandoned approaches and exploratory context from influencing subsequent implementation work.

Why B is incorrect:
Using the `!` prefix to execute exploration logic as a bash subprocess misunderstands the feature—bash output still flows back into the conversation context, and meaningful brainstorming and evaluation of implementation approaches requires LLM reasoning rather than shell commands.

Why C is incorrect:
Splitting the skill into start and end commands does not solve the problem because there is no mechanism to discard context mid-conversation—conversation history is additive, and creating two skills cannot retroactively remove prior discussion from the context window.

Why D is incorrect:
Placing the skill in the user-level `~/.claude/skills/` directory instead of the project-level `.claude/skills/` directory only affects discoverability and precedence of the skill, not how its execution context is managed or isolated from subsequent conversation.

Study Area: Code Generation with Claude Code — review Custom Slash Commands concepts in the exam study guide.

## Question 10

You've created a /commit skill in `.claude/skills/`commit/SKILL.md that your team uses. One developer wants to customize it for their personal workflow (different commit message format, additional checks) without affecting teammates. What should you recommend?

A) Create a personal version at `~/.claude/skills/`commit/SKILL.md with the same name

B) Create a personal versionin `~/.claude/skills/` with a different name like /my-commit

C) Add username-based conditional logic to the project skill's frontmatter

D) Set override: true inthe personal skill's frontmatter to take precedence over the project version

Correct Answer: B
This is correct. Since project skills take precedence over personal skills with the same name, the developer must use a different skill name (like /my-commit ) in their personal `~/.claude/skills/` directory to ensure their custom version is accessible alongside the team's project skill.

Why A is incorrect:
Project skills in `.claude/skills/` take precedence over personal skills in `~/.claude/skills/` when they share the same name, so creating a personal `/commit` skill would be shadowed by the project's version and never invoked.

Why C is incorrect:
Skill frontmatter does not support username-based conditional logic, and modifying the shared project skill to handle individual developer preferences would unnecessarily complicate the team's shared configuration.

Why D is incorrect:
There is no `override: true` frontmatter option in the skill system; this feature does not exist, so a personal skill cannot be configured to take precedence over a project skill with the same name.

Study Area: Code Generation with Claude Code — review Custom Slash Commands concepts in the exam study guide.

## Question 11

You've been assigned to restructure the team's monolithic application into microservices. This will involve changes across dozens of files and requires decisions about service boundaries and module dependencies. Which approach should you take?

A) Begin in direct execution mode and only switch to plan mode if you encounter unexpected complexity during implementation.

B) Start with direct execution and make changes incrementally, letting the implementation reveal the natural service boundaries.

C) Use direct execution with comprehensive upfront instructions detailing exactly how each service should be structured.

D) Enter plan mode to explore the codebase, understand dependencies, and design an implementation approach before making changes.

Correct Answer: D
Using plan mode to explore the codebase, understand dependencies, and design an approach before making changes is the correct strategy for a complex architectural restructuring like breaking apart a monolith. This allows safe exploration and informed decision-making about service boundaries before committing to potentially costly changes across dozens of files.

Why A is incorrect:
Starting in direct execution mode and only switching to plan mode if unexpected complexity arises ignores the fact that the task is already known to be complex—involving dozens of files, service boundary decisions, and module dependencies. The need for planning is evident from the requirements themselves, not something that might emerge later.

Why B is incorrect:
Starting with direct execution and making incremental changes without upfront planning risks costly rework when hidden dependencies and architectural conflicts are discovered late in the process. A monolith-to-microservices migration requires understanding the full dependency graph before making structural decisions, not discovering it piecemeal.

Why C is incorrect:
Providing comprehensive upfront instructions for exact service structure assumes you already know the right architecture without first exploring the existing codebase and its dependencies. This approach bypasses the critical discovery phase needed to make informed decisions about service boundaries in a monolithic application.

Study Area: Code Generation with Claude Code — review Plan Mode vs Direct Execution concepts in the exam study guide.

## Question 12

You need to add Slack as a new notification channel. The existing codebase has clear, consistent patterns for email, SMS, and push channels. However, the Slack API offers fundamentally different integration approaches—incoming webhooks (simple, one-way only), bot tokens (enables delivery confirmation and programmatic control), or Slack Apps (bidirectional events, requires workspace approval). Your ticket says "add Slack support" without specifying which integration method or whether advanced features like delivery tracking will be needed. How should you approach this task?

A) Start direct execution using incoming webhooks to match the existing one-way notification pattern.

B) Start direct execution using the bot token approach to enable delivery confirmation capabilities.

C) Start direct execution to scaffold the Slack channel class following existing patterns, deferring the integration method decision until later.

D) Enter plan mode to explore the integration options and their architectural implications, then present a recommendation before implementing.

Correct Answer: D
This is correct because the Slack integration involves multiple valid approaches with significantly different architectural implications, and the requirements are ambiguous. Using plan mode to explore trade-offs between webhooks, bot tokens, and Slack Apps allows for an informed recommendation and team alignment before committing to an implementation path.

Why A is incorrect:
While incoming webhooks match the existing one-way notification pattern, this approach prematurely commits to a specific integration method without evaluating whether advanced features like delivery tracking might be needed. The ambiguous requirements warrant exploring all options before making an architectural decision.

Why B is incorrect:
Although bot tokens enable delivery confirmation, jumping directly to this approach assumes that delivery tracking is a requirement when the ticket doesn't specify this. Choosing a more complex integration method without evaluating trade-offs risks over-engineering the solution.

Why C is incorrect:
This approach incorrectly assumes that meaningful scaffolding can proceed before choosing the integration method, when in reality the authentication flows, response handling, and error patterns differ significantly between webhooks, bot tokens, and Slack Apps. Deferring the integration method decision would likely result in rework once the approach is selected.

Study Area: Code Generation with Claude Code — review Plan Mode vs Direct Execution concepts in the exam study guide.

## Question 13

You've found that including 2-3 full exemplar endpoint implementations as context significantly improves consistency when generating new API endpoints. However, this context is only useful for creating new endpoints—not for bug fixes, code reviews, or other API directory work. What's the most efficient configuration approach?

A) Configure path-specific rules in `.claude/rules/`api/ that include the exemplar code and activate when working in the API directory.

B) Reference the exemplar endpoints manually in each generation request by copying relevant code into your prompt.

C) Add the exemplar endpoint code with pattern documentation to the project `CLAUDE.md` file so it's automatically available.

D) Create a skill that references the exemplar endpoints and includes pattern-following instructions, invoked on-demand via slash command.

Correct Answer: D
Creating a skill with the exemplar endpoints and pattern-following instructions allows on-demand invocation via a slash command, ensuring the context is loaded only when generating new endpoints and not during unrelated tasks like bug fixes or code reviews.

Why A is incorrect:
Path-specific rules that activate when working in the API directory would trigger for all API directory work, including bug fixes and code reviews—exactly the scenarios where the exemplar context is unnecessary and wasteful.

Why B is incorrect:
Manually copying exemplar endpoint code into each prompt would technically work, but it is tedious, error-prone, and fails to leverage Claude Code's built-in configuration capabilities for reusable context loading.

Why C is incorrect:
Adding exemplar endpoint code to the project CLAUDE.md file would load this context for every session regardless of the task type, wasting context window on bug fixes, code reviews, and other work where it isn't needed.

Study Area: Code Generation with Claude Code — review Custom Slash Commands concepts in the exam study guide.

## Question 14

Your codebase has distinct areas with different coding conventions: React components use functional style with hooks, API handlers use async/await with specific error handling, and database models follow a repository pattern. Test files are spread throughout the codebase alongside the code they test (e.g., Button.test.tsx next to Button.tsx ),and you want all tests to follow the same conventions regardless of location. What's the most maintainable way to ensure Claude automatically applies the correct conventions when generating code?

A) Create skills in `.claude/skills/` for each code type that include the relevant conventions in their SKILL.md files

B) Place a separate `CLAUDE.md` file in each subdirectory containing that area's specific conventions

C) Consolidate all conventions in the root `CLAUDE.md` file under headers for each area, relying on Claude to infer which section applies

D) Create rule files in `.claude/rules/` with YAML frontmatter specifying glob patterns to conditionally apply conventions based on file paths

Correct Answer: D
Using rule files in .claude/rules/ with YAML frontmatter and glob patterns (e.g., '/.test.tsx, ' src/api//.ts ) allows conventions to be automatically and deterministically applied based on file paths, regardless of where those files are located in the directory structure. This is the most maintainable approach because it handles cross-cutting concerns like test files spread throughout the codebase without requiring duplication or manual intervention.

Why A is incorrect:
Skills in `.claude/skills/` are designed for task-based workflows and typically require manual invocation or rely on Claude choosing to load them, which contradicts the requirement for automatic, deterministic application of conventions based on file paths. This approach lacks the glob-pattern-based conditional triggering needed to reliably match conventions to specific file types.

Why B is incorrect:
Placing CLAUDE.md files in each subdirectory works for directory-scoped conventions but cannot efficiently handle test files that are co-located with source files across many directories, since you would need to duplicate test conventions in every directory containing tests. This approach becomes unmaintainable as the codebase grows and doesn't support cross-cutting concerns that span multiple directories.

Why C is incorrect:
Consolidating all conventions in a single root CLAUDE.md and relying on Claude to infer which section applies is unreliable because there is no deterministic mechanism ensuring the correct conventions are matched to the correct files. This approach may lead to inconsistent application of conventions, especially for files like tests that exist alongside different code types throughout the codebase.

Study Area: Code Generation with Claude Code — review Path-Specific Rule Configuration concepts in the exam study guide.

## Question 15

Your team wants to add a GitHub MCP server to enable PR lookups and CI status checks through Claude Code. Each of the six developers has their own GitHub personal access token. You want consistent tooling across the team without committing credentials to version control. What's the most effective configuration approach?

A) Add the server to a project-scoped `.mcp.json` with environment variable expansion ( `${GITHUB_TOKEN}` ) for authentication, and document the required environment variable in your project README.

B) Have each developer configure the server in user scope with claude mcp add --scope user,

C) Create an MCP server wrapper that reads tokens from a .env file and proxies requests to the GitHub API, then add this wrapper to your project `.mcp.json`.

D) Configure the server in project scope with a placeholder token value, then instruct developers to override it in their local scope configuration.

Correct Answer: A
Using a project-scoped .mcp.json with environment variable expansion ( `${GITHUB_TOKEN}`') is the idiomatic approach—it provides a single, version-controlled source of truth for the team's MCP configuration while allowing each developer to supply their own credentials through environment variables. Documenting the required variable in the README ensures easy onboarding without ever committing secrets.

Why B is incorrect:
Having each developer independently configure the server in user scope fails the consistency requirement, as six separate configurations will inevitably diverge over time with no shared source of truth. This approach also increases onboarding friction since new developers must manually replicate the correct setup.

Why C is incorrect:
Building a custom wrapper to proxy GitHub API requests and read tokens from a `.env` file is over-engineered, since Claude Code's native environment variable expansion in `.mcp.json` already solves the credential injection problem cleanly. This approach introduces additional maintenance burden and potential points of failure without meaningful benefit.

Why D is incorrect:
Committing a placeholder token value to the project-scoped configuration is an anti-pattern that could be mistaken for a real credential and relies on a fragile override mechanism that developers may forget or misconfigure. This approach adds unnecessary complexity compared to native environment variable expansion.

Study Area: Code Generation with Claude Code — review MCP Server Integration concepts in the exam study guide.
