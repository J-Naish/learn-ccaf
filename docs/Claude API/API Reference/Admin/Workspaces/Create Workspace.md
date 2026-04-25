## Create

`admin.workspaces.create(WorkspaceCreateParams**kwargs)  -> Workspace`

**post** `/v1/organizations/workspaces`

Create Workspace

### Parameters

- `name: str`

  Name of the Workspace.

- `data_residency: Optional[DataResidency]`

  Data residency configuration for the workspace. If omitted, defaults to workspace_geo=`"us"`, allowed_inference_geos=`"unrestricted"`, and default_inference_geo=`"global"`.

  - `allowed_inference_geos: Optional[Union[Sequence[str], Literal["unrestricted"], null]]`

    Permitted inference geo values. Defaults to 'unrestricted' if omitted, which allows all geos. Use the string 'unrestricted' to allow all geos, or a list of specific geos.

    - `Sequence[str]`

    - `Literal["unrestricted"]`

      - `"unrestricted"`

  - `default_inference_geo: Optional[str]`

    Default inference geo applied when requests omit the parameter. Defaults to 'global' if omitted. Must be a member of allowed_inference_geos unless allowed_inference_geos is `"unrestricted"`.

  - `workspace_geo: Optional[str]`

    Geographic region for workspace data storage. Immutable after creation. Defaults to 'us' if omitted.

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
workspace = client.admin.workspaces.create(
    name="x",
)
print(workspace.id)
```