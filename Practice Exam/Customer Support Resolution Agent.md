# Customer Support Resolution Agent

You are building a **customer support resolution agent** using the Claude Agent SDK. The agent handles high-ambiguity requests like returns, billing disputes, and account issues. It has access to your backend systems through custom Model Context Protocol (MCP) tools (`get_customer`, `lookup_order`, `process_refund`, `escalate_to_human`). Your target is 80%+ first-contact resolution while knowing when to escalate.

## Question 1

Your agent achieves 55% first-contact resolution, well below the 80% target. Logs show it escalates straightforward cases (standard damage replacements with photo evidence) while attempting to autonomously handle complex situations requiring policy exceptions. What's the most effective way to improve escalation calibration?

A) Have the agent self-report a confidence score (1-10) before each response and automatically route requests to humans when confidence falls below a threshold.

B) Implement sentiment analysis to detect customer frustration levels and automatically escalate when negative sentiment exceeds a threshold.

C) Add explicit escalation criteria to your system prompt with few-shot examples demonstrating when to escalate versus resolve autonomously.

D) Deploy a separate classifier model trained on historical tickets to predict which requests need escalation before the main agent begins processing.

Correct Answer: C
Adding explicit escalation criteria with few-shot examples directly addresses the root cause—unclear decision boundaries between straightforward and complex cases. This is the most proportionate and effective first intervention, as it teaches the agent precisely when to escalate versus resolve autonomously without requiring additional infrastructure.

Why A is incorrect:
LLM self-reported confidence scores are notoriously poorly calibrated, and the logs already show the agent is incorrectly confident on complex cases while being overly cautious on simple ones. A numeric self-assessment would likely replicate the same miscalibration rather than fix the underlying decision boundary problem.

Why B is incorrect:
Sentiment analysis detects customer frustration, not case complexity, so it addresses a fundamentally different problem than the one described. Straightforward cases with calm customers would still be escalated and complex edge cases with polite customers would still be handled autonomously, failing to fix the miscalibration issue.

Why D is incorrect:
Deploying a separate classifier model is over-engineered for this situation, requiring labeled training data and additional ML infrastructure when the simpler approach of optimizing the system prompt with explicit escalation criteria hasn't yet been attempted. This adds unnecessary complexity and latency when prompt-level improvements should be tried first.

Study Area: Customer Support Resolution Agent — review Escalation Decisions concepts in the exam study guide.

## Question 2

Production logs show that for simple requests like "refund order #1234", your agent succeeds in 3-4 tool calls with 91% resolution rate. However, for complex requests like "I've been charged twice, my discount didn't apply, and I want to cancel", the agent averages 12+ tool calls with only 54% resolution—often investigating concerns sequentially and gathering redundant customer data for each one. What's the most effective change to improve complex request handling?

A) Decompose the request into distinct concerns, then investigate each in parallel using shared customer context before synthesizing a resolution.

B) Reduce the number of available tools by consolidating get_customer, lookup_order, and billing-related lookups into a single investigate_issue tool.

C) Add few-shot examples demonstrating ideal tool call sequences for various multi-part billing scenarios to your system prompt.

D) Add explicit verification gates between steps requiring the agent to checkpoint after resolving each concern before moving to the next.

Correct Answer: A
Decomposing the request into distinct concerns and investigating them in parallel with shared customer context directly addresses both core issues: it eliminates redundant data fetching by reusing context across concerns and reduces total tool calls by parallelizing investigations before synthesizing a unified resolution.

Why B is incorrect:
Consolidating multiple tools into a single investigation tool obscures useful functional distinctions and doesn't address the underlying workflow pattern problem of sequential processing and redundant data gathering across concerns.

Why C is incorrect:
While few-shot examples might marginally improve tool call sequences for known scenarios, they don't systematically solve the structural problems of redundant context gathering or sequential investigation, and they won't generalize well to novel multi-part request combinations.

Why D is incorrect:
Adding verification gates between sequential steps would actually worsen the problem by reinforcing the sequential processing pattern and adding overhead, rather than addressing the root cause of redundant data gathering and serial investigation.

