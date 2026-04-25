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

Why A is incorrect:
The `--batch` flag is not a documented or supported option for Claude Code. Using this non-existent flag would likely result in an error or be ignored, failing to resolve the interactive input issue.

Why C is incorrect:
Redirecting stdin from /dev/null is a generic Unix workaround that does not properly address Claude Code's command syntax for non-interactive use. While it might prevent some input waiting, it is not the correct or reliable way to run Claude Code in a pipeline.

Why D is incorrect:
The `CLAUDE_HEADLESS=true` environment variable is not a documented or supported feature of Claude Code. This approach would not switch Claude Code into a non-interactive mode and the job would still hang.

Study Area: Claude Code for Continuous Integration — review CI/CD Integration concepts in the exam study guide.

## Question 2

Your automated code review averages 15 findings per pull request, with developers reporting a 40% false positive rate. The bottleneck is investigation time: developers must click into each finding to read Claude's reasoning before deciding whether to address or dismiss it. Your `CLAUDE.md` already contains comprehensive rules for acceptable patterns, and stakeholders have rejected any approach that filters findings before developer review. What change would best address the investigation time bottleneck?

A) Require Claude to include its reasoning and confidence assessment inline with each finding
B) Add a post-processor that analyzes finding patterns and automatically suppresses those matching historical false positive signatures
C) Categorize findings as "blocking issues" versus "suggestions" with tiered review requirements
D) Configure Claude to only surface findings it assesses as high confidence, filtering out uncertain flags before developers see them

Correct Answer: A
Including reasoning and confidence assessments inline with each finding directly addresses the investigation time bottleneck by allowing developers to quickly evaluate findings without clicking into each one separately. This approach respects the constraint against filtering, since all findings remain visible while making triage significantly faster.

Why B is incorrect:
Automatically suppressing findings that match historical false positive signatures is another form of filtering before developer review, which stakeholders have explicitly rejected. Even though it uses data-driven pattern matching, it still removes findings from the developer's view.

Why C is incorrect:
Categorizing findings into tiers reorganizes the review workflow but does not reduce the core investigation time problem, as developers still need to click into each finding to understand Claude's reasoning before deciding to address or dismiss it. This approach adds structure without addressing the root cause of the bottleneck.

Why D is incorrect:
Filtering out low-confidence findings has Claude directly violate the stakeholder constraint that no findings should be filtered before developer review. This approach would reduce the number of findings but is explicitly disallowed by the stated requirements.

Study Area: Claude Code for Continuous Integration — review False Positive Reduction concepts in the exam study guide.

## Question 3

Your CI pipeline runs the Claude Code CLI (with `--print` mode) using `CLAUDE.md` to provide project context for code reviews, and developers generally find the reviews insightful. However, they report that integrating findings into your workflow is difficult—Claude produces narrative paragraphs that must be manually copied into PR comments. Your team wants to automatically post each finding as a separate inline PR comment at the relevant code location, which requires structured data with file path, line number, severity, and suggested fix. What's the most effective approach?

A) Add a "Review Output Format" section to `CLAUDE.md` with examples showing structured findings, so Claude learns the expected format from project context.
B) Keep the narrative review format but add a summarization step that uses Claude to generate a structured JSON summary of the findings.
C) Use CLI flags `--output-format json` and `--json-schema` to enforce structured findings, then parse output to post inline comments via the GitHub API.
D) Include explicit formatting instructions in your review prompt requiring each finding to follow a parseable template like [FILE:path] [LINE:n] [SEVERITY: level] ....

Correct Answer: C
Using --output-format json" with --json-schema' enforces structured output at the CLI level, guaranteeing well-formed JSON with the required fields (file path, line number, severity, suggested fix) that can be reliably parsed and posted as inline PR comments via the GitHub API. This is the most effective approach because it leverages native CLI capabilities designed specifically for structured output enforcement.

Why A is incorrect:
Adding format examples to CLAUDE.md relies on prompt-based formatting guidance, which LLMs follow inconsistently. This approach is unreliable for automated pipelines that require specific structured fields like file path, line number, and severity to be present and parseable every time.

Why B is incorrect:
Adding a second LLM call to summarize narrative output into structured JSON introduces unnecessary complexity, additional cost, and another potential point of failure. The problem can be solved more directly and reliably by enforcing structured output in the original CLI invocation using built-in flags.

