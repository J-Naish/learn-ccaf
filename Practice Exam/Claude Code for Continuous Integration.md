# Claude Code for Continuous Integration

You are integrating **Claude Code into your Continuous Integration/Continuous Deployment (CI/CD) pipeline**. The system runs automated code reviews, generates test cases, and provides feedback on pull requests. You need to design prompts that provide actionable feedback and minimize false positives.

## Question 1

You are integrating Claude Code into your Continuous Integration/Continuous Deployment (CI/CD) pipeline. The system runs automated code reviews, generates test cases, and provides feedback on pull requests. You need to design prompts that provide actionable feedback and minimize false positives. Your pipeline script runs claude "Analyze this pull request for security issues" but the job hangs indefinitely. Logs indicate Claude Code is waiting for interactive input. What's the correct approach to run Claude Code in an automated pipeline?

A) Add the --batch flag: claude --batch "Analyze this pull request for security issues"
B) Add the `-p` flag: claude `-p` "Analyze this pull request for security issues"
C) Redirect stdin from /dev/null: claude "Analyze this pull request for security issues" < /dev/null
D) Set the environment variable CLAUDE_HEADLESS=true before running the command

Correct Answer: B
The '-p (or --print) flag is the documented way to run Claude Code in non-interactive mode. It processes the given prompt, outputs the result to stdout, and exits without waiting for user input, making it ideal for CI/CD pipelines.

## Question 2

Your automated code review averages 15 findings per pull request, with developers reporting a 40% false positive rate. The bottleneck is investigation time: developers must click into each finding to read Claude's reasoning before deciding whether to address or dismiss it. Your `CLAUDE.md` already contains comprehensive rules for acceptable patterns, and stakeholders have rejected any approach that filters findings before developer review. What change would best address the investigation time bottleneck?

A) Require Claude to include its reasoning and confidence assessment inline with each finding
B) Add a post-processor that analyzes finding patterns and automatically suppresses those matching historical false positive signatures
C) Categorize findings as "blocking issues" versus "suggestions" with tiered review requirements
D) Configure Claude to only surface findings it assesses as high confidence, filtering out uncertain flags before developers see them

Correct Answer: A
Including reasoning and confidence assessments inline with each finding directly addresses the investigation time bottleneck by allowing developers to quickly evaluate findings without clicking into each one separately. This approach respects the constraint against filtering, since all findings remain visible while making triage significantly faster.

## Question 3

Your CI pipeline runs the Claude Code CLI (with `--print` mode) using `CLAUDE.md` to provide project context for code reviews, and developers generally find the reviews insightful. However, they report that integrating findings into your workflow is difficult—Claude produces narrative paragraphs that must be manually copied into PR comments. Your team wants to automatically post each finding as a separate inline PR comment at the relevant code location, which requires structured data with file path, line number, severity, and suggested fix. What's the most effective approach?

A) Add a "Review Output Format" section to `CLAUDE.md` with examples showing structured findings, so Claude learns the expected format from project context.
B) Keep the narrative review format but add a summarization step that uses Claude to generate a structured JSON summary of the findings.
C) Use CLI flags `--output-format json` and `--json-schema` to enforce structured findings, then parse output to post inline comments via the GitHub API.
D) Include explicit formatting instructions in your review prompt requiring each finding to follow a parseable template like [FILE:path] [LINE:n] [SEVERITY: level] ....

Correct Answer: C
Using --output-format json" with --json-schema' enforces structured output at the CLI level, guaranteeing well-formed JSON with the required fields (file path, line number, severity, suggested fix) that can be reliably parsed and posted as inline PR comments via the GitHub API. This is the most effective approach because it leverages native CLI capabilities designed specifically for structured output enforcement.

## Question 4

Analysis of your automated code review shows significant variation in false positive rates across finding categories. Security and correctness findings have an 8% false positive rate, performance findings have 18%, style and naming findings have 52%, and documentation findings have 48%. Developer surveys indicate growing distrust—many have started dismissing findings without review because "half are wrong." The high false positive categories are undermining confidence in the accurate categories. What approach best restores developer trust while improving the system?

A) Keep all categories enabled while adding few-shot examples to improve each category's accuracy over the coming weeks.
B) Keep all categories but display a confidence score with each finding, letting developers decide which to investigate.
C) Temporarily disable high false positive categories (style, naming, documentation) and run only high-precision categories while improving prompts.
D) Apply a uniform strictness reduction across all categories to bring the overall false positive rate to an acceptable level.

Correct Answer: C
Temporarily disabling the high false positive categories (style, naming, documentation) immediately stops trust erosion by removing the noise that causes developers to dismiss all findings, while preserving the value of high-precision categories like security and correctness. This approach allows time to improve prompts for the problematic categories before re-enabling them, rebuilding trust through demonstrated accuracy.

## Question 5

A pull request modifies 14 files across the stock tracking module. Your single-pass review analyzing all files together produces inconsistent results: detailed feedback for some files but superficial comments for others, obvious bugs missed, and contradictory feedback—flagging a pattern as problematic in one file while approving identical code elsewhere in the same PR. How should you restructure the review?