Study Area: Customer Support Resolution Agent — review Multi-step Workflow Orchestration concepts in the exam study guide.

## Question 3

You're implementing the agentic loop for your support agent. After each API call to Claude, you need to determine whether to continue the loop (execute the requested tools and call Claude again) or stop (present the final response to the customer). What determines this decision?

A) Parse Claude's response text for phrases like "I've completed" or "Is there anything else?""—these natural language signals indicate the task is finished.

B) Set a maximum iteration count (e.g., 10 calls) and stop when reached, regardless of whether Claude indicates more work is needed.

C) Check the `stop_reason` field in Claude's response—continue when it equals ""tool_use" and stop when it equals "end_turn" .

D) Check whether the response includes any assistant text content—if Claude generated explanatory text, the loop should end.

Correct Answer: C
This is correct. The 'stop_reason' field is Claude's explicit, structured signal for loop control: '"tool_use" indicates Claude wants to execute a tool and receive the results back, while '""end_turn"' indicates Claude has completed its response and the loop should terminate.

Why A is incorrect:
Parsing natural language phrases to detect completion is unreliable and fragile, as Claude may use varied phrasing or include such phrases mid-task. The API provides a structured `stop_reason` field specifically designed for this purpose, making text parsing unnecessary and error-prone.

Why B is incorrect:
While setting a maximum iteration count is a useful safety guardrail to prevent runaway loops, it is not the primary mechanism for determining whether to continue or stop the agentic loop. The loop should be driven by Claude's explicit signals about whether it needs to use more tools, not by an arbitrary iteration limit.

Why D is incorrect:
This approach is incorrect because Claude frequently generates explanatory text alongside tool use requests in the same response. The presence of assistant text does not indicate that the task is complete, since Claude may be explaining its reasoning while simultaneously requesting a tool call.

Study Area: Customer Support Resolution Agent — review Agentic Loop Fundamentals concepts in the exam study guide.

## Question 4

After calling `get_customer` and `lookup_order` , the agent has retrieved all available system data but faces uncertainty. Which situation represents the most appropriate trigger for calling `escalate_to_human` ?

A) The customer claims they never received their order, but tracking shows it was delivered and signed for at their address three days ago. The agent should escalate because presenting contradictory evidence might damage the customer relationship.

B) The customer wants to cancel an order that shipped yesterday, with delivery scheduled for tomorrow. The agent should escalate because the customer might change their mind once they receive the package.

C) The customer requests a price match against a competitor. Your policies allow adjustments for price drops on your own site within 14 days but are silent on competitor pricing. The agent should escalate for policy interpretation.

D) The customer's message mentions both a billing question and a product return. The agent should escalate so a human can coordinate handling both issues in a single interaction.

Correct Answer: C
This represents a genuine policy gap where the company's guidelines cover own-site price drops but are silent on competitor price matching, meaning the agent cannot fabricate a policy and must escalate for human judgment on how to interpret or extend existing rules.

Why A is incorrect:
While the situation involves contradictory information, the agent has factual tracking data to share with the customer per standard procedure; escalating to avoid presenting evidence out of concern for relationship damage reflects emotional avoidance rather than an operational need for human intervention.

Why B is incorrect:
Escalating based on speculation that the customer might change their mind after receiving the package is not a valid trigger; the agent should address the customer's current, clearly stated request rather than deferring action based on hypothetical future intent.

Why D is incorrect:
Handling multiple issues such as a billing question and a product return does not require escalation, as the agent can address each topic sequentially within the same interaction using available tools and policies.

Study Area: Customer Support Resolution Agent — review Escalation Decisions concepts in the exam study guide.

## Question 5

Production logs show the agent frequently calls `get_customer` when users ask about orders (e.g., "check my order #12345"), instead of calling `lookup_order` . Both tools have minimal descriptions ("Retrieves customer information" / "Retrieves order details") and accept similar identifier formats. What's the most effective first step to improve tool selection reliability?