Why D is incorrect:
Including explicit formatting instructions in the prompt to produce a parseable template is a prompt-engineering approach that LLMs may follow inconsistently, sometimes deviating from the expected format. For automated pipelines requiring reliable parsing to post inline comments, this approach is fragile compared to using built-in CLI flags that enforce structured output.

Study Area: Claude Code for Continuous Integration — review Structured Output concepts in the exam study guide.

## Question 4

Analysis of your automated code review shows significant variation in false positive rates across finding categories. Security and correctness findings have an 8% false positive rate, performance findings have 18%, style and naming findings have 52%, and documentation findings have 48%. Developer surveys indicate growing distrust—many have started dismissing findings without review because "half are wrong." The high false positive categories are undermining confidence in the accurate categories. What approach best restores developer trust while improving the system?

A) Keep all categories enabled while adding few-shot examples to improve each category's accuracy over the coming weeks.
B) Keep all categories but display a confidence score with each finding, letting developers decide which to investigate.
C) Temporarily disable high false positive categories (style, naming, documentation) and run only high-precision categories while improving prompts.
D) Apply a uniform strictness reduction across all categories to bring the overall false positive rate to an acceptable level.

Correct Answer: C
Temporarily disabling the high false positive categories (style, naming, documentation) immediately stops trust erosion by removing the noise that causes developers to dismiss all findings, while preserving the value of high-precision categories like security and correctness. This approach allows time to improve prompts for the problematic categories before re-enabling them, rebuilding trust through demonstrated accuracy.

Why A is incorrect:
Improving prompts with few-shot examples addresses the root cause, but keeping categories enabled during the weeks-long improvement process allows trust to continue eroding as developers keep encountering the ~50% false positive rates that have already caused them to dismiss findings without review.

Why B is incorrect:
Displaying confidence scores shifts the evaluation burden onto developers without actually reducing noise, and developers who have already lost trust in the system are unlikely to trust its self-reported confidence scores either.

Why D is incorrect:
Applying uniform strictness reduction across all categories would degrade the performance of already-accurate categories (like security and correctness at 8% false positive rate) to compensate for others, sacrificing real value without effectively solving the trust problem in any category.

Study Area: Claude Code for Continuous Integration — review False Positive Reduction concepts in the exam study guide.

## Question 5

A pull request modifies 14 files across the stock tracking module. Your single-pass review analyzing all files together produces inconsistent results: detailed feedback for some files but superficial comments for others, obvious bugs missed, and contradictory feedback—flagging a pattern as problematic in one file while approving identical code elsewhere in the same PR. How should you restructure the review?

A) Switch to a higher-tier model with a larger context window to give all 14 files adequate attention in one pass.
B) Require developers to split large PRs into smaller submissions of 3-4 files before the automated review runs.
C) Split into focused passes: analyze each file individually for local issues, then run a separate integration-focused pass examining cross-file data flow.
D) Run three independent review passes on the full PR and only flag issues that appear in at least two of the three runs.

Correct Answer: C
Splitting the review into focused per-file passes directly addresses the root cause of attention dilution, ensuring consistent depth and catching local issues reliably. A separate integration-focused pass then handles cross-file concerns like data flow dependencies, covering both dimensions of review quality.

Why A is incorrect:
Simply using a larger context window does not solve the core problem of attention quality degradation; the "lost in the middle" phenomenon means models still struggle to give uniform attention across large inputs. The inconsistencies and contradictions observed are symptoms of attention dilution, not insufficient context capacity.

Why B is incorrect:
Requiring developers to break up PRs shifts the burden to the development workflow without actually improving the review system's analytical approach. This is a process workaround rather than a technical solution, and logically related changes across files may still need to be reviewed together for proper review quality.

Why D is incorrect:
Running multiple independent review passes does not address the root cause of attention dilution; each pass would still face the same problem of producing inconsistent quality across files. Filtering for findings that appear in at least two runs may reduce false positives but also discards valid findings that only one pass identified, while doing nothing to address missed bugs or contradictory feedback within a single pass.

Study Area: Claude Code for Continuous Integration — review Task Decomposition concepts in the exam study guide.

## Question 6

The code review component works iteratively: Claude analyzes a changed file, then may request related files (imports, base classes, tests) via tool calling to understand context before providing final feedback. Your application defines a tool that lets Claude request file contents; Claude invokes this tool, receives results, and continues its analysis. You're evaluating batch processing to reduce API costs. What is the primary technical constraint when considering batch processing for this workflow?