A) Switch to a higher-tier model with a larger context window to give all 14 files adequate attention in one pass.
B) Require developers to split large PRs into smaller submissions of 3-4 files before the automated review runs.
C) Split into focused passes: analyze each file individually for local issues, then run a separate integration-focused pass examining cross-file data flow.
D) Run three independent review passes on the full PR and only flag issues that appear in at least two of the three runs.

Correct Answer: C
Splitting the review into focused per-file passes directly addresses the root cause of attention dilution, ensuring consistent depth and catching local issues reliably. A separate integration-focused pass then handles cross-file concerns like data flow dependencies, covering both dimensions of review quality.

## Question 6

The code review component works iteratively: Claude analyzes a changed file, then may request related files (imports, base classes, tests) via tool calling to understand context before providing final feedback. Your application defines a tool that lets Claude request file contents; Claude invokes this tool, receives results, and continues its analysis. You're evaluating batch processing to reduce API costs. What is the primary technical constraint when considering batch processing for this workflow?

A) The batch API doesn't support tool definitions in request parameters.
B) Batch processing lacks request correlation identifiers for matching outputs to input requests.
C) Batch processing latency of up to 24 hours is too slow for pull request feedback, though the workflow could otherwise function.
D) The asynchronous model prevents executing tools mid-request and returning results for Claude to continue analysis.

Correct Answer: D
This is correct. The batch API's asynchronous fire-and-forget model means there is no mechanism to intercept a tool call mid-request, execute the tool, and return results for Claude to continue its analysis. This fundamentally breaks iterative tool-calling workflows that require multiple rounds of tool invocation and response within a single logical interaction.

## Question 7

Your automated review analyzes comments and docstrings. The current prompt instructs Claude to "check that comments are accurate and up-to-date." Findings frequently flag acceptable patterns (TODO markers, straightforward descriptions) while missing comments that describe behavior the code no longer implements. What change addresses the root cause of this inconsistent analysis?

A) Filter out TODO, FIXME, and descriptive comment patterns before analysis to reduce noise
B) Add few-shot examples of misleading comments to help the model recognize similar patterns in the codebase
C) Specify explicit criteria: flag comments only when their claimed behavior contradicts actual code behavior
D) Include git blame data so Claude can identify comments that predate recent code modifications

Correct Answer: C
Specifying explicit criteria—flag comments only when their claimed behavior contradicts actual code behavior—directly addresses the root cause by replacing the vague instruction with a precise definition of what constitutes a problem. This eliminates both false positives on acceptable patterns and false negatives on genuinely misleading comments.

## Question 8

Your CI/CD system performs three types of Claude-powered analysis: (1) quick style checks on each PR that block merging until complete, (2) comprehensive security audits of the entire codebase run weekly, and (3) test case generation triggered nightly for recently-modified modules. The Message Batches API offers 50% cost savings but can take up to 24 hours to process. You want to optimize API costs while maintaining acceptable developer experience. Which combination correctly matches each task to its API approach?

A) Use synchronous calls for PR style checks; use the Message Batches API for weekly security audits and nightly test generation.
B) Use synchronous calls for PR style checks and nightly test generation; use Message Batches API only for weekly security audits.
C) Use the Message Batches API for all three tasks to maximize the 50% cost savings, and configure the pipeline to poll for batch completion.
D) Use synchronous calls for all three tasks for consistent response times, and rely on prompt caching to reduce costs across all workloads.

Correct Answer: A
This is the correct approach. PR style checks block developers and require immediate responses via synchronous calls, while weekly security audits and nightly test generation are scheduled tasks with flexible timelines that can easily tolerate the up-to-24-hour batch processing window, capturing the 50% cost savings on both.

## Question 9

Your automated reviews identify valid issues but developers report the feedback isn't actionable. Findings say things like "complex ticket allocation logic" or "potential null pointer" without specifying what to change. When you add detailed instructions like "always include specific fix suggestions," the model still produces inconsistent output—sometimes detailed, sometimes vague. What prompting technique would most reliably produce consistently actionable feedback?

A) Expand the context window to include more of the surrounding codebase so the model has sufficient information to suggest specific fixes
B) Add 3-4 few-shot examples showing the exact format you want: issue identified, code location, specific fix suggestion
C) Implement a two-pass approach where one prompt identifies issues and a second prompt generates fixes, allowing specialization
D) Further refine the instructions with more explicit requirements for each part of the feedback format (location, issue, severity, suggested fix)

Correct Answer: B
Few-shot examples are the most effective technique for achieving consistent output format when instructions alone produce variable results. Providing 3-4 examples showing the exact desired format (issue, location, specific fix) gives the model a concrete pattern to follow, which is more reliable than abstract instructions.

## Question 10

Your CI pipeline includes two Claude-powered code review modes: a pre-merge-commit hook that blocks PR merging until complete, and "deep analysis" that runs overnight, polls for batch completion, then posts detailed suggestions to the PR. You want to reduce API costs using the Message Batches API, which offers 50% cost savings but requires polling and may take up to 24 hours to complete. Which mode should use batch processing?