A) Expand each tool's description to include input formats it handles, example queries, edge cases, and boundaries explaining when to use it versus similar tools.

B) Consolidate both tools into a single lookup_entity tool that accepts any identifier and internally determines which backend to query.

C) Implement a routing layer that parses user input before each turn and pre-selects the appropriate tool based on detected keywords and identifier patterns.

D) Add few-shot examples to the system prompt demonstrating correct tool selection patterns, with 5-8 examples showing order- related queries routing to `lookup_order` .

Correct Answer: A
Expanding tool descriptions to include input formats, example queries, edge cases, and boundaries directly addresses the root cause— minimal descriptions that leave the LLM unable to distinguish between similar tools. This is a low-effort, high-leverage first step that improves the primary mechanism LLMs use for tool selection.

Why B is incorrect:
Consolidating both tools into a single endpoint is a valid architectural approach but requires significantly more engineering effort than simply improving tool descriptions. As a first step, it is disproportionate when the immediate problem—inadequate descriptions—can be resolved much more quickly.

Why C is incorrect:
Implementing a keyword-based routing layer is over-engineered for a first step and bypasses the LLM's natural language understanding capabilities. It also introduces brittle logic that must be maintained separately and may fail on ambiguous or novel queries.

Why D is incorrect:
Adding few-shot examples to the system prompt increases token overhead and addresses symptoms rather than the root cause. While examples can help, they are less effective and scalable than fixing the minimal tool descriptions that the LLM relies on for tool selection.

Study Area: Customer Support Resolution Agent — review Tool Interface Design concepts in the exam study guide.

## Question 6

Production logs show the agent sometimes selects `get_customer` when `lookup_order` would be more appropriate, particularly for ambiguous requests like "I need help with my recent purchase." You decide to add few-shot examples to your system prompt to improve tool selection. Which approach will most effectively address this issue?

A) Add explicit "Use when" and "do not use when" guidelines in each tool's description covering the ambiguous cases.

B) Add examples grouped by tool—all `get_customer` scenarios together, then all `lookup_order` scenarios.

C) Add 10-15 examples of clear, unambiguous requests that demonstrate correct tool selection for each tool's typical use cases.

D) Add 4-6 examples targeting ambiguous scenarios, each showing reasoning for why one tool was chosen over plausible alternatives.

Correct Answer: D
Targeting few-shot examples at the specific ambiguous scenarios where errors occur, with explicit reasoning about why one tool is preferred over another, directly teaches the model the comparative decision-making process it needs for edge cases. This approach is the most effective because worked examples demonstrating reasoning are better than declarative rules for nuanced tool selection.

Why A is incorrect:
Adding explicit usage guidelines to tool descriptions can help, but static declarative rules are less effective than worked examples for teaching nuanced edge-case reasoning. The model benefits from seeing the actual decision process in context than from reading abstract rules about when to use or avoid each tool.

Why B is incorrect:
Grouping examples by tool encourages the model to learn each tool's use cases in isolation, which fails to teach comparative reasoning about when to choose one tool over another for ambiguous requests. The problem specifically involves distinguishing between tools, so examples need to contrast tools rather than present them separately.

Why C is incorrect:
Providing examples of clear, unambiguous requests addresses cases where the model already performs well, not the specific failure mode of ambiguous requests. This approach wastes few-shot budget on easy cases and does nothing to teach the model how to reason through the tricky scenarios causing errors.

Study Area: Customer Support Resolution Agent — review Tool Selection Reliability concepts in the exam study guide.

## Question 7

Production data shows that in 12% of cases, your agent skips `get_customer` entirely and calls `lookup_order` using only the customer's stated name, occasionally leading to misidentified accounts and incorrect refunds. What change would most effectively address this reliability issue?

A) Enhance the system prompt to state that customer verification via `get_customer` is mandatory before any order operations.

B) Implement a routing classifier that analyzes each request and enables only the subset of tools appropriate for that request type.

C) Add few-shot examples showing the agent always calling `get_customer` first, even when customers volunteer order details.

D) Add a programmatic prerequisite that blocks `lookup_order` and `process_refund` calls until `get_customer` has returned a verified customer ID.

