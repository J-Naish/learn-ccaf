## Delete

`admin.users.delete(struser_id)  -> UserDeleteResponse`

**delete** `/v1/organizations/users/{user_id}`

Remove User

### Parameters

- `user_id: str`

  ID of the User.

### Returns

- `class UserDeleteResponse: …`

  - `id: str`

    ID of the User.

  - `type: Literal["user_deleted"]`

    Deleted object type.

    For Users, this is always `"user_deleted"`.

    - `"user_deleted"`

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
user = client.admin.users.delete(
    "user_id",
)
print(user.id)
```