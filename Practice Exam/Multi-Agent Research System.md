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

Why A is incorrect:
Silently skipping the corrupted document hides critical information from the coordinator and the user, potentially undermining the completeness and accuracy of the final research output without anyone being aware of the gap.

Why C is incorrect:
Terminating the entire research workflow due to a single corrupted file is a disproportionate response that sacrifices all progress and prevents the system from processing the remaining valid documents.

Why D is incorrect:
Retrying with exponential backoff is a strategy suited for transient failures such as network timeouts, but file corruption is a permanent condition that will not resolve on its own, making repeated attempts wasteful and ineffective.

Study Area: Multi-Agent Research System — review Error Propagation concepts in the exam study guide.

## Question 2

The web search subagent returns results for only 3 of 5 requested source categories (competitor websites and industry reports succeeded, but news archives and social media feeds timed out). The document analysis subagent successfully processed all provided documents. The synthesis subagent must now produce a findings summary from this mixed-quality input. What's the most effective error propagation strategy?

A) Have the synthesis subagent return an error to the coordinator indicating incomplete upstream data, triggering a full retry or task failure.

B) Have the synthesis subagent request the coordinator retry the timed-out sources with extended timeouts before proceeding, ensuring complete data coverage before synthesis begins.

C) Structure the synthesis output with coverage annotations indicating which findings are well-supported versus which topic areas have gaps due to unavailable sources.

D) Proceed with synthesis using only the successful sources, generating output without indicating which data was unavailable.

Correct Answer: C
Structuring the synthesis output with coverage annotations embodies graceful degradation with transparency, allowing downstream consumers and end users to understand which findings are well-supported and which topic areas have gaps. This approach preserves the value of completed work while propagating uncertainty information so informed decisions can be made about confidence levels.

Why A is incorrect:
Treating partial success as total failure by returning an error wastes all the successfully completed work from both the web search and document analysis subagents. This approach provides no value when partial results with appropriate caveats would still be useful to stakeholders.

Why B is incorrect:
Having the synthesis subagent direct the coordinator to retry timed-out sources violates separation of concerns, as subagents should not dictate coordinator retry logic. Additionally, this approach assumes retries will succeed and unnecessarily delays producing useful output from the data already available.

Why D is incorrect:
Proceeding with synthesis without indicating which data sources were unavailable hides critical information from downstream consumers, potentially producing misleading research reports. Users would have no way to know that certain topic areas lack coverage, undermining the trustworthiness of the findings.

Study Area: Multi-Agent Research System — review Error Propagation concepts in the exam study guide.

## Question 3

The document analysis agent discovers that two credible sources contain directly conflicting statistics on a key metric: one government report states 40% growth while an industry analysis states 12% growth. Both sources appear legitimate and the discrepancy could significantly affect the research conclusions. What's the most effective way for the document analysis agent to handle this?

A) Complete the document analysis with both figures included, explicitly annotate the conflict with source attribution, and let the coordinator decide how to reconcile before passing to synthesis.

B) Include both figures in the analysis output without flagging them as conflicting, allowing the synthesis agent to determine which to use based on the broader research context.

C) Halt analysis and escalate to the coordinator immediately, asking it to determine which source is authoritative before the agent continues processing remaining documents.

D) Apply source credibility heuristics to select the most likely accurate figure, complete the analysis using that value, and include a footnote mentioning the discrepancy.

Correct Answer: A
This is the most effective approach because it respects separation of concerns: the document analysis agent completes its primary task without blocking, preserves both conflicting data points with explicit source attribution, and appropriately defers the reconciliation decision to the coordinator, which has the broader context needed to resolve the conflict.

Why B is incorrect:
Including both figures without flagging the conflict risks the discrepancy being overlooked by downstream agents, and it inappropriately delegates the reconciliation decision to the synthesis agent rather than the coordinator. Explicit conflict annotation is essential to ensure significant data discrepancies are properly addressed in the workflow.

Why C is incorrect:
Halting and escalating immediately introduces unnecessary workflow blocking, preventing the agent from completing its core document analysis task. The agent should finish processing all documents and flag the conflict rather than stopping work to wait for a coordinator decision.

Why D is incorrect:
Having the document analysis agent apply credibility heuristics to select one figure oversteps its role, as it lacks the broader context needed to make authoritative credibility judgments. This approach also risks losing important information by relegating the discrepancy to a footnote rather than treating it as a significant finding requiring reconciliation.