Correct Answer: D
Adding a programmatic prerequisite that blocks downstream tools until ' get_customer' returns a verified customer ID provides a deterministic guarantee that the required sequence is followed. This is the most effective approach because it removes the possibility of the agent skipping verification, regardless of LLM behavior.

Why A is incorrect:
Enhancing the system prompt to mandate customer verification relies on the LLM consistently following instructions, which is inherently probabilistic. Since 12% of cases already show the agent skipping this step, prompt-based guidance alone is insufficient for preventing errors with financial consequences.

Why B is incorrect:
Implementing a routing classifier to enable only relevant tool subsets addresses which tools are available per request type, not the ordering in which tools must be called. The core issue is enforcing required sequence (verification before order lookup), which tool-subset selection does not solve.

Why C is incorrect:
Adding few-shot examples demonstrating the correct tool-calling sequence can improve compliance but still depends on the LLM generalizing from those examples. This approach cannot guarantee deterministic adherence, making it unreliable for a critical business process where mistakes lead to incorrect refunds.

Study Area: Customer Support Resolution Agent — review Multi-step Workflow Enforcement concepts in the exam study guide.

## Question 8

Your `get_customer` tool returns all matches when searching by name. Claude currently picks the customer with the most recent order when multiple results are returned, but production data shows this causes 15% of multi-match cases to proceed with the wrong customer account. How should you address this?

A) Modify `get_customer` to return only the single most likely match based on a ranking algorithm, simplifying Claude's decision by eliminating ambiguous results.

B) Instruct Claude to ask for an additional identifier (email, phone, or order number) when `get_customer` returns multiple matches, before taking any customer-specific action.

C) Add few-shot examples showing Claude how to use conversational context (products mentioned, dates referenced) to infer the correct customer without requiring clarification.

D) Implement a confidence scoring system that proceeds automatically above 85% confidence and prompts for clarification below that threshold.

Correct Answer: B
Asking the user for an additional identifier (such as email, phone, or order number) is the most reliable way to disambiguate multiple matches, since the user has definitive knowledge of their own identity. One extra conversational turn is a small cost to eliminate the 15% error rate caused by incorrect customer selection.

Why A is incorrect:
Modifying the tool to return only a single best-guess match hides the ambiguity from Claude, removing its ability to recognize and handle cases where multiple customers share the same name. This approach may reduce errors in some cases but eliminates transparency and prevents proper disambiguation when the ranking algorithm is wrong.

Why C is incorrect:
Relying on conversational context clues like mentioned products or dates to infer the correct customer is unreliable, as these signals may be absent, ambiguous, or misleading. This approach trades one form of guessing for another without addressing the fundamental problem of needing definitive identification.

Why D is incorrect:
Using an arbitrary confidence threshold to decide when to auto-proceed versus ask for clarification is difficult to calibrate accurately and still allows errors whenever the confidence score is misleadingly high. This approach adds complexity without guaranteeing correct customer identification, unlike directly asking the user for a disambiguating identifier.

Study Area: Customer Support Resolution Agent — review Ambiguous Result Handling concepts in the exam study guide.

## Question 9

Your support agent uses progressive summarization—when context reaches 70% capacity, older turns are summarized while recent ones remain verbatim. Production logs reveal a pattern: customers reference specific amounts ("the 15% discount I mentioned"), but the agent responds with incorrect values. Investigation shows these details were stated 20+ turns ago and got condensed into vague summaries like "discussed promotional pricing." What's the most effective fix?

A) Extract transactional facts (amounts, dates, order numbers) into a persistent "case facts" block included in each prompt, outside the summarized history.

B) Revise the summarization prompt to explicitly preserve all numerical values, percentages, dates, and customer-stated expectations verbatim.

C) Increase the summarization threshold from 70% to 85% capacity so conversations have more room before summarization triggers.

D) Store full conversation history externally and implement retrieval to search it when the agent detects reference phrases like "as | mentioned."

