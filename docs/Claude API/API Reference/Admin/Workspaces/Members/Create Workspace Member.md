## Create

`admin.workspaces.members.create(strworkspace_id, MemberCreateParams**kwargs)  -> WorkspaceMember`

**post** `/v1/organizations/workspaces/{workspace_id}/members`

Create Workspace Member

### Parameters

- `workspace_id: str`

  ID of the Workspace.

- `user_id: str`

  ID of the User.

- `workspace_role: Literal["workspace_user", "workspace_developer", "workspace_restricted_developer", "workspace_admin"]`

  Role of the new Workspace Member. Cannot be "workspace_billing".

  - `"workspace_user"`

  - `"workspace_developer"`

  - `"workspace_restricted_developer"`

  - `"workspace_admin"`

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
workspace_member = client.admin.workspaces.members.create(
    workspace_id="workspace_id",
    user_id="user_01WCz1FkmYMm4gnmykNKUu3Q",
    workspace_role="workspace_user",
)
print(workspace_member.user_id)
```