A) The batch API doesn't support tool definitions in request parameters.
B) Batch processing lacks request correlation identifiers for matching outputs to input requests.
C) Batch processing latency of up to 24 hours is too slow for pull request feedback, though the workflow could otherwise function.
D) The asynchronous model prevents executing tools mid-request and returning results for Claude to continue analysis.

Correct Answer: D
This is correct. The batch API's asynchronous fire-and-forget model means there is no mechanism to intercept a tool call mid-request, execute the tool, and return results for Claude to continue its analysis. This fundamentally breaks iterative tool-calling workflows that require multiple rounds of tool invocation and response within a single logical interaction.

Why A is incorrect:
The batch API fully supports tool definitions in request parameters and can generate tool use responses. This is not a constraint of batch processing.

Why B is incorrect:
Batch processing provides a custom_id field for each request, which serves as a correlation identifier for matching outputs back to their corresponding inputs. This is not a limitation of the batch API.

Why C is incorrect:
While the up-to-24-hour latency is a real practical concern, the claim that the workflow could otherwise function is incorrect. The fundamental architectural limitation is that batch processing cannot support the iterative tool-calling loop this workflow requires, regardless of how fast results are returned.

Study Area: Claude Code for Continuous Integration — review Batch Processing concepts in the exam study guide.

## Question 7

Your automated review analyzes comments and docstrings. The current prompt instructs Claude to "check that comments are accurate and up-to-date." Findings frequently flag acceptable patterns (TODO markers, straightforward descriptions) while missing comments that describe behavior the code no longer implements. What change addresses the root cause of this inconsistent analysis?

A) Filter out TODO, FIXME, and descriptive comment patterns before analysis to reduce noise
B) Add few-shot examples of misleading comments to help the model recognize similar patterns in the codebase
C) Specify explicit criteria: flag comments only when their claimed behavior contradicts actual code behavior
D) Include git blame data so Claude can identify comments that predate recent code modifications

Correct Answer: C
Specifying explicit criteria—flag comments only when their claimed behavior contradicts actual code behavior—directly addresses the root cause by replacing the vague instruction with a precise definition of what constitutes a problem. This eliminates both false positives on acceptable patterns and false negatives on genuinely misleading comments.

Why A is incorrect:
Filtering out TODO, FIXME, and descriptive comment patterns before analysis only addresses one symptom (false positives) while completely ignoring the false negative problem of missing comments that describe behavior the code no longer implements.

Why B is incorrect:
While few-shot examples of misleading comments can help the model recognize similar patterns, this approach won't generalize well to novel types of contradictions because it relies on pattern-matching rather than defining the underlying criterion the model should apply.

Why D is incorrect:
Including git blame data provides temporal context about when comments were written, but comment age alone doesn't determine accuracy—old comments can still be correct and new comments can be wrong—so this doesn't define what should actually be flagged.

Study Area: Claude Code for Continuous Integration — review Prompt Specificity concepts in the exam study guide.

## Question 8

Your CI/CD system performs three types of Claude-powered analysis: (1) quick style checks on each PR that block merging until complete, (2) comprehensive security audits of the entire codebase run weekly, and (3) test case generation triggered nightly for recently-modified modules. The Message Batches API offers 50% cost savings but can take up to 24 hours to process. You want to optimize API costs while maintaining acceptable developer experience. Which combination correctly matches each task to its API approach?

A) Use synchronous calls for PR style checks; use the Message Batches API for weekly security audits and nightly test generation.
B) Use synchronous calls for PR style checks and nightly test generation; use Message Batches API only for weekly security audits.
C) Use the Message Batches API for all three tasks to maximize the 50% cost savings, and configure the pipeline to poll for batch completion.
D) Use synchronous calls for all three tasks for consistent response times, and rely on prompt caching to reduce costs across all workloads.

Correct Answer: A
This is the correct approach. PR style checks block developers and require immediate responses via synchronous calls, while weekly security audits and nightly test generation are scheduled tasks with flexible timelines that can easily tolerate the up-to-24-hour batch processing window, capturing the 50% cost savings on both.

Why B is incorrect:
While correctly using synchronous calls for latency-sensitive PR checks, this approach misses the 50% cost savings on nightly test generation, which runs on a scheduled basis and can easily tolerate batch processing times. There is no reason to pay full price for a task that doesn't need immediate results.

