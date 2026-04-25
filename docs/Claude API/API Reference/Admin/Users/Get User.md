## Retrieve

`admin.users.retrieve(struser_id)  -> User`

**get** `/v1/organizations/users/{user_id}`

Get User

### Parameters

- `user_id: str`

  ID of the User.

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
user = client.admin.users.retrieve(
    "user_id",
)
print(user.id)
```