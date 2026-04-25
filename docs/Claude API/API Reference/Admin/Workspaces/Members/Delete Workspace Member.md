## Delete

`admin.workspaces.members.delete(struser_id, MemberDeleteParams**kwargs)  -> MemberDeleteResponse`

**delete** `/v1/organizations/workspaces/{workspace_id}/members/{user_id}`

Delete Workspace Member

### Parameters

- `workspace_id: str`

  ID of the Workspace.

- `user_id: str`

  ID of the User.

### Returns

- `class MemberDeleteResponse: …`

  - `type: Literal["workspace_member_deleted"]`

    Deleted object type.

    For Workspace Members, this is always `"workspace_member_deleted"`.

    - `"workspace_member_deleted"`

  - `user_id: str`

    ID of the User.

  - `workspace_id: str`

    ID of the Workspace.

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
member = client.admin.workspaces.members.delete(
    user_id="user_id",
    workspace_id="workspace_id",
)
print(member.user_id)
```