Study Area: Multi-Agent Research System — review Multi-Agent Orchestration concepts in the exam study guide.

## Question 4

During testing, you observe that the synthesis agent frequently needs to verify specific claims while combining findings. Currently, when verification is needed, the synthesis agent returns control to the coordinator, which invokes the web search agent, then re-invokes synthesis with results. This adds 2-3 round trips per task and increases latency by 40%. Your evaluation shows that 85% of these verifications are simple fact-checks (dates, names, statistics) while 15% require deeper investigation. What's the most effective approach to reduce overhead while maintaining system reliability?

A) Have the web search agent proactively cache extra context around each source during initial research, anticipating what the synthesis agent might need to verify.

B) Give the synthesis agent a scoped `verify_fact` tool for simple lookups, while complex verifications continue delegating to the web search agent through the coordinator.

C) Have the synthesis agent accumulate all verification needs and return them as a batch to the coordinator at the end of its pass, which then sends them all to the web search agent at once.

D) Give the synthesis agent access to all web search tools so it can handle any verification need directly without round-trips through the coordinator.

Correct Answer: B
Providing a scoped fact-verification tool handles the 85% of simple lookups directly, eliminating most round-trips while preserving the coordinator-based delegation path for the 15% of complex verifications. This applies the principle of least privilege, keeping the synthesis agent focused on its primary task while still reducing latency significantly.

Why A is incorrect:
Proactive caching relies on speculative prediction of what the synthesis agent will need to verify, which is inherently unreliable and wastes resources fetching information that may never be used. This approach cannot anticipate the specific facts requiring verification with sufficient accuracy to meaningfully reduce round-trips.

Why C is incorrect:
Batching all verification needs until the end of the synthesis pass creates blocking dependencies, since later synthesis steps may depend on facts that needed to be verified earlier. This approach cannot handle sequential dependencies within the synthesis process and would likely degrade output quality.

Why D is incorrect:
Giving the synthesis agent full access to all web search tools violates separation of concerns and over-provisions it beyond what is needed, potentially degrading its performance on its primary synthesis task. This approach also increases complexity and the risk of misuse of powerful search capabilities.

Study Area: Multi-Agent Research System — review Tool Distribution concepts in the exam study guide.

## Question 5

A colleague suggests having the document analysis agent send its output directly to the synthesis agent instead of routing through the coordinator. What is the main advantage of keeping the coordinator as the central hub for all subagent communication?

A) Routing through the coordinator enables automatic retry logic that direct agent-to-agent calls cannot support

B) The coordinator batches multiple subagent requests together, reducing the total number of API calls and overall latency

C) The coordinator can observe all interactions, handle errors consistently, and decide what information each subagent should receive

D) Subagents operate with isolated memory, and direct communication would require complex serialization that only the coordinator can perform

Correct Answer: C
This is correct. The coordinator pattern provides centralized visibility into all interactions, consistent error handling across the system, and fine-grained control over what information each subagent receives, which are the primary advantages of hub-and-spoke communication.

Why A is incorrect:
Automatic retry logic can be implemented at any level of the system, not exclusively within a coordinator. This creates a false dichotomy by implying that direct agent-to-agent calls inherently cannot support retries.

Why B is incorrect:
Routing all communication through a coordinator actually adds overhead and an extra hop rather than reducing API calls or latency. Batching is not the primary rationale for the coordinator pattern.

Why D is incorrect:
Serialization of data between components is not unique to coordinators—any component can serialize and pass data, so direct communication between subagents would not face a special serialization barrier that only a coordinator can overcome.

Study Area: Multi-Agent Research System — review Multi-Agent Orchestration concepts in the exam study guide.

## Question 6

Production monitoring reveals inconsistent synthesis quality. When aggregated results total ~75K tokens, the synthesis agent reliably cites information from the first 15K tokens (web search headlines and snippets) and the final 10K tokens (document analysis conclusions), but frequently omits critical findings that appear in the middle 50K tokens—even when those findings directly address the research question. How should you restructure the aggregated input?

A) Stream subagent results to the synthesis agent incrementally, processing web search results first to completion before introducing document analysis findings.

B) Implement rotation that alternates which subagent's results appear first across different research tasks, ensuring both sources receive primacy positioning equally over time.

C) Place a key findings summary at the beginning of the aggregated input and organize detailed results with explicit section headers for easier navigation.

D) Summarize all subagent outputs to under 20K tokens total before aggregation, ensuring content stays within the model's reliable processing range.

