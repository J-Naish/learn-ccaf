## List

`admin.users.list(UserListParams**kwargs)  -> SyncPage[User]`

**get** `/v1/organizations/users`

List Users

### Parameters

- `after_id: Optional[str]`

  ID of the object to use as a cursor for pagination. When provided, returns the page of results immediately after this object.

- `before_id: Optional[str]`

  ID of the object to use as a cursor for pagination. When provided, returns the page of results immediately before this object.

- `email: Optional[str]`

  Filter by user email.

- `limit: Optional[int]`

  Number of items to return per page.

  Defaults to `20`. Ranges from `1` to `1000`.

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
page = client.admin.users.list()
page = page.data[0]
print(page.id)
```