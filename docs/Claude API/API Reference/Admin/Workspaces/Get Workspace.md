## Retrieve

`admin.workspaces.retrieve(strworkspace_id)  -> Workspace`

**get** `/v1/organizations/workspaces/{workspace_id}`

Get Workspace

### Parameters

- `workspace_id: str`

  ID of the Workspace.

### Returns

- `class Workspace: …`

  - `id: str`

    ID of the Workspace.

  - `archived_at: Optional[datetime]`

    RFC 3339 datetime string indicating when the Workspace was archived, or `null` if the Workspace is not archived.

  - `created_at: datetime`

    RFC 3339 datetime string indicating when the Workspace was created.

  - `data_residency: DataResidency`

    Data residency configuration.

    - `allowed_inference_geos: Union[List[str], Literal["unrestricted"]]`

      Permitted inference geo values. 'unrestricted' means all geos are allowed.

      - `List[str]`

      - `Literal["unrestricted"]`

        - `"unrestricted"`

    - `default_inference_geo: str`

      Default inference geo applied when requests omit the parameter.

    - `workspace_geo: str`

      Geographic region for workspace data storage. Immutable after creation.

  - `display_color: str`

    Hex color code representing the Workspace in the Anthropic Console.

  - `name: str`

    Name of the Workspace.

  - `type: Literal["workspace"]`

    Object type.

    For Workspaces, this is always `"workspace"`.

    - `"workspace"`

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
workspace = client.admin.workspaces.retrieve(
    "workspace_id",
)
print(workspace.id)
```