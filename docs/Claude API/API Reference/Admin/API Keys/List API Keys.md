## List

`admin.api_keys.list(APIKeyListParams**kwargs)  -> SyncPage[APIKey]`

**get** `/v1/organizations/api_keys`

List API Keys

### Parameters

- `after_id: Optional[str]`

  ID of the object to use as a cursor for pagination. When provided, returns the page of results immediately after this object.

- `before_id: Optional[str]`

  ID of the object to use as a cursor for pagination. When provided, returns the page of results immediately before this object.

- `created_by_user_id: Optional[str]`

  Filter by the ID of the User who created the object.

- `limit: Optional[int]`

  Number of items to return per page.

  Defaults to `20`. Ranges from `1` to `1000`.

- `status: Optional[Literal["active", "inactive", "archived"]]`

  Filter by API key status.

  - `"active"`

  - `"inactive"`

  - `"archived"`

- `workspace_id: Optional[str]`

  Filter by Workspace ID.

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
page = client.admin.api_keys.list()
page = page.data[0]
print(page.id)
```