Correct Answer: C
Placing a key findings summary at the beginning leverages the primacy effect, ensuring critical information occupies the most reliably attended position. Adding explicit section headers throughout the aggregated input helps the model navigate and attend to middle-section content, directly mitigating the 'lost in the middle' phenomenon.

Why A is incorrect:
Streaming results sequentially and processing them one at a time prevents the synthesis agent from performing holistic cross-source analysis. This approach fragments the synthesis task rather than addressing the attention distribution issue within a single comprehensive context.

Why B is incorrect:
Rotating which subagent's results appear first only redistributes which findings get lost across different tasks rather than solving the core problem. This approach treats fairness between sources rather than reliable quality for any individual synthesis task.

Why D is incorrect:
Aggressively summarizing all outputs to under 20K tokens risks discarding critical detailed findings during the compression process. While shorter context would avoid the lost-in-the-middle effect, the information loss from summarization could be worse than the original problem.

Study Area: Multi-Agent Research System — review Long-Context Position Effects concepts in the exam study guide.

## Question 7

The web search and document analysis agents have both completed their tasks and returned findings to the coordinator. What is the appropriate next step for producing an integrated research output?

A) The document analysis agent requests the web search results and merges them internally

B) The coordinator passes both sets of findings to the synthesis agent for unified integration

C) The coordinator concatenates the raw outputs from both agents and returns them as the final result

D) Each agent directly sends its findings to the report generation agent, bypassing the coordinator

Correct Answer: B
This is correct because the orchestrator-workers pattern requires the coordinator to maintain centralized control by collecting results from subagents and routing them to the appropriate next component—in this case, the synthesis agent, which is specifically designed to unify and integrate findings into a coherent output.

Why A is incorrect:
Having one worker agent request results from another and merge them internally introduces peer-to-peer communication that breaks the centralized coordination pattern. The coordinator, not individual agents, should be responsible for collecting and routing all intermediate results through the workflow.

Why C is incorrect:
Simply concatenating raw outputs from both agents does not constitute meaningful integration, as it would produce an incoherent result lacking synthesis or unified analysis. The coordinator should leverage a specialized synthesis agent to produce a coherent, integrated research output.

Why D is incorrect:
Having agents communicate directly with the report generation agent bypasses the coordinator, violating the centralized control principle of the orchestrator-workers pattern. This creates architectural problems by introducing unmanaged peer-to-peer communication and removing the coordinator's ability to oversee the workflow.

Study Area: Multi-Agent Research System — review Multi-Agent Orchestration concepts in the exam study guide.

## Question 8

During a materials research task, the web search subagent queries three source categories with different outcomes: academic databases returned 15 relevant papers, industry reports returned "0 results found," and patent databases returned "Connection timeout." When designing error propagation to the coordinator, what approach enables the best recovery decisions?

A) Aggregate outcomes into a single success rate metric (e.g., "67% source coverage") with detailed logs available on request.

B) Have the subagent retry transient failures internally and only report persistent errors.

C) Distinguish access failures (timeout) needing retry decisions from valid empty results ("0 results") representing successful queries.

D) Report both the timeout and "0 results" as failures requiring coordinator intervention.

Correct Answer: C
This is correct because a timeout (access failure) and '0 results' (valid empty result) are semantically distinct outcomes requiring different responses. Distinguishing them enables the coordinator to retry the timed-out patent database while accepting the empty industry report results as a valid and informative finding.

Why A is incorrect:
Aggregating outcomes into a single success rate metric like "67% source coverage" destroys the actionable detail needed for recovery decisions, since it obscures whether the missing 33% is due to transient access failures or legitimately empty results. The coordinator cannot make appropriate retry or escalation decisions without understanding the nature of each individual outcome.

Why B is incorrect:
Having the subagent retry transient failures internally without reporting them hides important context from the coordinator, which limits its ability to manage resource budgets and make informed decisions about retry strategies. The coordinator needs visibility into access failures to appropriately allocate time and resources across the workflow.

Why D is incorrect:
Treating both the timeout and the "0 results" outcome as failures requiring coordinator intervention is incorrect because "0 results" represents a successful query that simply found no matching data, not a failure. This approach would cause unnecessary retries and waste resources on sources that have already been definitively queried.

Study Area: Multi-Agent Research System — review Error Propagation concepts in the exam study guide.

## Question 9

