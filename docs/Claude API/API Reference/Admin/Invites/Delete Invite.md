## Delete

`admin.invites.delete(strinvite_id)  -> InviteDeleteResponse`

**delete** `/v1/organizations/invites/{invite_id}`

Delete Invite

### Parameters

- `invite_id: str`

  ID of the Invite.

### Returns

- `class InviteDeleteResponse: …`

  - `id: str`

    ID of the Invite.

  - `type: Literal["invite_deleted"]`

    Deleted object type.

    For Invites, this is always `"invite_deleted"`.

    - `"invite_deleted"`

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
invite = client.admin.invites.delete(
    "invite_id",
)
print(invite.id)
```