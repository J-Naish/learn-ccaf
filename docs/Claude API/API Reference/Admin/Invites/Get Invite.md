## Retrieve

`admin.invites.retrieve(strinvite_id)  -> Invite`

**get** `/v1/organizations/invites/{invite_id}`

Get Invite

### Parameters

- `invite_id: str`

  ID of the Invite.

### Returns

- `class Invite: …`

  - `id: str`

    ID of the Invite.

  - `email: str`

    Email of the User being invited.

  - `expires_at: datetime`

    RFC 3339 datetime string indicating when the Invite expires.

  - `invited_at: datetime`

    RFC 3339 datetime string indicating when the Invite was created.

  - `role: Literal["user", "developer", "billing", 3 more]`

    Organization role of the User.

    - `"user"`

    - `"developer"`

    - `"billing"`

    - `"admin"`

    - `"claude_code_user"`

    - `"managed"`

  - `status: Literal["accepted", "expired", "deleted", "pending"]`

    Status of the Invite.

    - `"accepted"`

    - `"expired"`

    - `"deleted"`

    - `"pending"`

  - `type: Literal["invite"]`

    Object type.

    For Invites, this is always `"invite"`.

    - `"invite"`

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
invite = client.admin.invites.retrieve(
    "invite_id",
)
print(invite.id)
```