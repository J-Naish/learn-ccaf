## Retrieve

`admin.cost_report.retrieve(CostReportRetrieveParams**kwargs)  -> CostReport`

**get** `/v1/organizations/cost_report`

Get Cost Report

### Parameters

- `starting_at: Union[str, datetime]`

  Time buckets that start on or after this RFC 3339 timestamp will be returned.
  Each time bucket will be snapped to the start of the minute/hour/day in UTC.

- `bucket_width: Optional[Literal["1d"]]`

  Time granularity of the response data.

  - `"1d"`

- `ending_at: Optional[Union[str, datetime, null]]`

  Time buckets that end before this RFC 3339 timestamp will be returned.

- `group_by: Optional[List[Literal["workspace_id", "description"]]]`

  Group by any subset of the available options.

  - `"workspace_id"`

  - `"description"`

- `limit: Optional[int]`

  Maximum number of time buckets to return in the response.

- `page: Optional[Union[str, datetime, null]]`

  Optionally set to the `next_page` token from the previous response.

- `anthropic_beta: Optional[Sequence[str]]`

  Optional header to specify the beta version(s) you want to use.

  To use multiple betas, use a comma separated list like `beta1,beta2` or specify the header multiple times for each beta.

### Returns

- `class CostReport: …`

  - `data: List[Data]`

    - `ending_at: str`

      End of the time bucket (exclusive) in RFC 3339 format.

    - `results: List[DataResult]`

      List of cost items for this time bucket. There may be multiple items if one or more `group_by[]` parameters are specified.

      - `amount: str`

        Cost amount in lowest currency units (e.g. cents) as a decimal string. For example, `"123.45"` in `"USD"` represents `$1.23`.

      - `context_window: Optional[Literal["0-200k", "200k-1M"]]`

        Input context window used. `null` if not grouping by description or for non-token costs.

        - `"0-200k"`

        - `"200k-1M"`

      - `cost_type: Optional[Literal["tokens", "web_search", "code_execution"]]`

        Type of cost. `null` if not grouping by description.

        - `"tokens"`

        - `"web_search"`

        - `"code_execution"`

      - `currency: str`

        Currency code for the cost amount. Currently always `"USD"`.

      - `description: Optional[str]`

        Description of the cost item. `null` if not grouping by description.

      - `inference_geo: Optional[str]`

        Inference geo used matching requests' `inference_geo` parameter if set, otherwise the workspace's `default_inference_geo`.
        For models that do not support specifying `inference_geo` the value is `"not_available"`. Always `null` if not grouping by inference geo.

      - `model: Optional[str]`

        Model name used. `null` if not grouping by description or for non-token costs.

      - `service_tier: Optional[Literal["standard", "batch"]]`

        Service tier used. `null` if not grouping by description or for non-token costs.

        - `"standard"`

        - `"batch"`

      - `speed: Optional[Literal["standard", "fast"]]`

        Speed used (research preview). `null` if not grouping by speed, or for non-token costs.
        Only returned when the `fast-mode-2026-02-01` beta header is provided.

        - `"standard"`

        - `"fast"`

      - `token_type: Optional[Literal["uncached_input_tokens", "output_tokens", "cache_read_input_tokens", 2 more]]`

        Type of token. `null` if not grouping by description or for non-token costs.

        - `"uncached_input_tokens"`

        - `"output_tokens"`

        - `"cache_read_input_tokens"`

        - `"cache_creation.ephemeral_1h_input_tokens"`

        - `"cache_creation.ephemeral_5m_input_tokens"`

      - `workspace_id: Optional[str]`

        ID of the Workspace this cost is associated with. `null` if not grouping by workspace or for the default workspace.

    - `starting_at: str`

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
cost_report = client.admin.cost_report.retrieve(
    starting_at=datetime.fromisoformat("2024-10-30T23:58:27.427722"),
)
print(cost_report.data)
```