Why C is incorrect:
Using the Message Batches API for all three tasks would create unacceptable delays for PR style checks, which block merging and require immediate feedback for developers. While this maximizes cost savings, the up-to-24-hour processing time makes it unsuitable for latency-sensitive, developer-blocking workflows.

Why D is incorrect:
Using synchronous calls for all tasks foregoes the significant 50% cost savings available from the Message Batches API for scheduled, non-latency-sensitive workloads. Prompt caching, while useful, does not provide an equivalent level of cost reduction and is not a substitute for batch processing savings on appropriate tasks.

Study Area: Claude Code for Continuous Integration — review Batch Processing concepts in the exam study guide.

## Question 9

Your automated reviews identify valid issues but developers report the feedback isn't actionable. Findings say things like "complex ticket allocation logic" or "potential null pointer" without specifying what to change. When you add detailed instructions like "always include specific fix suggestions," the model still produces inconsistent output—sometimes detailed, sometimes vague. What prompting technique would most reliably produce consistently actionable feedback?

A) Expand the context window to include more of the surrounding codebase so the model has sufficient information to suggest specific fixes
B) Add 3-4 few-shot examples showing the exact format you want: issue identified, code location, specific fix suggestion
C) Implement a two-pass approach where one prompt identifies issues and a second prompt generates fixes, allowing specialization
D) Further refine the instructions with more explicit requirements for each part of the feedback format (location, issue, severity, suggested fix)

Correct Answer: B
Few-shot examples are the most effective technique for achieving consistent output format when instructions alone produce variable results. Providing 3-4 examples showing the exact desired format (issue, location, specific fix) gives the model a concrete pattern to follow, which is more reliable than abstract instructions.

Why A is incorrect:
Expanding the context window to include more of the surrounding codebase—the model already produces detailed, actionable feedback with the current context, indicating that insufficient information is not the root cause. The real issue is format inconsistency, which more context does not resolve.

Why C is incorrect:
A two-pass approach adds architectural complexity without directly addressing the core problem of inconsistent output formatting. While specialization can help in some cases, it doesn't solve why the model sometimes produces vague feedback instead of actionable suggestions.

Why D is incorrect:
The scenario already describes adding detailed instructions that failed to produce consistent output, so further refining instructions repeats the same failing approach. More explicit requirements are still abstract instructions, which models follow less reliably than concrete examples.

Study Area: Claude Code for Continuous Integration — review Few-Shot Prompting concepts in the exam study guide.

## Question 10

Your CI pipeline includes two Claude-powered code review modes: a pre-merge-commit hook that blocks PR merging until complete, and "deep analysis" that runs overnight, polls for batch completion, then posts detailed suggestions to the PR. You want to reduce API costs using the Message Batches API, which offers 50% cost savings but requires polling and may take up to 24 hours to complete. Which mode should use batch processing?

A) Neither mode
B) Both modes
C) Deep analysis only
D) Pre-merge-commit hook only

Correct Answer: C
Deep analysis is the ideal candidate for batch processing because it already runs overnight, tolerates latency, and uses a polling model to check for completion before posting results—perfectly matching the Message Batches API's asynchronous, poll-based design while capturing the 50% cost savings.

Why A is incorrect:
Avoiding batch processing entirely wastes the opportunity to save 50% on API costs for the deep analysis workload, which is inherently latency-tolerant and already designed around an asynchronous polling pattern that aligns perfectly with the Message Batches API.

Why B is incorrect:
Applying batch processing to both modes would break the pre-merge-commit hook workflow, since it requires near-immediate results to unblock PR merging, and the Message Batches API cannot guarantee timely completion. While deep analysis benefits from batching, the blocking nature of pre-merge hooks makes them incompatible.

Why D is incorrect:
Using batch processing for the pre-merge-commit hook is inappropriate because it blocks PR merging and requires timely completion, which is incompatible with the Message Batches API's potential latency of up to 24 hours and lack of timing guarantees. This would cause unacceptable delays in the development workflow.

Study Area: Claude Code for Continuous Integration — review Batch Processing concepts in the exam study guide.

## Question 11

Your team uses Claude Code to generate code suggestions, but you notice a pattern: subtle issues—performance optimizations that break edge cases, cleanups that change behavior unexpectedly—only surface when a different team member reviews the PR. Claude's reasoning during generation shows it considered these cases but concluded its approach was correct. Which approach directly addresses the root cause of this self-review limitation?