Correct Answer: A
Extracting transactional facts (amounts, dates, order numbers) into a persistent "case facts" block addresses the root cause: summarization is inherently lossy for precise details. By preserving critical information in a structured block outside the summarized history, these facts remain reliably available in every prompt regardless of how many turns are summarized.

Why B is incorrect:
Revising the summarization prompt to preserve numerical values sounds reasonable but is unreliable in practice—LLMs don't consistently follow such preservation instructions under context pressure, meaning summarization will still be lossy for precise details. You cannot reliably "prompt your way out" of the fundamental information loss that summarization introduces.

Why C is incorrect:
Raising the summarization threshold from 70% to 85% only delays when summarization occurs rather than solving the underlying problem. Longer conversations will still eventually trigger summarization and lose the same critical details, making this a temporary workaround rather than a fix.

Why D is incorrect:
Storing full history externally with retrieval triggered by reference phrases is over-engineered and fragile, since detecting when a customer is referencing earlier details is unreliable. It also fails when the agent proactively uses incorrect values without the customer explicitly referencing prior statements.

Study Area: Customer Support Resolution Agent — review Conversation Context Management concepts in the exam study guide.

## Question 10

Production metrics show your agent averages 4+ API round-trips per resolution. Analysis reveals Claude frequently requests get_customer and `lookup_order` in separate sequential turns even when both are needed upfront. What's the most effective way to reduce round-trips?

A) Increase `max_tokens` to give Claude more space to plan ahead and naturally batch its tool requests.

B) Create composite tools like `get_customer_with_orders` that bundle common lookup combinations into single calls.

C) Implement speculative execution that automatically calls likely-needed tools alongside any requested tool, returning all results regardless of what was requested.

D) Prompt Claude to batch tool requests per turn, and return all tool results together before the next API call.

Correct Answer: D
Prompting Claude to batch related tool requests in a single turn, and returning all results together before the next API call, leverages Claude's native ability to request multiple tools simultaneously. This is the most effective approach because it directly addresses the sequential calling pattern with minimal architectural changes.

Why A is incorrect:
Increasing max_tokens only affects the maximum length of Claude's text output and does not influence whether Claude batches multiple tool requests in a single turn, so this change would have no meaningful impact on reducing round-trips.

Why B is incorrect:
Creating composite tools reduces flexibility and increases maintenance burden by requiring new bundled tools for every common combination, without addressing the root cause—which is that Claude already supports requesting multiple tools per turn and simply needs to be prompted to do so.

Why C is incorrect:
Speculatively executing tools that weren't explicitly requested wastes resources on unneeded calls, introduces unnecessary complexity, and may return irrelevant or confusing results that don't align with Claude's actual reasoning process.

Study Area: Customer Support Resolution Agent — review Parallel Tool Execution concepts in the exam study guide.

## Question 11

In testing, you notice the agent frequently calls `get_customer` when users ask about order status, even though `lookup_order` would be more appropriate. What should you examine first to address this issue?

A) Implement a pre-processing classifier that detects order queries and routes directly to lookup_order

B) Review tool descriptions to ensure they clearly distinguish each tool's purpose

C) Add few-shot examples covering every possible order-related query pattern to the system prompt

D) Reduce the number of tools available to the agent to simplify selection

Correct Answer: B
Tool descriptions are the primary input the model uses to decide which tool to call. When an agent consistently selects the wrong tool, the first diagnostic step is to examine whether the tool descriptions clearly distinguish each tool's purpose and specify when each should be used.

Why A is incorrect:
Building a separate pre-processing classifier to route queries is an over-engineered solution that bypasses the agent's native tool selection ability instead of fixing the underlying tool definitions that guide that selection.

Why C is incorrect:
Attempting to add few-shot examples covering every possible order-related query pattern is impractical and addresses symptoms rather than the root cause, which is likely ambiguous or overlapping tool descriptions.

Why D is incorrect:
Reducing the number of available tools sacrifices necessary functionality rather than addressing the root cause of why the agent confuses the two tools, which is most likely unclear tool descriptions.

Study Area: Customer Support Resolution Agent — review Tool Selection Reliability concepts in the exam study guide.

