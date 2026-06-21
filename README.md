# LLM-Module1-Homework
This architecture is built entirely on the **Google Gemini API** (using `gemini-2.5-flash`) via the OpenAI SDK wrapper.

The following key architectural adjustments were made to ensure the systems work seamlessly on Gemini's infrastructure:

* **Targeted Client Routing:** Configured the client initialization to explicitly ingest the `GEMINI_API_KEY` and redirect all API traffic to Google's specialized `/v1beta/openai/` compatibility endpoint gateway.
* **Strict History Cleansing:** Cleaned the conversation array of legacy metadata keys (like `'type': 'function_call_output'`) and enforced strict alternating role sequences (`developer` $\rightarrow$ `user` $\rightarrow$ `assistant` $\rightarrow$ `tool`) to prevent payload validation failures.
* **Explicit Tool Parameter Nesting:** Wrapped all function definitions (name, description, parameters) deeply within an explicit outer `"function"` key container to satisfy Gemini’s strict tool schema parsing requirements.
* **Enforced Tool Handshake Mapping:** Modified the multi-turn agent loop to return the execution results using a precise structural handshake. When a `tool` role message is passed back to history, a top-level `"name"` string parameter matching the function identifier is explicitly passed alongside the `tool_call_id` to prevent backend empty name validation rejections.