During testing, combined outputs from the web search agent (85K tokens including page content) and the document analysis agent (70K tokens including reasoning chains) total 155K tokens, but the synthesis agent performs optimally with inputs under 50K tokens. What's the most effective solution?

A) Store findings in a vector database and give the synthesis agent retrieval tools to query during its work

B) Have the synthesis agent process findings in sequential batches, maintaining running state between calls

C) Modify upstream agents to return structured data (key facts, citations, relevance scores) instead of verbose content and reasoning

D) Add an intermediate summarization agent that condenses findings before passing to synthesis

Correct Answer: C
Modifying upstream agents to return structured data (key facts, citations, relevance scores) addresses the root cause by reducing token volume at the source while preserving essential information. This eliminates verbose page content and reasoning chains that inflate token counts without adding value for the synthesis step.

Why A is incorrect:
Using a vector database with retrieval tools is over-engineered for this scenario, as synthesis requires comprehensive coverage of all known findings rather than selective query-based retrieval. This approach risks missing important information that doesn't match the synthesis agent's queries and adds unnecessary architectural complexity.

Why B is incorrect:
Processing findings in sequential batches degrades synthesis quality because the agent cannot see all findings simultaneously to identify cross-source patterns, contradictions, and connections. Maintaining running state between calls also adds complexity and risks compounding information loss across batches.

Why D is incorrect:
Adding an intermediate summarization agent introduces additional latency, cost, and risk of information loss, while the summarization agent itself must still process the full 155K tokens. This treats the symptom rather than the root cause of upstream agents producing unnecessarily verbose output.

Study Area: Multi-Agent Research System — review Multi-Agent Orchestration concepts in the exam study guide.

## Question 10

When designing the system, you gave the document analysis agent access to a general-purpose `fetch_url` tool so it could load documents from URLs. Production logs reveal this agent now frequently fetches search engine result pages to conduct ad-hoc web searches—behavior that should route through the web search agent. This causes inconsistent results. What's the most effective fix?

A) Remove `fetch_url` from the document analysis agent and route all URL loading through the coordinator to the web search agent.

B) Implement filtering that blocks `fetch_url` calls to known search engine domains while allowing other URLs.

C) Replace `fetch_url` with a `load_document` tool that validates URLs point to document formats.

D) Add instructions to the document analysis agent's prompt clarifying it should only use `fetch_url` for loading document URLs, not searching.

Correct Answer: C
Replacing the general-purpose tool with a document-specific tool that validates URLs point to document formats addresses the root cause by constraining capability at the interface level. This follows the principle of least privilege, making the undesired search behavior impossible rather than merely discouraged.

Why A is incorrect:
Removing URL fetching entirely from the document analysis agent and routing all requests through the coordinator eliminates useful capability and adds unnecessary latency for legitimate document loading. This over-corrects the problem when the agent legitimately needs to load documents from URLs.

Why B is incorrect:
Blocking known search engine domains uses a blocklist approach that requires ongoing maintenance and can never be comprehensive, as new search engines or alternative URLs can bypass the filter. This addresses symptoms rather than fixing the underlying design flaw of an overly permissive tool.

Why D is incorrect:
Relying on prompt instructions to constrain tool usage provides unreliable enforcement, since LLMs can ignore or misinterpret instructions when the tool technically permits the undesired behavior. This approach addresses symptoms through soft guidance rather than enforcing constraints at the tool level.

Study Area: Multi-Agent Research System — review Tool Distribution concepts in the exam study guide.

## Question 11

The web search subagent times out while researching a complex topic. You need to design how this failure information flows back to the coordinator agent. Which error propagation approach best enables intelligent recovery?

A) Catch the timeout within the subagent and return an empty result set marked as successful.

B) Propagate the timeout exception directly to a top-level handler that terminates the entire research workflow.

C) Implement automatic retry logic with exponential backoff within the subagent, returning a generic "search unavailable" status only after all retries are exhausted.

D) Return structured error context to the coordinator including the failure type, the attempted query, any partial results, and potential alternative approaches.

Correct Answer: D
Returning structured error context—including the failure type, attempted query, partial results, and alternative approaches—gives the coordinator all the information it needs to make intelligent recovery decisions, such as retrying with a modified query or proceeding with partial results. This is the best approach because it preserves maximum context for informed decision-making at the coordination level.

Why A is incorrect:
Catching the timeout and returning an empty result set marked as successful suppresses the error entirely, preventing the coordinator from knowing a failure occurred. This risks producing incomplete or misleading research outputs because the coordinator cannot trigger any recovery strategy for a problem it doesn't know exists.

