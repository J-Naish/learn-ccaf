# Multi-Agent Research System

You are building a **multi-agent research system** using the Claude Agent SDK. A coordinator agent delegates to specialized subagents: one searches the web, one analyzes documents, one synthesizes findings, and one generates reports. The system researches topics and produces comprehensive, cited reports.

## Question 1

The document analysis subagent encounters a corrupted PDF file it cannot parse. When designing the system's error handling, what is the most effective way to handle this failure?

A) Silently skip the corrupted document and continue processing other files to avoid interrupting the workflow.
B) Return the error with context to the coordinator agent, letting it decide how to proceed.
C) Throw an exception that terminates the entire research workflow.
D) Automatically retry parsing the document three times with exponential backoff before reporting failure.

Correct Answer: B
Returning the error with context to the coordinator agent is the most effective approach because it enables the coordinator to make an informed decision—such as skipping the file, trying an alternative parsing method, or notifying the user—while maintaining visibility into the failure.

## Question 2

The web search subagent returns results for only 3 of 5 requested source categories (competitor websites and industry reports succeeded, but news archives and social media feeds timed out). The document analysis subagent successfully processed all provided documents. The synthesis subagent must now produce a findings summary from this mixed-quality input. What's the most effective error propagation strategy?

A) Have the synthesis subagent return an error to the coordinator indicating incomplete upstream data, triggering a full retry or task failure.
B) Have the synthesis subagent request the coordinator retry the timed-out sources with extended timeouts before proceeding, ensuring complete data coverage before synthesis begins.
C) Structure the synthesis output with coverage annotations indicating which findings are well-supported versus which topic areas have gaps due to unavailable sources.
D) Proceed with synthesis using only the successful sources, generating output without indicating which data was unavailable.

Correct Answer: C
Structuring the synthesis output with coverage annotations embodies graceful degradation with transparency, allowing downstream consumers and end users to understand which findings are well-supported and which topic areas have gaps. This approach preserves the value of completed work while propagating uncertainty information so informed decisions can be made about confidence levels.

## Question 3

The document analysis agent discovers that two credible sources contain directly conflicting statistics on a key metric: one government report states 40% growth while an industry analysis states 12% growth. Both sources appear legitimate and the discrepancy could significantly affect the research conclusions. What's the most effective way for the document analysis agent to handle this?

A) Complete the document analysis with both figures included, explicitly annotate the conflict with source attribution, and let the coordinator decide how to reconcile before passing to synthesis.
B) Include both figures in the analysis output without flagging them as conflicting, allowing the synthesis agent to determine which to use based on the broader research context.
C) Halt analysis and escalate to the coordinator immediately, asking it to determine which source is authoritative before the agent continues processing remaining documents.
D) Apply source credibility heuristics to select the most likely accurate figure, complete the analysis using that value, and include a footnote mentioning the discrepancy.

Correct Answer: A
This is the most effective approach because it respects separation of concerns: the document analysis agent completes its primary task without blocking, preserves both conflicting data points with explicit source attribution, and appropriately defers the reconciliation decision to the coordinator, which has the broader context needed to resolve the conflict.

## Question 4

During testing, you observe that the synthesis agent frequently needs to verify specific claims while combining findings. Currently, when verification is needed, the synthesis agent returns control to the coordinator, which invokes the web search agent, then re-invokes synthesis with results. This adds 2-3 round trips per task and increases latency by 40%. Your evaluation shows that 85% of these verifications are simple fact-checks (dates, names, statistics) while 15% require deeper investigation. What's the most effective approach to reduce overhead while maintaining system reliability?

A) Have the web search agent proactively cache extra context around each source during initial research, anticipating what the synthesis agent might need to verify.
B) Give the synthesis agent a scoped `verify_fact` tool for simple lookups, while complex verifications continue delegating to the web search agent through the coordinator.
C) Have the synthesis agent accumulate all verification needs and return them as a batch to the coordinator at the end of its pass, which then sends them all to the web search agent at once.
D) Give the synthesis agent access to all web search tools so it can handle any verification need directly without round-trips through the coordinator.

Correct Answer: B
Providing a scoped fact-verification tool handles the 85% of simple lookups directly, eliminating most round-trips while preserving the coordinator-based delegation path for the 15% of complex verifications. This applies the principle of least privilege, keeping the synthesis agent focused on its primary task while still reducing latency significantly.

## Question 5

A colleague suggests having the document analysis agent send its output directly to the synthesis agent instead of routing through the coordinator. What is the main advantage of keeping the coordinator as the central hub for all subagent communication?

A) Routing through the coordinator enables automatic retry logic that direct agent-to-agent calls cannot support
B) The coordinator batches multiple subagent requests together, reducing the total number of API calls and overall latency
C) The coordinator can observe all interactions, handle errors consistently, and decide what information each subagent should receive
D) Subagents operate with isolated memory, and direct communication would require complex serialization that only the coordinator can perform

