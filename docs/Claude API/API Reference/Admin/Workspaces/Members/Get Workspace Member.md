## Retrieve

`admin.workspaces.members.retrieve(struser_id, MemberRetrieveParams**kwargs)  -> WorkspaceMember`

**get** `/v1/organizations/workspaces/{workspace_id}/members/{user_id}`

Get Workspace Member

### Parameters

- `workspace_id: str`

  ID of the Workspace.

- `user_id: str`

  ID of the User.

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
workspace_member = client.admin.workspaces.members.retrieve(
    user_id="user_id",
    workspace_id="workspace_id",
)
print(workspace_member.user_id)
```