A) Have a second, independent Claude Code instance review the changes without seeing the generator''s reasoning.
B) Enable extended thinking mode for the generation pass, allowing more thorough deliberation before producing suggestions.
C) Include comprehensive test files and documentation in the prompt context so Claude better understands expected behavior during generation.
D) Add explicit self-review instructions to the generation prompt, asking Claude to critique its own suggestions before finalizing output.

Correct Answer: A
Using a second, independent Claude Code instance without access to the generator's reasoning directly addresses the root cause by eliminating confirmation bias. This fresh perspective mirrors the benefit of human peer review, where a different team member catches issues the original author rationalized away.

Why B is incorrect:
Enabling extended thinking mode gives Claude more deliberation time during generation, but the question states Claude already considered the edge cases and concluded its approach was correct. More thinking time does not resolve the fundamental self-review limitation where the model has already rationalized its decisions.

Why C is incorrect:
Providing more context such as test files and documentation improves the quality of generation input, but the question specifies Claude's reasoning already considered these cases and still concluded its approach was correct. This means the issue is not a lack of information but rather a self-review blind spot that additional context cannot fix.

Why D is incorrect:
Asking Claude to critique its own suggestions within the same context does not address the root cause, because the same confirmation bias that led it to conclude its approach was correct will persist during self-review. The question explicitly states Claude already considered these cases and rationalized its decisions, so additional self-critique in the same context will likely reach the same conclusions.

Study Area: Claude Code for Continuous Integration — review Multi-Agent Verification concepts in the exam study guide.

## Question 12

After an initial automated review generates 12 findings, a developer pushes new commits to address the issues. When the review runs again, it produces 8 findings—but developers report that 5 duplicate earlier comments on code that was already fixed in the new commits. What's the most effective way to eliminate this redundant feedback while maintaining thorough analysis?

A) Add a post-processing filter that removes findings matching previous file paths and issue descriptions before posting comments.
B) Run reviews only on initial PR creation and final pre-merge state, skipping intermediate commits.
C) Restrict the review scope to only files modified in the most recent push, excluding files from earlier commits.
D) Include prior review findings in context, instructing Claude to only report new or still-unaddressed issues.

Correct Answer: D
Including prior review findings in context allows Claude to intelligently distinguish between new issues and those already addressed by recent commits. This approach maintains thorough analysis while leveraging Claude's reasoning ability to avoid redundant feedback on fixed code.

Why A is incorrect:
A post-processing filter based on matching file paths and issue descriptions is brittle because LLM-generated comments vary in wording across runs, making exact matching unreliable. It also cannot semantically determine whether an issue has actually been resolved or merely rephrased differently.

Why B is incorrect:
Running reviews only at PR creation and final pre-merge state eliminates the continuous feedback loop that makes CI/CD-integrated reviews valuable. This sidesteps the duplication problem rather than solving it, and developers lose the benefit of iterative review feedback during development.

Why C is incorrect:
Restricting the review scope to only recently modified files sacrifices thoroughness, since changes in one file can introduce or affect issues in other files that weren't directly modified. This approach trades analysis quality for deduplication, which is not an ideal tradeoff.

Study Area: Claude Code for Continuous Integration — review CI/CD Integration concepts in the exam study guide.

## Question 13

Your automated review generates test case suggestions for each PR. When reviewing a PR that adds course completion tracking, Claude suggests 10 test cases but developer feedback indicates 6 duplicate scenarios already covered in the existing test suite. What change would most effectively reduce duplicate suggestions?

A) Include the existing test file in the context so Claude can identify what scenarios are already covered
B) Reduce requested suggestions from 10 to 5, assuming Claude will prioritize the most valuable cases first
C) Implement post-processing that filters suggestions whose descriptions match keywords from existing test names
D) Add instructions directing Claude to focus exclusively on edge cases and error conditions rather than successful paths

Correct Answer: A
Including the existing test file in the context directly addresses the root cause of duplication: Claude can only avoid suggesting already- covered scenarios if it knows what tests already exist. This gives Claude the information needed to reason about which suggestions would be genuinely new and valuable.

Why B is incorrect:
Simply reducing the number of requested suggestions does not give Claude any information about which scenarios are already covered, so it has no basis to prioritize non-duplicate cases. This approach incorrectly assumes that fewer suggestions will naturally be more unique, when in reality the most obvious suggestions are likely the ones already in the test suite.