Correct Answer: C
This is correct. The coordinator pattern provides centralized visibility into all interactions, consistent error handling across the system, and fine-grained control over what information each subagent receives, which are the primary advantages of hub-and-spoke communication.

## Question 6

Production monitoring reveals inconsistent synthesis quality. When aggregated results total ~75K tokens, the synthesis agent reliably cites information from the first 15K tokens (web search headlines and snippets) and the final 10K tokens (document analysis conclusions), but frequently omits critical findings that appear in the middle 50K tokens—even when those findings directly address the research question. How should you restructure the aggregated input?

A) Stream subagent results to the synthesis agent incrementally, processing web search results first to completion before introducing document analysis findings.
B) Implement rotation that alternates which subagent's results appear first across different research tasks, ensuring both sources receive primacy positioning equally over time.
C) Place a key findings summary at the beginning of the aggregated input and organize detailed results with explicit section headers for easier navigation.
D) Summarize all subagent outputs to under 20K tokens total before aggregation, ensuring content stays within the model's reliable processing range.

Correct Answer: C
Placing a key findings summary at the beginning leverages the primacy effect, ensuring critical information occupies the most reliably attended position. Adding explicit section headers throughout the aggregated input helps the model navigate and attend to middle-section content, directly mitigating the 'lost in the middle' phenomenon.

## Question 7

The web search and document analysis agents have both completed their tasks and returned findings to the coordinator. What is the appropriate next step for producing an integrated research output?

A) The document analysis agent requests the web search results and merges them internally
B) The coordinator passes both sets of findings to the synthesis agent for unified integration
C) The coordinator concatenates the raw outputs from both agents and returns them as the final result
D) Each agent directly sends its findings to the report generation agent, bypassing the coordinator

Correct Answer: B
This is correct because the orchestrator-workers pattern requires the coordinator to maintain centralized control by collecting results from subagents and routing them to the appropriate next component—in this case, the synthesis agent, which is specifically designed to unify and integrate findings into a coherent output.

## Question 8

During a materials research task, the web search subagent queries three source categories with different outcomes: academic databases returned 15 relevant papers, industry reports returned "0 results found," and patent databases returned "Connection timeout." When designing error propagation to the coordinator, what approach enables the best recovery decisions?

A) Aggregate outcomes into a single success rate metric (e.g., "67% source coverage") with detailed logs available on request.
B) Have the subagent retry transient failures internally and only report persistent errors.
C) Distinguish access failures (timeout) needing retry decisions from valid empty results ("0 results") representing successful queries.
D) Report both the timeout and "0 results" as failures requiring coordinator intervention.

Correct Answer: C
This is correct because a timeout (access failure) and '0 results' (valid empty result) are semantically distinct outcomes requiring different responses. Distinguishing them enables the coordinator to retry the timed-out patent database while accepting the empty industry report results as a valid and informative finding.

## Question 9

During testing, combined outputs from the web search agent (85K tokens including page content) and the document analysis agent (70K tokens including reasoning chains) total 155K tokens, but the synthesis agent performs optimally with inputs under 50K tokens. What's the most effective solution?

A) Store findings in a vector database and give the synthesis agent retrieval tools to query during its work
B) Have the synthesis agent process findings in sequential batches, maintaining running state between calls
C) Modify upstream agents to return structured data (key facts, citations, relevance scores) instead of verbose content and reasoning
D) Add an intermediate summarization agent that condenses findings before passing to synthesis

Correct Answer: C
Modifying upstream agents to return structured data (key facts, citations, relevance scores) addresses the root cause by reducing token volume at the source while preserving essential information. This eliminates verbose page content and reasoning chains that inflate token counts without adding value for the synthesis step.

## Question 10

When designing the system, you gave the document analysis agent access to a general-purpose `fetch_url` tool so it could load documents from URLs. Production logs reveal this agent now frequently fetches search engine result pages to conduct ad-hoc web searches—behavior that should route through the web search agent. This causes inconsistent results. What's the most effective fix?

A) Remove `fetch_url` from the document analysis agent and route all URL loading through the coordinator to the web search agent.
B) Implement filtering that blocks `fetch_url` calls to known search engine domains while allowing other URLs.
C) Replace `fetch_url` with a `load_document` tool that validates URLs point to document formats.
D) Add instructions to the document analysis agent's prompt clarifying it should only use `fetch_url` for loading document URLs, not searching.

Correct Answer: C
Replacing the general-purpose tool with a document-specific tool that validates URLs point to document formats addresses the root cause by constraining capability at the interface level. This follows the principle of least privilege, making the undesired search behavior impossible rather than merely discouraged.

## Question 11

The web search subagent times out while researching a complex topic. You need to design how this failure information flows back to the coordinator agent. Which error propagation approach best enables intelligent recovery?

A) Catch the timeout within the subagent and return an empty result set marked as successful.
B) Propagate the timeout exception directly to a top-level handler that terminates the entire research workflow.
C) Implement automatic retry logic with exponential backoff within the subagent, returning a generic "search unavailable" status only after all retries are exhausted.
D) Return structured error context to the coordinator including the failure type, the attempted query, any partial results, and potential alternative approaches.

