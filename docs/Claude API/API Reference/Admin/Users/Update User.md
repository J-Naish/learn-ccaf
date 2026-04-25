## Update

`admin.users.update(struser_id, UserUpdateParams**kwargs)  -> User`

**post** `/v1/organizations/users/{user_id}`

Update User

### Parameters

- `user_id: str`

  ID of the User.

- `role: Literal["user", "developer", "billing", 2 more]`

  New role for the User. Cannot be "admin".

  - `"user"`

  - `"developer"`

  - `"billing"`

  - `"claude_code_user"`

  - `"managed"`

### Returns

- `class User: …`

  - `id: str`

    ID of the User.

  - `added_at: datetime`

    RFC 3339 datetime string indicating when the User joined the Organization.

  - `email: str`

    Email of the User.

  - `name: str`

    Name of the User.

  - `role: Literal["user", "developer", "billing", 3 more]`

    Organization role of the User.

    - `"user"`

    - `"developer"`

    - `"billing"`

    - `"admin"`

    - `"claude_code_user"`

    - `"managed"`

  - `type: Literal["user"]`

    Object type.

    For Users, this is always `"user"`.

    - `"user"`

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
user = client.admin.users.update(
    user_id="user_id",
    role="user",
)
print(user.id)
```