A) Neither mode
B) Both modes
C) Deep analysis only
D) Pre-merge-commit hook only

Correct Answer: C
Deep analysis is the ideal candidate for batch processing because it already runs overnight, tolerates latency, and uses a polling model to check for completion before posting results—perfectly matching the Message Batches API's asynchronous, poll-based design while capturing the 50% cost savings.

## Question 11

Your team uses Claude Code to generate code suggestions, but you notice a pattern: subtle issues—performance optimizations that break edge cases, cleanups that change behavior unexpectedly—only surface when a different team member reviews the PR. Claude's reasoning during generation shows it considered these cases but concluded its approach was correct. Which approach directly addresses the root cause of this self-review limitation?

A) Have a second, independent Claude Code instance review the changes without seeing the generator''s reasoning.
B) Enable extended thinking mode for the generation pass, allowing more thorough deliberation before producing suggestions.
C) Include comprehensive test files and documentation in the prompt context so Claude better understands expected behavior during generation.
D) Add explicit self-review instructions to the generation prompt, asking Claude to critique its own suggestions before finalizing output.

Correct Answer: A
Using a second, independent Claude Code instance without access to the generator's reasoning directly addresses the root cause by eliminating confirmation bias. This fresh perspective mirrors the benefit of human peer review, where a different team member catches issues the original author rationalized away.

## Question 12

After an initial automated review generates 12 findings, a developer pushes new commits to address the issues. When the review runs again, it produces 8 findings—but developers report that 5 duplicate earlier comments on code that was already fixed in the new commits. What's the most effective way to eliminate this redundant feedback while maintaining thorough analysis?

A) Add a post-processing filter that removes findings matching previous file paths and issue descriptions before posting comments.
B) Run reviews only on initial PR creation and final pre-merge state, skipping intermediate commits.
C) Restrict the review scope to only files modified in the most recent push, excluding files from earlier commits.
D) Include prior review findings in context, instructing Claude to only report new or still-unaddressed issues.

Correct Answer: D
Including prior review findings in context allows Claude to intelligently distinguish between new issues and those already addressed by recent commits. This approach maintains thorough analysis while leveraging Claude's reasoning ability to avoid redundant feedback on fixed code.

## Question 13

Your automated review generates test case suggestions for each PR. When reviewing a PR that adds course completion tracking, Claude suggests 10 test cases but developer feedback indicates 6 duplicate scenarios already covered in the existing test suite. What change would most effectively reduce duplicate suggestions?

A) Include the existing test file in the context so Claude can identify what scenarios are already covered
B) Reduce requested suggestions from 10 to 5, assuming Claude will prioritize the most valuable cases first
C) Implement post-processing that filters suggestions whose descriptions match keywords from existing test names
D) Add instructions directing Claude to focus exclusively on edge cases and error conditions rather than successful paths

Correct Answer: A
Including the existing test file in the context directly addresses the root cause of duplication: Claude can only avoid suggesting already- covered scenarios if it knows what tests already exist. This gives Claude the information needed to reason about which suggestions would be genuinely new and valuable.

## Question 14

Your automated code review system shows inconsistent severity ratings—similar issues like null pointer risks receive "critical" severity in some PRs but only "medium" in others. Developer trust is declining because teams can't predict which findings require immediate attention. What's the most effective way to improve severity consistency?

A) Include explicit severity criteria in your prompt with concrete code examples for each severity level
B) Modify the prompt to ask Claude to rate severity relative to other issues in the same PR, so the most severe issue is always marked critical and others rated proportionally
C) Add a `CLAUDE.md` file that lists issue types and their default severities, instructing Claude to reference this mapping when assigning ratings
D) Request that Claude include its reasoning for each severity assignment, then use that reasoning to manually calibrate and adjust ratings during review

Correct Answer: A
Including explicit severity criteria with concrete code examples directly addresses the root cause of inconsistency by removing ambiguity about what each severity level means. This is a proven prompt engineering technique that gives the model clear reference points for classification, leading to more reliable and predictable severity assignments.

## Question 15

Your team wants to reduce API costs for automated analysis. Currently, real-time Claude calls power two workflows: (1) a blocking pre- merge check that must complete before developers can merge, and (2) a technical debt report generated overnight for review the next morning. Your manager proposes switching both to the Message Batches API for its 50% cost savings. How should you evaluate this proposal?

A) Use batch processing for the technical debt reports only; keep real-time calls for pre-merge checks.
B) Keep real-time calls for both workflows to avoid batch result ordering issues.
C) Switch both workflows to batch processing with status polling to check for completion.
D) Switch both to batch processing with a timeout fallback to real-time if batches take too long.

Correct Answer: A
This is the correct approach because the Message Batches API's up to 24-hour processing time with no guaranteed latency SLA makes it ideal for overnight technical debt reports but unsuitable for blocking pre-merge checks where developers are waiting. This matches each workflow to the appropriate API based on its latency requirements.
