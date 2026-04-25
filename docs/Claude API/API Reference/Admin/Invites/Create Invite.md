## Create

`admin.invites.create(InviteCreateParams**kwargs)  -> Invite`

**post** `/v1/organizations/invites`

Create Invite

### Parameters

- `email: str`

  Email of the User.

- `role: Literal["user", "developer", "billing", 2 more]`

  Role for the invited User. Cannot be "admin".

  - `"user"`

  - `"developer"`

  - `"billing"`

  - `"claude_code_user"`

  - `"managed"`

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
invite = client.admin.invites.create(
    email="user@emaildomain.com",
    role="user",
)
print(invite.id)
```