Why B is incorrect:
Propagating the timeout exception directly to a top-level handler that terminates the entire workflow is an unnecessarily drastic response, as the coordinator could potentially recover through alternative strategies like modifying the query or using cached/partial results. This approach sacrifices resilience by treating a single subagent failure as a fatal workflow error.

Why C is incorrect:
While automatic retries with exponential backoff can be useful, returning only a generic "search unavailable" status after exhausting retries hides valuable context from the coordinator, preventing it from making informed recovery decisions such as modifying the query or trying alternative research approaches. The coordinator needs detailed failure information to orchestrate intelligent recovery, not just a binary success/failure status.

Study Area: Multi-Agent Research System — review Error Propagation concepts in the exam study guide.

## Question 12

When researching a broad topic, you observe that the web search agent and document analysis agent are both investigating the same subtopics, resulting in significant overlap in their findings. Token usage has nearly doubled without proportionally increasing the breadth or depth of research coverage. What's the most effective way to address this?

A) Implement a shared state mechanism where agents log their current focus area, allowing other agents to dynamically avoid duplicating work in progress

B) Have the coordinator explicitly partition the research space before delegation, assigning distinct subtopics or source types to each agent

C) Allow both agents to complete their parallel work, then have the coordinator deduplicate overlapping findings before passing to the synthesis agent

D) Convert to sequential execution where document analysis runs only after web search completes, using the web search findings as context to avoid duplication

Correct Answer: B
Having the coordinator explicitly partition the research space before delegation is the most effective approach because it addresses the root cause—unclear task boundaries—before any work begins. This preserves the benefits of parallel execution while preventing duplicated effort and wasted tokens.

Why A is incorrect:
While a shared state mechanism could theoretically reduce overlap, it introduces unnecessary complexity, potential race conditions, and still allows some duplicated work before agents detect each other's focus areas. Proactive partitioning at the coordinator level is simpler and more reliable than reactive dynamic coordination between agents.

Why C is incorrect:
Deduplicating findings after both agents complete their work fails to address the core problem of wasted tokens, since both agents still perform redundant research in parallel. This approach only cleans up the output without reducing the unnecessary resource consumption that was identified as the concern.

Why D is incorrect:
Converting to sequential execution unnecessarily sacrifices the performance benefits of parallelism when the same deduplication goal can be achieved through upfront task partitioning. This approach also increases total execution time without offering meaningful advantages over proactive partitioning of the research space.

Study Area: Multi-Agent Research System — review Multi-Agent Orchestration concepts in the exam study guide.

## Question 13

Production logs reveal a consistent pattern: requests to "analyze the quarterly report I uploaded" are routed to the web search agent 45% of the time instead of the document analysis agent. Examining the tool definitions, you find the web search agent has an analyze_content tool described as "analyzes content and extracts key information," while the document analysis agent has an analyze_document tool described as "analyzes documents and extracts key information." How should you address this misrouting?

A) Expand the document analysis tool's description to include example use cases like "Use for uploaded PDFs, Word documents, and spreadsheets" while leaving the web search tool unchanged.

B) Rename the web search tool to `extract_web_results` and update its description to "processes and returns information retrieved from web searches and URLs."

C) Add a pre-routing classifier that determines whether the user is referencing uploaded files or web content before the coordinator makes delegation decisions.

D) Add few-shot examples to the coordinator's prompt showing correct routing: "User uploads quarterly report > document analysis agent" and "User asks about a webpage > web search agent."

Correct Answer: B
Renaming the web search tool to 'extract_web_results' and updating its description to clearly reference web searches and URLs directly addresses the root cause by eliminating the semantic overlap between the two tools' names and descriptions. This makes each tool's purpose unambiguous, allowing the coordinator to correctly distinguish between document analysis and web search tasks.

Why A is incorrect:
Expanding only the document analysis tool's description addresses just one side of the semantic overlap, leaving the web search tool's ambiguous `analyze_content` name and generic description intact. This partial fix may reduce misrouting but does not fully resolve the confusion since the web search tool still appears relevant for general content analysis tasks.

Why C is incorrect:
Adding a pre-routing classifier over-engineers the solution by introducing an additional component when the core issue is simply ambiguous tool definitions. Fixing the tool names and descriptions is a simpler, more maintainable approach that addresses the root cause rather than adding architectural complexity.

