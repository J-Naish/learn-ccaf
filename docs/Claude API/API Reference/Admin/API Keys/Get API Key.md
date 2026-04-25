## Retrieve

`admin.api_keys.retrieve(strapi_key_id)  -> APIKey`

**get** `/v1/organizations/api_keys/{api_key_id}`

Get API Key

### Parameters

- `api_key_id: str`

  ID of the API key.

### Returns

- `class APIKey: …`

  - `id: str`

    ID of the API key.

  - `created_at: datetime`

    RFC 3339 datetime string indicating when the API Key was created.

  - `created_by: CreatedBy`

    The ID and type of the actor that created the API key.

    - `id: str`

      ID of the actor that created the object.

    - `type: str`

      Type of the actor that created the object.

  - `name: str`

    Name of the API key.

  - `partial_key_hint: Optional[str]`

    Partially redacted hint for the API key.

  - `status: Literal["active", "inactive", "archived"]`

    Status of the API key.

    - `"active"`

    - `"inactive"`

    - `"archived"`

  - `type: Literal["api_key"]`

    Object type.

    For API Keys, this is always `"api_key"`.

    - `"api_key"`

  - `workspace_id: Optional[str]`

    ID of the Workspace associated with the API key, or `null` if the API key belongs to the default Workspace.

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
api_key = client.admin.api_keys.retrieve(
    "api_key_id",
)
print(api_key.id)
```