Correct Answer: D
Returning structured error context—including the failure type, attempted query, partial results, and alternative approaches—gives the coordinator all the information it needs to make intelligent recovery decisions, such as retrying with a modified query or proceeding with partial results. This is the best approach because it preserves maximum context for informed decision-making at the coordination level.

## Question 12

When researching a broad topic, you observe that the web search agent and document analysis agent are both investigating the same subtopics, resulting in significant overlap in their findings. Token usage has nearly doubled without proportionally increasing the breadth or depth of research coverage. What's the most effective way to address this?

A) Implement a shared state mechanism where agents log their current focus area, allowing other agents to dynamically avoid duplicating work in progress
B) Have the coordinator explicitly partition the research space before delegation, assigning distinct subtopics or source types to each agent
C) Allow both agents to complete their parallel work, then have the coordinator deduplicate overlapping findings before passing to the synthesis agent
D) Convert to sequential execution where document analysis runs only after web search completes, using the web search findings as context to avoid duplication

Correct Answer: B
Having the coordinator explicitly partition the research space before delegation is the most effective approach because it addresses the root cause—unclear task boundaries—before any work begins. This preserves the benefits of parallel execution while preventing duplicated effort and wasted tokens.

## Question 13

Production logs reveal a consistent pattern: requests to "analyze the quarterly report I uploaded" are routed to the web search agent 45% of the time instead of the document analysis agent. Examining the tool definitions, you find the web search agent has an analyze_content tool described as "analyzes content and extracts key information," while the document analysis agent has an analyze_document tool described as "analyzes documents and extracts key information." How should you address this misrouting?

A) Expand the document analysis tool's description to include example use cases like "Use for uploaded PDFs, Word documents, and spreadsheets" while leaving the web search tool unchanged.
B) Rename the web search tool to `extract_web_results` and update its description to "processes and returns information retrieved from web searches and URLs."
C) Add a pre-routing classifier that determines whether the user is referencing uploaded files or web content before the coordinator makes delegation decisions.
D) Add few-shot examples to the coordinator's prompt showing correct routing: "User uploads quarterly report > document analysis agent" and "User asks about a webpage > web search agent."

Correct Answer: B
Renaming the web search tool to 'extract_web_results' and updating its description to clearly reference web searches and URLs directly addresses the root cause by eliminating the semantic overlap between the two tools' names and descriptions. This makes each tool's purpose unambiguous, allowing the coordinator to correctly distinguish between document analysis and web search tasks.

## Question 14

After running the system on the topic "impact of AI on creative industries," you observe that each subagent completes successfully: the web search agent finds relevant articles, the document analysis agent summarizes papers correctly, and the synthesis agent produces coherent output. However, the final reports cover only visual arts, completely missing music, writing, and film production. When you examine the coordinator's logs, you see it decomposed the topic into three subtasks: "AI in digital art creation," "AI in graphic design," and "AI in photography." What is the most likely root cause?

A) The synthesis agent lacks instructions for identifying coverage gaps in the findings it receives from other agents.
B) The document analysis agent is filtering out sources related to non-visual creative industries due to overly restrictive relevance criteria.
C) The web search agent's queries are not comprehensive enough and need to be expanded to cover more creative industry sectors.
D) The coordinator agent's task decomposition is too narrow, resulting in subagent assignments that don't cover all relevant domains of the topic.

Correct Answer: D
The coordinator's logs directly reveal it decomposed the broad topic into only three visual arts subtasks (digital art, graphic design, photography), completely omitting music, writing, and film. Since the subagents all executed their assigned tasks correctly, the narrow decomposition by the coordinator is clearly the root cause of the missing coverage.

## Question 15

The document analysis subagent frequently encounters failures when processing PDF files—some have corrupted sections causing parsing exceptions, others are password-protected, and occasionally the parsing library times out on large files. Currently, any exception immediately terminates the subagent and returns an error to the coordinator, which must decide whether to retry, skip the document, or fail the entire research task. This is causing excessive coordinator involvement in routine error handling. What's the most effective architectural improvement?

A) Have the subagent implement local recovery for transient failures and only propagate errors it cannot resolve to the coordinator, including what was attempted and any partial results obtained.
B) Configure the subagent to always return partial results with success status, embedding error details in metadata. The coordinator treats all responses as successful and filters problematic items during synthesis.
C) Create a dedicated error-handling agent that monitors all subagent failures via a shared queue and makes recovery decisions independently, dispatching retry commands directly to subagents.
D) Have the coordinator validate all documents before dispatching to the subagent, rejecting documents likely to cause failures to ensure the subagent only receives processable files.

Correct Answer: A
Implementing local recovery for transient failures within the subagent follows the principle of handling errors at the lowest level capable of resolving them. This reduces excessive coordinator involvement while still escalating truly unresolvable issues with full context, including what recovery was attempted and any partial results obtained.