## Question 12

Production logs reveal that the agent misinterprets data from your MCP tools: Unix timestamps from `get_customer` , ISO 8601 dates from lookup_order , and numeric status codes (1=pending, 2=shipped). Some tools are third-party MCP servers you cannot modify. What's the most maintainable approach to normalize data formats?

A) Create a normalize_data tool that the agent calls after each data retrieval to transform values

B) Add detailed format documentation to your system prompt explaining each tool's data conventions

C) Modify tools you control to return human-readable formats; create wrapper tools for third-party tools

D) Use a `PostToolUse` hook to intercept tool results and apply formatting transformations before agent processing

Correct Answer: D
Using a `PostToolUse` hook provides a centralized, deterministic point to intercept and normalize all tool outputs—including those from third- party MCP servers—before the agent processes them. This is the most maintainable approach because it applies transformations uniformly via code rather than relying on LLM interpretation or agent behavior.

Why A is incorrect:
Requiring the agent to call a separate normalization tool after every data retrieval doubles API calls and depends on the agent reliably remembering to invoke it each time. This approach is fragile because agents can forget or skip steps, making it unsuitable for deterministic data transformations.

Why B is incorrect:
Adding format documentation to the system prompt relies on the LLM to probabilistically interpret and convert data formats correctly, which is unreliable for deterministic transformations. This approach is also fragile, as the agent may still misinterpret formats despite the documentation, especially under complex reasoning scenarios.

Why C is incorrect:
Modifying tools you control while wrapping third-party tools creates two different normalization strategies, resulting in an inconsistent and harder-to-maintain architecture. This dual approach increases complexity and makes it difficult to ensure uniform data formatting across all tools.

Study Area: Customer Support Resolution Agent — review Agent SDK Hook Patterns concepts in the exam study guide.

## Question 13

Production metrics show that when your agent resolves complex cases involving billing disputes or multi-order returns, customer satisfaction scores are 15% lower than for simple cases—even when the resolution is technically correct. Root cause analysis reveals the agent provides accurate resolutions but inconsistently explains the reasoning: sometimes omitting relevant policy details, other times missing timeline information or next steps. The specific context gaps vary by case. You want to improve resolution quality without adding human review overhead. Which approach is most effective?

A) Increase the model tier from Haiku to Sonnet for complex cases, routing based on detected case complexity.

B) Add a confirmation step where the agent asks "Does this fully address your concern?" before closing, letting customers request additional information if needed.

C) Implement few-shot examples in the system prompt showing complete resolution explanations for five common complex case types, demonstrating how to include policy context, timelines, and next steps.

D) Add a self-critique step where the agent evaluates its draft response for completeness—ensuring it addresses the customer's concern, includes relevant context, and anticipates follow-up questions.

Correct Answer: D
A self-critique step (evaluator-optimizer pattern) directly addresses the root cause of inconsistent explanation completeness by having the agent evaluate its own draft against specific criteria—such as policy context, timelines, and next steps—before presenting it. This catches case-specific gaps that vary across different complex scenarios without requiring human review.

Why A is incorrect:
Upgrading the model tier does not address the structural issue, since the agent is already producing technically correct resolutions—the problem is inconsistent inclusion of explanatory context, not insufficient reasoning capability. A more capable model without a process change would likely exhibit the same inconsistency.

Why B is incorrect:
Adding a customer confirmation step shifts the burden of identifying missing information onto the customer rather than proactively improving the agent's output quality. This increases customer effort and friction, which is likely to further harm satisfaction scores rather than improve them.

Why C is incorrect:
While few-shot examples can demonstrate ideal response structure for common case types, they cannot adequately cover the highly variable context gaps that differ from case to case. This approach works better for consistent, predictable patterns rather than the diverse omissions described in the scenario.

Study Area: Customer Support Resolution Agent — review Self-Evaluation Patterns concepts in the exam study guide.

## Question 14

