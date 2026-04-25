## Retrieve Messages

`admin.usage_report.retrieve_messages(UsageReportRetrieveMessagesParams**kwargs)  -> MessagesUsageReport`

**get** `/v1/organizations/usage_report/messages`

Get Messages Usage Report

### Parameters

- `starting_at: Union[str, datetime]`

  Time buckets that start on or after this RFC 3339 timestamp will be returned.
  Each time bucket will be snapped to the start of the minute/hour/day in UTC.

- `api_key_ids: Optional[Sequence[str]]`

  Restrict usage returned to the specified API key ID(s).

- `bucket_width: Optional[Literal["1d", "1m", "1h"]]`

  Time granularity of the response data.

  - `"1d"`

  - `"1m"`

  - `"1h"`

- `context_window: Optional[List[Literal["0-200k", "200k-1M"]]]`

  Restrict usage returned to the specified context window(s).

  - `"0-200k"`

  - `"200k-1M"`

- `ending_at: Optional[Union[str, datetime, null]]`

  Time buckets that end before this RFC 3339 timestamp will be returned.

- `group_by: Optional[List[Literal["api_key_id", "workspace_id", "model", 4 more]]]`

  Group by any subset of the available options. Grouping by `speed` requires the `fast-mode-2026-02-01` beta header.

  - `"api_key_id"`

  - `"workspace_id"`

  - `"model"`

  - `"service_tier"`

  - `"context_window"`

  - `"inference_geo"`

  - `"speed"`

- `inference_geos: Optional[List[Literal["global", "us", "not_available"]]]`

  Restrict usage returned to the specified inference geo(s). Use `not_available` for models that do not support specifying `inference_geo`.

  - `"global"`

  - `"us"`

  - `"not_available"`

- `limit: Optional[int]`

  Maximum number of time buckets to return in the response.

  The default and max limits depend on `bucket_width`:
  • `"1d"`: Default of 7 days, maximum of 31 days
  • `"1h"`: Default of 24 hours, maximum of 168 hours
  • `"1m"`: Default of 60 minutes, maximum of 1440 minutes

- `models: Optional[Sequence[str]]`

  Restrict usage returned to the specified model(s).

- `page: Optional[Union[str, datetime, null]]`

  Optionally set to the `next_page` token from the previous response.

- `service_tiers: Optional[List[Literal["standard", "batch", "priority", 3 more]]]`

  Restrict usage returned to the specified service tier(s).

  - `"standard"`

  - `"batch"`

  - `"priority"`

  - `"priority_on_demand"`

  - `"flex"`

  - `"flex_discount"`

- `speeds: Optional[List[Literal["standard", "fast"]]]`

  Restrict usage returned to the specified speed(s) (research preview).
  Requires the `fast-mode-2026-02-01` beta header.

  - `"standard"`

  - `"fast"`

- `workspace_ids: Optional[Sequence[str]]`

  Restrict usage returned to the specified workspace ID(s).

- `anthropic_beta: Optional[Sequence[str]]`

  Optional header to specify the beta version(s) you want to use.

  To use multiple betas, use a comma separated list like `beta1,beta2` or specify the header multiple times for each beta.

### Returns

- `class MessagesUsageReport: …`

  - `data: List[Data]`

    - `ending_at: datetime`

      End of the time bucket (exclusive) in RFC 3339 format.

    - `results: List[DataResult]`

      List of usage items for this time bucket.  There may be multiple items if one or more `group_by[]` parameters are specified.

      - `api_key_id: Optional[str]`

        ID of the API key used. `null` if not grouping by API key or for usage in the Anthropic Console.

      - `cache_creation: DataResultCacheCreation`

        The number of input tokens for cache creation.

        - `ephemeral_1h_input_tokens: int`

          The number of input tokens used to create the 1 hour cache entry.

        - `ephemeral_5m_input_tokens: int`

          The number of input tokens used to create the 5 minute cache entry.

      - `cache_read_input_tokens: int`

        The number of input tokens read from the cache.

      - `context_window: Optional[Literal["0-200k", "200k-1M"]]`

        Context window used. `null` if not grouping by context window.

        - `"0-200k"`

        - `"200k-1M"`

      - `inference_geo: Optional[str]`

        Inference geo used matching requests' `inference_geo` parameter if set, otherwise the workspace's `default_inference_geo`.
        For models that do not support specifying `inference_geo` the value is `"not_available"`. Always `null` if not grouping by inference geo.

      - `model: Optional[str]`

        Model used. `null` if not grouping by model.

      - `output_tokens: int`

        The number of output tokens generated.

      - `server_tool_use: DataResultServerToolUse`

        Server-side tool usage metrics.

        - `web_search_requests: int`

          The number of web search requests made.

      - `service_tier: Optional[Literal["standard", "batch", "priority", 3 more]]`

        Service tier used. `null` if not grouping by service tier.

        - `"standard"`

        - `"batch"`

        - `"priority"`

        - `"priority_on_demand"`

        - `"flex"`

        - `"flex_discount"`

      - `speed: Optional[Literal["standard", "fast"]]`

        Speed of the usage (research preview). `null` if not grouping by speed.
        Only returned when the `fast-mode-2026-02-01` beta header is provided.

        - `"standard"`

        - `"fast"`

      - `uncached_input_tokens: int`

        The number of uncached input tokens processed.

      - `workspace_id: Optional[str]`

        ID of the Workspace used. `null` if not grouping by workspace or for the default workspace.

    - `starting_at: datetime`

      Start of the time bucket (inclusive) in RFC 3339 format.

  - `has_more: bool`

    Indicates if there are more results.

  - `next_page: Optional[datetime]`

    Token to provide in as `page` in the subsequent request to retrieve the next page of data.

### Example

```python
import os
from datetime import datetime
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
messages_usage_report = client.admin.usage_report.retrieve_messages(
    starting_at=datetime.fromisoformat("2024-10-30T23:58:27.427722"),
)
print(messages_usage_report.data)
```