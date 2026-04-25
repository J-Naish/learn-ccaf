## Me

`admin.organizations.me()  -> Organization`

**get** `/v1/organizations/me`

Retrieve information about the organization associated with the authenticated API key.

### Returns

- `class Organization: …`

  - `id: str`

    ID of the Organization.

  - `name: str`

    Name of the Organization.

  - `type: Literal["organization"]`

    Object type.

    For Organizations, this is always `"organization"`.

    - `"organization"`

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
organization = client.admin.organizations.me()
print(organization.id)
```