Production logs reveal a consistent pattern: when customers include "account" in messages (e.g., "| want to check my account for the order I placed yesterday"), the agent calls `get_customer` first 78% of the time. When customers phrase similar requests without "account" (e.g., "| want to check on the order I placed yesterday"), it calls `lookup_order` first 93% of the time. The tool descriptions are well-written and unambiguous. What is the most likely root cause of this discrepancy?

A) The system prompt contains keyword-sensitive instructions that steer behavior based on terms like "account," creating unintended tool selection patterns

B) The tool descriptions need additional negative examples specifying when NOT to use each tool to prevent this keyword-triggered confusion

C) The model's base training creates associations between "account" terminology and customer-related operations that override the tool descriptions

D) The model requires more training data on multi-concept messages and should be fine-tuned on examples that include both account and order language

Correct Answer: A
This is the most likely root cause because the systematic, keyword-triggered pattern (78% vs 93%) strongly suggests explicit routing logic in the system prompt that reacts to the word "account" and directs the agent toward customer-related tools. Since the tool descriptions are already well-written and unambiguous, the discrepancy points to prompt-level instructions creating unintended behavioral steering.

Why B is incorrect:
Adding negative examples to tool descriptions contradicts the stated premise that the descriptions are already well-written and unambiguous. The issue is not with the tool descriptions themselves but with upstream instructions in the system prompt that override correct tool selection based on keyword triggers.

Why C is incorrect:
Base model associations are unlikely to be the root cause because the model correctly selects the appropriate tool 93% of the time when the word "account" is absent, demonstrating it can properly interpret the user's intent without interference. The sharp, keyword-dependent shift in behavior points to explicit prompt instructions rather than inherent model biases.

Why D is incorrect:
Proposing fine-tuning is an impractical and disproportionate response when the systematic keyword-triggered pattern strongly suggests a simpler prompt-level fix would resolve the issue. The model already demonstrates correct tool selection 93% of the time when the triggering keyword is absent, indicating it does not need additional training data.

Study Area: Customer Support Resolution Agent — review Tool Selection Reliability concepts in the exam study guide.

## Question 15

Your agent handles single-concern requests with 94% accuracy (e.g., "I need a refund for order #1234"). However, when customers include multiple concerns in one message (e.g., "I need a refund for order #1234 and also want to update my shipping address for order #5678"), tool selection accuracy drops to 58%. The agent typically addresses only one concern or mixes up parameters between requests. What's the most effective approach to improve reliability for multi-concern requests?

A) Implement response validation that detects incomplete responses and automatically re-prompts the agent to address any missed concerns.

B) Add few-shot examples to your prompt demonstrating the correct reasoning and tool sequence for multi-concern requests.

C) Implement a preprocessing layer that uses a separate model call to decompose multi-concern messages into individual requests, process each independently, then combine the results.

D) Consolidate related tools into fewer, more general-purpose tools.

Correct Answer: B
Adding few-shot examples demonstrating correct reasoning and tool sequencing for multi-concern requests is the most effective approach because the agent already handles individual concerns well at 94% accuracy—it simply needs pattern guidance for handling multiple concerns in one message. This is a low-cost, proven technique that directly addresses the root cause of the agent failing to decompose and properly route parameters across multiple requests.

Why A is incorrect:
Detecting incomplete responses and re-prompting is a reactive strategy that only addresses missed concerns after the fact and does not prevent the core issue of parameter mixing between requests. A preventive approach that teaches the agent to correctly handle multi-concern messages from the start is more effective and efficient.

Why C is incorrect:
Using a separate model call to decompose multi-concern messages adds unnecessary latency, complexity, and cost when the agent already demonstrates strong single-concern understanding. This over-engineers the solution when simpler prompt-level guidance can address the pattern recognition gap.

Why D is incorrect:
Consolidating tools into fewer general-purpose tools misdiagnoses the problem, since the agent's high 94% accuracy on single-concern requests shows that tool selection and design are not the issue. The problem lies in the agent's inability to recognize and separately handle multiple concerns, not in having too many tools.

Study Area: Customer Support Resolution Agent — review Tool Selection Reliability concepts in the exam study guide.
