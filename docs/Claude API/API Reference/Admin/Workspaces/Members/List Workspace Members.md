## List

`admin.workspaces.members.list(strworkspace_id, MemberListParams**kwargs)  -> SyncPage[WorkspaceMember]`

**get** `/v1/organizations/workspaces/{workspace_id}/members`

List Workspace Members

### Parameters

- `workspace_id: str`

  ID of the Workspace.

- `after_id: Optional[str]`

  ID of the object to use as a cursor for pagination. When provided, returns the page of results immediately after this object.

- `before_id: Optional[str]`

  ID of the object to use as a cursor for pagination. When provided, returns the page of results immediately before this object.

- `limit: Optional[int]`

  Number of items to return per page.

  Defaults to `20`. Ranges from `1` to `1000`.

### Returns

- `class WorkspaceMember: …`

  - `type: Literal["workspace_member"]`

    Object type.

    For Workspace Members, this is always `"workspace_member"`.

    - `"workspace_member"`

  - `user_id: str`

    ID of the User.

  - `workspace_id: str`

    ID of the Workspace.

  - `workspace_role: Literal["workspace_user", "workspace_developer", "workspace_restricted_developer", 2 more]`

    Role of the Workspace Member.

    - `"workspace_user"`

    - `"workspace_developer"`

    - `"workspace_restricted_developer"`

    - `"workspace_admin"`

    - `"workspace_billing"`

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
page = client.admin.workspaces.members.list(
    workspace_id="workspace_id",
)
page = page.data[0]
print(page.user_id)
```