Why D is incorrect:
Adding few-shot examples to the coordinator's prompt is a workaround that treats the symptom rather than fixing the underlying problem of ambiguous tool definitions. This approach is fragile and may not generalize well to varied phrasings of similar requests, whereas clarifying the tool definitions eliminates the confusion at its source.

Study Area: Multi-Agent Research System — review Tool Distribution concepts in the exam study guide.

## Question 14

After running the system on the topic "impact of AI on creative industries," you observe that each subagent completes successfully: the web search agent finds relevant articles, the document analysis agent summarizes papers correctly, and the synthesis agent produces coherent output. However, the final reports cover only visual arts, completely missing music, writing, and film production. When you examine the coordinator's logs, you see it decomposed the topic into three subtasks: "AI in digital art creation," "AI in graphic design," and "AI in photography." What is the most likely root cause?

A) The synthesis agent lacks instructions for identifying coverage gaps in the findings it receives from other agents.

B) The document analysis agent is filtering out sources related to non-visual creative industries due to overly restrictive relevance criteria.

C) The web search agent's queries are not comprehensive enough and need to be expanded to cover more creative industry sectors.

D) The coordinator agent's task decomposition is too narrow, resulting in subagent assignments that don't cover all relevant domains of the topic.

Correct Answer: D
The coordinator's logs directly reveal it decomposed the broad topic into only three visual arts subtasks (digital art, graphic design, photography), completely omitting music, writing, and film. Since the subagents all executed their assigned tasks correctly, the narrow decomposition by the coordinator is clearly the root cause of the missing coverage.

Why A is incorrect:
While the synthesis agent could theoretically flag coverage gaps, it can only work with the findings it receives. The root problem is upstream—the coordinator never assigned subtasks for music, writing, or film, so there were no findings about those domains to flag as missing.

Why B is incorrect:
There is no evidence that the document analysis agent filtered out non-visual sources. The missing coverage is explained entirely by the coordinator's narrow task decomposition, which never directed any subagent to research non-visual creative industries.

Why C is incorrect:
The web search agent correctly found relevant articles for the subtasks it was assigned. The problem is not with the search queries but with the fact that the coordinator never assigned subtasks covering music, writing, or film production in the first place.

Study Area: Multi-Agent Research System — review Multi-Agent Orchestration concepts in the exam study guide.

## Question 15

The document analysis subagent frequently encounters failures when processing PDF files—some have corrupted sections causing parsing exceptions, others are password-protected, and occasionally the parsing library times out on large files. Currently, any exception immediately terminates the subagent and returns an error to the coordinator, which must decide whether to retry, skip the document, or fail the entire research task. This is causing excessive coordinator involvement in routine error handling. What's the most effective architectural improvement?

A) Have the subagent implement local recovery for transient failures and only propagate errors it cannot resolve to the coordinator, including what was attempted and any partial results obtained.

B) Configure the subagent to always return partial results with success status, embedding error details in metadata. The coordinator treats all responses as successful and filters problematic items during synthesis.

C) Create a dedicated error-handling agent that monitors all subagent failures via a shared queue and makes recovery decisions independently, dispatching retry commands directly to subagents.

D) Have the coordinator validate all documents before dispatching to the subagent, rejecting documents likely to cause failures to ensure the subagent only receives processable files.

Correct Answer: A
Implementing local recovery for transient failures within the subagent follows the principle of handling errors at the lowest level capable of resolving them. This reduces excessive coordinator involvement while still escalating truly unresolvable issues with full context, including what recovery was attempted and any partial results obtained.

Why B is incorrect:
Masking errors as success and embedding error details in metadata hides real problems from the coordinator rather than solving them. This approach risks silent data quality issues during synthesis and removes the coordinator's ability to make informed decisions about genuinely unresolvable failures.

Why C is incorrect:
Introducing a dedicated error-handling agent adds unnecessary architectural complexity, an additional coordination layer, and a shared queue dependency. This approach distributes error-handling logic across multiple components rather than resolving failures at the appropriate level—the subagent itself.

Why D is incorrect:
Pre-validating all documents at the coordinator level increases the coordinator's burden rather than reducing it, and shifts routine error-handling responsibilities upward. Additionally, pre-validation cannot reliably predict all failure modes such as timeouts on large files or subtle corruption detected only during full parsing.

Study Area: Multi-Agent Research System — review Error Propagation concepts in the exam study guide.