Why C is incorrect:
Using keyword matching to filter suggestions is a brittle approach that would miss semantically equivalent tests described with different wording or terminology. This post-processing workaround addresses symptoms rather than the root cause and would be unreliable in practice.

Why D is incorrect:
Restricting suggestions to only edge cases and error conditions does not solve the duplication problem, since these may already cover those edge cases and error conditions. Without knowledge of the existing test suite, Claude would still have no way to avoid suggesting scenarios that are already tested.

Study Area: Claude Code for Continuous Integration — review Content Provision Methods concepts in the exam study guide.

## Question 14

Your automated code review system shows inconsistent severity ratings—similar issues like null pointer risks receive "critical" severity in some PRs but only "medium" in others. Developer trust is declining because teams can't predict which findings require immediate attention. What's the most effective way to improve severity consistency?

A) Include explicit severity criteria in your prompt with concrete code examples for each severity level
B) Modify the prompt to ask Claude to rate severity relative to other issues in the same PR, so the most severe issue is always marked critical and others rated proportionally
C) Add a `CLAUDE.md` file that lists issue types and their default severities, instructing Claude to reference this mapping when assigning ratings
D) Request that Claude include its reasoning for each severity assignment, then use that reasoning to manually calibrate and adjust ratings during review

Correct Answer: A
Including explicit severity criteria with concrete code examples directly addresses the root cause of inconsistency by removing ambiguity about what each severity level means. This is a proven prompt engineering technique that gives the model clear reference points for classification, leading to more reliable and predictable severity assignments.

Why B is incorrect:
Rating severity relative to other issues within the same PR means that a single minor issue would be marked "critical" if it's the only finding, and truly critical issues could be downgraded if multiple severe issues exist. This relative approach introduces even more inconsistency rather than establishing a stable, absolute severity standard.

Why C is incorrect:
A static issue-type-to-severity mapping loses important context, since the same issue type (e.g., a null pointer risk) may warrant different severities depending on factors like code path, exposure, or criticality of the affected component. This rigid approach oversimplifies severity assignment and can lead to inaccurate ratings.

Why D is incorrect:
Requesting reasoning and then manually calibrating ratings shifts the burden to human reviewers, which defeats the purpose of automated code review and does not scale. While transparency in reasoning is valuable, it does not directly solve the consistency problem in the automated system itself.

Study Area: Claude Code for Continuous Integration — review Classification Consistency concepts in the exam study guide.

## Question 15

Your team wants to reduce API costs for automated analysis. Currently, real-time Claude calls power two workflows: (1) a blocking pre- merge check that must complete before developers can merge, and (2) a technical debt report generated overnight for review the next morning. Your manager proposes switching both to the Message Batches API for its 50% cost savings. How should you evaluate this proposal?

A) Use batch processing for the technical debt reports only; keep real-time calls for pre-merge checks.
B) Keep real-time calls for both workflows to avoid batch result ordering issues.
C) Switch both workflows to batch processing with status polling to check for completion.
D) Switch both to batch processing with a timeout fallback to real-time if batches take too long.

Correct Answer: A
This is the correct approach because the Message Batches API's up to 24-hour processing time with no guaranteed latency SLA makes it ideal for overnight technical debt reports but unsuitable for blocking pre-merge checks where developers are waiting. This matches each workflow to the appropriate API based on its latency requirements.

Why B is incorrect:
Keeping real-time calls for both workflows unnecessarily forgoes the 50% cost savings on the overnight technical debt reports, which have no latency constraints. The concern about batch result ordering is a misconception, as batch results can be correlated to their requests using `custom_id` fields.

Why C is incorrect:
Switching both workflows to batch processing is problematic because the Message Batches API has no guaranteed latency SLA and can take up to 24 hours, making it unsuitable for blocking pre-merge checks where developers must wait for results before merging. Status polling does not solve the fundamental issue of unpredictable completion times for a latency-sensitive workflow.

Why D is incorrect:
Adding a timeout fallback introduces unnecessary architectural complexity when the simpler and more reliable solution is to use each API for its appropriate use case—real-time for latency-sensitive pre-merge checks and batch for overnight reports. This hybrid approach also still risks delays and inconsistent behavior for the pre-merge workflow.

Study Area: Claude Code for Continuous Integration — review Batch Processing concepts in the exam study guide.
