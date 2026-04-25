# Admin

# Organizations

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

## Domain Types

### Organization

- `class Organization: …`

  - `id: str`

    ID of the Organization.

  - `name: str`

    Name of the Organization.

  - `type: Literal["organization"]`

    Object type.

    For Organizations, this is always `"organization"`.

    - `"organization"`

# Invites

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

## List

`admin.invites.list(InviteListParams**kwargs)  -> SyncPage[Invite]`

**get** `/v1/organizations/invites`

List Invites

### Parameters

- `after_id: Optional[str]`

  ID of the object to use as a cursor for pagination. When provided, returns the page of results immediately after this object.

- `before_id: Optional[str]`

  ID of the object to use as a cursor for pagination. When provided, returns the page of results immediately before this object.

- `limit: Optional[int]`

  Number of items to return per page.

  Defaults to `20`. Ranges from `1` to `1000`.

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
page = client.admin.invites.list()
page = page.data[0]
print(page.id)
```

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

## Domain Types

### Invite

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

### Invite Delete Response

- `class InviteDeleteResponse: …`

  - `id: str`

    ID of the Invite.

  - `type: Literal["invite_deleted"]`

    Deleted object type.

    For Invites, this is always `"invite_deleted"`.

    - `"invite_deleted"`

# Users

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

## Domain Types

### User

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

### User Delete Response

- `class UserDeleteResponse: …`

  - `id: str`

    ID of the User.

  - `type: Literal["user_deleted"]`

    Deleted object type.

    For Users, this is always `"user_deleted"`.

    - `"user_deleted"`

# Workspaces

## Create

`admin.workspaces.create(WorkspaceCreateParams**kwargs)  -> Workspace`

**post** `/v1/organizations/workspaces`

Create Workspace

### Parameters

- `name: str`

  Name of the Workspace.

- `data_residency: Optional[DataResidency]`

  Data residency configuration for the workspace. If omitted, defaults to workspace_geo=`"us"`, allowed_inference_geos=`"unrestricted"`, and default_inference_geo=`"global"`.

  - `allowed_inference_geos: Optional[Union[Sequence[str], Literal["unrestricted"], null]]`

    Permitted inference geo values. Defaults to 'unrestricted' if omitted, which allows all geos. Use the string 'unrestricted' to allow all geos, or a list of specific geos.

    - `Sequence[str]`

    - `Literal["unrestricted"]`

      - `"unrestricted"`

  - `default_inference_geo: Optional[str]`

    Default inference geo applied when requests omit the parameter. Defaults to 'global' if omitted. Must be a member of allowed_inference_geos unless allowed_inference_geos is `"unrestricted"`.

  - `workspace_geo: Optional[str]`

    Geographic region for workspace data storage. Immutable after creation. Defaults to 'us' if omitted.

### Returns

- `class Workspace: …`

  - `id: str`

    ID of the Workspace.

  - `archived_at: Optional[datetime]`

    RFC 3339 datetime string indicating when the Workspace was archived, or `null` if the Workspace is not archived.

  - `created_at: datetime`

    RFC 3339 datetime string indicating when the Workspace was created.

  - `data_residency: DataResidency`

    Data residency configuration.

    - `allowed_inference_geos: Union[List[str], Literal["unrestricted"]]`

      Permitted inference geo values. 'unrestricted' means all geos are allowed.

      - `List[str]`

      - `Literal["unrestricted"]`

        - `"unrestricted"`

    - `default_inference_geo: str`

      Default inference geo applied when requests omit the parameter.

    - `workspace_geo: str`

      Geographic region for workspace data storage. Immutable after creation.

  - `display_color: str`

    Hex color code representing the Workspace in the Anthropic Console.

  - `name: str`

    Name of the Workspace.

  - `type: Literal["workspace"]`

    Object type.

    For Workspaces, this is always `"workspace"`.

    - `"workspace"`

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
workspace = client.admin.workspaces.create(
    name="x",
)
print(workspace.id)
```

## Retrieve

`admin.workspaces.retrieve(strworkspace_id)  -> Workspace`

**get** `/v1/organizations/workspaces/{workspace_id}`

Get Workspace

### Parameters

- `workspace_id: str`

  ID of the Workspace.

### Returns

- `class Workspace: …`

  - `id: str`

    ID of the Workspace.

  - `archived_at: Optional[datetime]`

    RFC 3339 datetime string indicating when the Workspace was archived, or `null` if the Workspace is not archived.

  - `created_at: datetime`

    RFC 3339 datetime string indicating when the Workspace was created.

  - `data_residency: DataResidency`

    Data residency configuration.

    - `allowed_inference_geos: Union[List[str], Literal["unrestricted"]]`

      Permitted inference geo values. 'unrestricted' means all geos are allowed.

      - `List[str]`

      - `Literal["unrestricted"]`

        - `"unrestricted"`

    - `default_inference_geo: str`

      Default inference geo applied when requests omit the parameter.

    - `workspace_geo: str`

      Geographic region for workspace data storage. Immutable after creation.

  - `display_color: str`

    Hex color code representing the Workspace in the Anthropic Console.

  - `name: str`

    Name of the Workspace.

  - `type: Literal["workspace"]`

    Object type.

    For Workspaces, this is always `"workspace"`.

    - `"workspace"`

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
workspace = client.admin.workspaces.retrieve(
    "workspace_id",
)
print(workspace.id)
```

## List

`admin.workspaces.list(WorkspaceListParams**kwargs)  -> SyncPage[Workspace]`

**get** `/v1/organizations/workspaces`

List Workspaces

### Parameters

- `after_id: Optional[str]`

  ID of the object to use as a cursor for pagination. When provided, returns the page of results immediately after this object.

- `before_id: Optional[str]`

  ID of the object to use as a cursor for pagination. When provided, returns the page of results immediately before this object.

- `include_archived: Optional[bool]`

  Whether to include Workspaces that have been archived in the response

- `limit: Optional[int]`

  Number of items to return per page.

  Defaults to `20`. Ranges from `1` to `1000`.

### Returns

- `class Workspace: …`

  - `id: str`

    ID of the Workspace.

  - `archived_at: Optional[datetime]`

    RFC 3339 datetime string indicating when the Workspace was archived, or `null` if the Workspace is not archived.

  - `created_at: datetime`

    RFC 3339 datetime string indicating when the Workspace was created.

  - `data_residency: DataResidency`

    Data residency configuration.

    - `allowed_inference_geos: Union[List[str], Literal["unrestricted"]]`

      Permitted inference geo values. 'unrestricted' means all geos are allowed.

      - `List[str]`

      - `Literal["unrestricted"]`

        - `"unrestricted"`

    - `default_inference_geo: str`

      Default inference geo applied when requests omit the parameter.

    - `workspace_geo: str`

      Geographic region for workspace data storage. Immutable after creation.

  - `display_color: str`

    Hex color code representing the Workspace in the Anthropic Console.

  - `name: str`

    Name of the Workspace.

  - `type: Literal["workspace"]`

    Object type.

    For Workspaces, this is always `"workspace"`.

    - `"workspace"`

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
page = client.admin.workspaces.list()
page = page.data[0]
print(page.id)
```

## Update

`admin.workspaces.update(strworkspace_id, WorkspaceUpdateParams**kwargs)  -> Workspace`

**post** `/v1/organizations/workspaces/{workspace_id}`

Update Workspace

### Parameters

- `workspace_id: str`

  ID of the Workspace.

- `name: str`

  Name of the Workspace.

- `data_residency: Optional[DataResidency]`

  Data residency configuration for the workspace.

  - `allowed_inference_geos: Optional[Union[Sequence[str], Literal["unrestricted"], null]]`

    Permitted inference geo values. Use 'unrestricted' to allow all geos, or a list of specific geos.

    - `Sequence[str]`

    - `Literal["unrestricted"]`

      - `"unrestricted"`

  - `default_inference_geo: Optional[str]`

    Default inference geo applied when requests omit the parameter. Must be a member of allowed_inference_geos unless allowed_inference_geos is `"unrestricted"`.

### Returns

- `class Workspace: …`

  - `id: str`

    ID of the Workspace.

  - `archived_at: Optional[datetime]`

    RFC 3339 datetime string indicating when the Workspace was archived, or `null` if the Workspace is not archived.

  - `created_at: datetime`

    RFC 3339 datetime string indicating when the Workspace was created.

  - `data_residency: DataResidency`

    Data residency configuration.

    - `allowed_inference_geos: Union[List[str], Literal["unrestricted"]]`

      Permitted inference geo values. 'unrestricted' means all geos are allowed.

      - `List[str]`

      - `Literal["unrestricted"]`

        - `"unrestricted"`

    - `default_inference_geo: str`

      Default inference geo applied when requests omit the parameter.

    - `workspace_geo: str`

      Geographic region for workspace data storage. Immutable after creation.

  - `display_color: str`

    Hex color code representing the Workspace in the Anthropic Console.

  - `name: str`

    Name of the Workspace.

  - `type: Literal["workspace"]`

    Object type.

    For Workspaces, this is always `"workspace"`.

    - `"workspace"`

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
workspace = client.admin.workspaces.update(
    workspace_id="workspace_id",
    name="x",
)
print(workspace.id)
```

## Archive

`admin.workspaces.archive(strworkspace_id)  -> Workspace`

**post** `/v1/organizations/workspaces/{workspace_id}/archive`

Archive Workspace

### Parameters

- `workspace_id: str`

  ID of the Workspace.

### Returns

- `class Workspace: …`

  - `id: str`

    ID of the Workspace.

  - `archived_at: Optional[datetime]`

    RFC 3339 datetime string indicating when the Workspace was archived, or `null` if the Workspace is not archived.

  - `created_at: datetime`

    RFC 3339 datetime string indicating when the Workspace was created.

  - `data_residency: DataResidency`

    Data residency configuration.

    - `allowed_inference_geos: Union[List[str], Literal["unrestricted"]]`

      Permitted inference geo values. 'unrestricted' means all geos are allowed.

      - `List[str]`

      - `Literal["unrestricted"]`

        - `"unrestricted"`

    - `default_inference_geo: str`

      Default inference geo applied when requests omit the parameter.

    - `workspace_geo: str`

      Geographic region for workspace data storage. Immutable after creation.

  - `display_color: str`

    Hex color code representing the Workspace in the Anthropic Console.

  - `name: str`

    Name of the Workspace.

  - `type: Literal["workspace"]`

    Object type.

    For Workspaces, this is always `"workspace"`.

    - `"workspace"`

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
workspace = client.admin.workspaces.archive(
    "workspace_id",
)
print(workspace.id)
```

# Members

## Create

`admin.workspaces.members.create(strworkspace_id, MemberCreateParams**kwargs)  -> WorkspaceMember`

**post** `/v1/organizations/workspaces/{workspace_id}/members`

Create Workspace Member

### Parameters

- `workspace_id: str`

  ID of the Workspace.

- `user_id: str`

  ID of the User.

- `workspace_role: Literal["workspace_user", "workspace_developer", "workspace_restricted_developer", "workspace_admin"]`

  Role of the new Workspace Member. Cannot be "workspace_billing".

  - `"workspace_user"`

  - `"workspace_developer"`

  - `"workspace_restricted_developer"`

  - `"workspace_admin"`

### Returns

- `class WorkspaceMember: …`

  - `type: Literal["workspace_member"]`

    Object type.

    For Workspace Members, this is always `"workspace_member"`.

    - `"workspace_member"`

  - `user_id: str`

    ID of the User.

  - `workspace_id: str`

    ID of the Workspace.

  - `workspace_role: Literal["workspace_user", "workspace_developer", "workspace_restricted_developer", 2 more]`

    Role of the Workspace Member.

    - `"workspace_user"`

    - `"workspace_developer"`

    - `"workspace_restricted_developer"`

    - `"workspace_admin"`

    - `"workspace_billing"`

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
workspace_member = client.admin.workspaces.members.create(
    workspace_id="workspace_id",
    user_id="user_01WCz1FkmYMm4gnmykNKUu3Q",
    workspace_role="workspace_user",
)
print(workspace_member.user_id)
```

## Retrieve

`admin.workspaces.members.retrieve(struser_id, MemberRetrieveParams**kwargs)  -> WorkspaceMember`

**get** `/v1/organizations/workspaces/{workspace_id}/members/{user_id}`

Get Workspace Member

### Parameters

- `workspace_id: str`

  ID of the Workspace.

- `user_id: str`

  ID of the User.

### Returns

- `class WorkspaceMember: …`

  - `type: Literal["workspace_member"]`

    Object type.

    For Workspace Members, this is always `"workspace_member"`.

    - `"workspace_member"`

  - `user_id: str`

    ID of the User.

  - `workspace_id: str`

    ID of the Workspace.

  - `workspace_role: Literal["workspace_user", "workspace_developer", "workspace_restricted_developer", 2 more]`

    Role of the Workspace Member.

    - `"workspace_user"`

    - `"workspace_developer"`

    - `"workspace_restricted_developer"`

    - `"workspace_admin"`

    - `"workspace_billing"`

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
workspace_member = client.admin.workspaces.members.retrieve(
    user_id="user_id",
    workspace_id="workspace_id",
)
print(workspace_member.user_id)
```

## List

`admin.workspaces.members.list(strworkspace_id, MemberListParams**kwargs)  -> SyncPage[WorkspaceMember]`

**get** `/v1/organizations/workspaces/{workspace_id}/members`

List Workspace Members

### Parameters

- `workspace_id: str`

  ID of the Workspace.

- `after_id: Optional[str]`

  ID of the object to use as a cursor for pagination. When provided, returns the page of results immediately after this object.

- `before_id: Optional[str]`

  ID of the object to use as a cursor for pagination. When provided, returns the page of results immediately before this object.

- `limit: Optional[int]`

  Number of items to return per page.

  Defaults to `20`. Ranges from `1` to `1000`.

### Returns

- `class WorkspaceMember: …`

  - `type: Literal["workspace_member"]`

    Object type.

    For Workspace Members, this is always `"workspace_member"`.

    - `"workspace_member"`

  - `user_id: str`

    ID of the User.

  - `workspace_id: str`

    ID of the Workspace.

  - `workspace_role: Literal["workspace_user", "workspace_developer", "workspace_restricted_developer", 2 more]`

    Role of the Workspace Member.

    - `"workspace_user"`

    - `"workspace_developer"`

    - `"workspace_restricted_developer"`

    - `"workspace_admin"`

    - `"workspace_billing"`

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
page = client.admin.workspaces.members.list(
    workspace_id="workspace_id",
)
page = page.data[0]
print(page.user_id)
```

## Update

`admin.workspaces.members.update(struser_id, MemberUpdateParams**kwargs)  -> WorkspaceMember`

**post** `/v1/organizations/workspaces/{workspace_id}/members/{user_id}`

Update Workspace Member

### Parameters

- `workspace_id: str`

  ID of the Workspace.

- `user_id: str`

  ID of the User.

- `workspace_role: Literal["workspace_user", "workspace_developer", "workspace_restricted_developer", 2 more]`

  New workspace role for the User.

  - `"workspace_user"`

  - `"workspace_developer"`

  - `"workspace_restricted_developer"`

  - `"workspace_admin"`

  - `"workspace_billing"`

### Returns

- `class WorkspaceMember: …`

  - `type: Literal["workspace_member"]`

    Object type.

    For Workspace Members, this is always `"workspace_member"`.

    - `"workspace_member"`

  - `user_id: str`

    ID of the User.

  - `workspace_id: str`

    ID of the Workspace.

  - `workspace_role: Literal["workspace_user", "workspace_developer", "workspace_restricted_developer", 2 more]`

    Role of the Workspace Member.

    - `"workspace_user"`

    - `"workspace_developer"`

    - `"workspace_restricted_developer"`

    - `"workspace_admin"`

    - `"workspace_billing"`

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
workspace_member = client.admin.workspaces.members.update(
    user_id="user_id",
    workspace_id="workspace_id",
    workspace_role="workspace_user",
)
print(workspace_member.user_id)
```

## Delete

`admin.workspaces.members.delete(struser_id, MemberDeleteParams**kwargs)  -> MemberDeleteResponse`

**delete** `/v1/organizations/workspaces/{workspace_id}/members/{user_id}`

Delete Workspace Member

### Parameters

- `workspace_id: str`

  ID of the Workspace.

- `user_id: str`

  ID of the User.

### Returns

- `class MemberDeleteResponse: …`

  - `type: Literal["workspace_member_deleted"]`

    Deleted object type.

    For Workspace Members, this is always `"workspace_member_deleted"`.

    - `"workspace_member_deleted"`

  - `user_id: str`

    ID of the User.

  - `workspace_id: str`

    ID of the Workspace.

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
member = client.admin.workspaces.members.delete(
    user_id="user_id",
    workspace_id="workspace_id",
)
print(member.user_id)
```

## Domain Types

### Workspace Member

- `class WorkspaceMember: …`

  - `type: Literal["workspace_member"]`

    Object type.

    For Workspace Members, this is always `"workspace_member"`.

    - `"workspace_member"`

  - `user_id: str`

    ID of the User.

  - `workspace_id: str`

    ID of the Workspace.

  - `workspace_role: Literal["workspace_user", "workspace_developer", "workspace_restricted_developer", 2 more]`

    Role of the Workspace Member.

    - `"workspace_user"`

    - `"workspace_developer"`

    - `"workspace_restricted_developer"`

    - `"workspace_admin"`

    - `"workspace_billing"`

### Member Delete Response

- `class MemberDeleteResponse: …`

  - `type: Literal["workspace_member_deleted"]`

    Deleted object type.

    For Workspace Members, this is always `"workspace_member_deleted"`.

    - `"workspace_member_deleted"`

  - `user_id: str`

    ID of the User.

  - `workspace_id: str`

    ID of the Workspace.

# API Keys

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

## Update

`admin.api_keys.update(strapi_key_id, APIKeyUpdateParams**kwargs)  -> APIKey`

**post** `/v1/organizations/api_keys/{api_key_id}`

Update API Key

### Parameters

- `api_key_id: str`

  ID of the API key.

- `name: Optional[str]`

  Name of the API key.

- `status: Optional[Literal["active", "inactive", "archived"]]`

  Status of the API key.

  - `"active"`

  - `"inactive"`

  - `"archived"`

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
api_key = client.admin.api_keys.update(
    api_key_id="api_key_id",
)
print(api_key.id)
```

# Usage Report

## Retrieve Messages

`admin.usage_report.retrieve_messages(UsageReportRetrieveMessagesParams**kwargs)  -> MessagesUsageReport`

**get** `/v1/organizations/usage_report/messages`

Get Messages Usage Report

### Parameters

- `starting_at: Union[str, datetime]`

  Time buckets that start on or after this RFC 3339 timestamp will be returned.
  Each time bucket will be snapped to the start of the minute/hour/day in UTC.

- `api_key_ids: Optional[Sequence[str]]`

  Restrict usage returned to the specified API key ID(s).

- `bucket_width: Optional[Literal["1d", "1m", "1h"]]`

  Time granularity of the response data.

  - `"1d"`

  - `"1m"`

  - `"1h"`

- `context_window: Optional[List[Literal["0-200k", "200k-1M"]]]`

  Restrict usage returned to the specified context window(s).

  - `"0-200k"`

  - `"200k-1M"`

- `ending_at: Optional[Union[str, datetime, null]]`

  Time buckets that end before this RFC 3339 timestamp will be returned.

- `group_by: Optional[List[Literal["api_key_id", "workspace_id", "model", 4 more]]]`

  Group by any subset of the available options. Grouping by `speed` requires the `fast-mode-2026-02-01` beta header.

  - `"api_key_id"`

  - `"workspace_id"`

  - `"model"`

  - `"service_tier"`

  - `"context_window"`

  - `"inference_geo"`

  - `"speed"`

- `inference_geos: Optional[List[Literal["global", "us", "not_available"]]]`

  Restrict usage returned to the specified inference geo(s). Use `not_available` for models that do not support specifying `inference_geo`.

  - `"global"`

  - `"us"`

  - `"not_available"`

- `limit: Optional[int]`

  Maximum number of time buckets to return in the response.

  The default and max limits depend on `bucket_width`:
  • `"1d"`: Default of 7 days, maximum of 31 days
  • `"1h"`: Default of 24 hours, maximum of 168 hours
  • `"1m"`: Default of 60 minutes, maximum of 1440 minutes

- `models: Optional[Sequence[str]]`

  Restrict usage returned to the specified model(s).

- `page: Optional[Union[str, datetime, null]]`

  Optionally set to the `next_page` token from the previous response.

- `service_tiers: Optional[List[Literal["standard", "batch", "priority", 3 more]]]`

  Restrict usage returned to the specified service tier(s).

  - `"standard"`

  - `"batch"`

  - `"priority"`

  - `"priority_on_demand"`

  - `"flex"`

  - `"flex_discount"`

- `speeds: Optional[List[Literal["standard", "fast"]]]`

  Restrict usage returned to the specified speed(s) (research preview).
  Requires the `fast-mode-2026-02-01` beta header.

  - `"standard"`

  - `"fast"`

- `workspace_ids: Optional[Sequence[str]]`

  Restrict usage returned to the specified workspace ID(s).

- `anthropic_beta: Optional[Sequence[str]]`

  Optional header to specify the beta version(s) you want to use.

  To use multiple betas, use a comma separated list like `beta1,beta2` or specify the header multiple times for each beta.

### Returns

- `class MessagesUsageReport: …`

  - `data: List[Data]`

    - `ending_at: datetime`

      End of the time bucket (exclusive) in RFC 3339 format.

    - `results: List[DataResult]`

      List of usage items for this time bucket.  There may be multiple items if one or more `group_by[]` parameters are specified.

      - `api_key_id: Optional[str]`

        ID of the API key used. `null` if not grouping by API key or for usage in the Anthropic Console.

      - `cache_creation: DataResultCacheCreation`

        The number of input tokens for cache creation.

        - `ephemeral_1h_input_tokens: int`

          The number of input tokens used to create the 1 hour cache entry.

        - `ephemeral_5m_input_tokens: int`

          The number of input tokens used to create the 5 minute cache entry.

      - `cache_read_input_tokens: int`

        The number of input tokens read from the cache.

      - `context_window: Optional[Literal["0-200k", "200k-1M"]]`

        Context window used. `null` if not grouping by context window.

        - `"0-200k"`

        - `"200k-1M"`

      - `inference_geo: Optional[str]`

        Inference geo used matching requests' `inference_geo` parameter if set, otherwise the workspace's `default_inference_geo`.
        For models that do not support specifying `inference_geo` the value is `"not_available"`. Always `null` if not grouping by inference geo.

      - `model: Optional[str]`

        Model used. `null` if not grouping by model.

      - `output_tokens: int`

        The number of output tokens generated.

      - `server_tool_use: DataResultServerToolUse`

        Server-side tool usage metrics.

        - `web_search_requests: int`

          The number of web search requests made.

      - `service_tier: Optional[Literal["standard", "batch", "priority", 3 more]]`

        Service tier used. `null` if not grouping by service tier.

        - `"standard"`

        - `"batch"`

        - `"priority"`

        - `"priority_on_demand"`

        - `"flex"`

        - `"flex_discount"`

      - `speed: Optional[Literal["standard", "fast"]]`

        Speed of the usage (research preview). `null` if not grouping by speed.
        Only returned when the `fast-mode-2026-02-01` beta header is provided.

        - `"standard"`

        - `"fast"`

      - `uncached_input_tokens: int`

        The number of uncached input tokens processed.

      - `workspace_id: Optional[str]`

        ID of the Workspace used. `null` if not grouping by workspace or for the default workspace.

    - `starting_at: datetime`

      Start of the time bucket (inclusive) in RFC 3339 format.

  - `has_more: bool`

    Indicates if there are more results.

  - `next_page: Optional[datetime]`

    Token to provide in as `page` in the subsequent request to retrieve the next page of data.

### Example

```python
import os
from datetime import datetime
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
messages_usage_report = client.admin.usage_report.retrieve_messages(
    starting_at=datetime.fromisoformat("2024-10-30T23:58:27.427722"),
)
print(messages_usage_report.data)
```

## Retrieve Claude Code

`admin.usage_report.retrieve_claude_code(UsageReportRetrieveClaudeCodeParams**kwargs)  -> ClaudeCodeUsageReport`

**get** `/v1/organizations/usage_report/claude_code`

Retrieve daily aggregated usage metrics for Claude Code users.
Enables organizations to analyze developer productivity and build custom dashboards.

### Parameters

- `starting_at: str`

  UTC date in YYYY-MM-DD format. Returns metrics for this single day only.

- `limit: Optional[int]`

  Number of records per page (default: 20, max: 1000).

- `page: Optional[str]`

  Opaque cursor token from previous response's `next_page` field.

### Returns

- `class ClaudeCodeUsageReport: …`

  - `data: List[Data]`

    List of Claude Code usage records for the requested date.

    - `actor: DataActor`

      The user or API key that performed the Claude Code actions.

      - `class DataActorUserActor: …`

        - `email_address: str`

          Email address of the user who performed Claude Code actions.

        - `type: Literal["user_actor"]`

          - `"user_actor"`

      - `class DataActorAPIActor: …`

        - `api_key_name: str`

          Name of the API key used to perform Claude Code actions.

        - `type: Literal["api_actor"]`

          - `"api_actor"`

    - `core_metrics: DataCoreMetrics`

      Core productivity metrics measuring Claude Code usage and impact.

      - `commits_by_claude_code: int`

        Number of git commits created through Claude Code's commit functionality.

      - `lines_of_code: DataCoreMetricsLinesOfCode`

        Statistics on code changes made through Claude Code.

        - `added: int`

          Total number of lines of code added across all files by Claude Code.

        - `removed: int`

          Total number of lines of code removed across all files by Claude Code.

      - `num_sessions: int`

        Number of distinct Claude Code sessions initiated by this actor.

      - `pull_requests_by_claude_code: int`

        Number of pull requests created through Claude Code's PR functionality.

    - `customer_type: Literal["api", "subscription"]`

      Type of customer account (api for API customers, subscription for Pro/Team customers).

      - `"api"`

      - `"subscription"`

    - `date: datetime`

      UTC date for the usage metrics in YYYY-MM-DD format.

    - `model_breakdown: List[DataModelBreakdown]`

      Token usage and cost breakdown by AI model used.

      - `estimated_cost: DataModelBreakdownEstimatedCost`

        Estimated cost for using this model

        - `amount: int`

          Estimated cost amount in minor currency units (e.g., cents for USD).

        - `currency: str`

          Currency code for the estimated cost (e.g., 'USD').

      - `model: str`

        Name of the AI model used for Claude Code interactions.

      - `tokens: DataModelBreakdownTokens`

        Token usage breakdown for this model

        - `cache_creation: int`

          Number of cache creation tokens consumed by this model.

        - `cache_read: int`

          Number of cache read tokens consumed by this model.

        - `input: int`

          Number of input tokens consumed by this model.

        - `output: int`

          Number of output tokens generated by this model.

    - `organization_id: str`

      ID of the organization that owns the Claude Code usage.

    - `terminal_type: str`

      Type of terminal or environment where Claude Code was used.

    - `tool_actions: Dict[str, DataToolActions]`

      Breakdown of tool action acceptance and rejection rates by tool type.

      - `accepted: int`

        Number of tool action proposals that the user accepted.

      - `rejected: int`

        Number of tool action proposals that the user rejected.

    - `subscription_type: Optional[Literal["enterprise", "team"]]`

      Subscription tier for subscription customers. `null` for API customers.

      - `"enterprise"`

      - `"team"`

  - `has_more: bool`

    True if there are more records available beyond the current page.

  - `next_page: Optional[str]`

    Opaque cursor token for fetching the next page of results, or null if no more pages are available.

### Example

```python
import os
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
claude_code_usage_report = client.admin.usage_report.retrieve_claude_code(
    starting_at="7321-69-10",
)
print(claude_code_usage_report.data)
```

## Domain Types

### Claude Code Usage Report

- `class ClaudeCodeUsageReport: …`

  - `data: List[Data]`

    List of Claude Code usage records for the requested date.

    - `actor: DataActor`

      The user or API key that performed the Claude Code actions.

      - `class DataActorUserActor: …`

        - `email_address: str`

          Email address of the user who performed Claude Code actions.

        - `type: Literal["user_actor"]`

          - `"user_actor"`

      - `class DataActorAPIActor: …`

        - `api_key_name: str`

          Name of the API key used to perform Claude Code actions.

        - `type: Literal["api_actor"]`

          - `"api_actor"`

    - `core_metrics: DataCoreMetrics`

      Core productivity metrics measuring Claude Code usage and impact.

      - `commits_by_claude_code: int`

        Number of git commits created through Claude Code's commit functionality.

      - `lines_of_code: DataCoreMetricsLinesOfCode`

        Statistics on code changes made through Claude Code.

        - `added: int`

          Total number of lines of code added across all files by Claude Code.

        - `removed: int`

          Total number of lines of code removed across all files by Claude Code.

      - `num_sessions: int`

        Number of distinct Claude Code sessions initiated by this actor.

      - `pull_requests_by_claude_code: int`

        Number of pull requests created through Claude Code's PR functionality.

    - `customer_type: Literal["api", "subscription"]`

      Type of customer account (api for API customers, subscription for Pro/Team customers).

      - `"api"`

      - `"subscription"`

    - `date: datetime`

      UTC date for the usage metrics in YYYY-MM-DD format.

    - `model_breakdown: List[DataModelBreakdown]`

      Token usage and cost breakdown by AI model used.

      - `estimated_cost: DataModelBreakdownEstimatedCost`

        Estimated cost for using this model

        - `amount: int`

          Estimated cost amount in minor currency units (e.g., cents for USD).

        - `currency: str`

          Currency code for the estimated cost (e.g., 'USD').

      - `model: str`

        Name of the AI model used for Claude Code interactions.

      - `tokens: DataModelBreakdownTokens`

        Token usage breakdown for this model

        - `cache_creation: int`

          Number of cache creation tokens consumed by this model.

        - `cache_read: int`

          Number of cache read tokens consumed by this model.

        - `input: int`

          Number of input tokens consumed by this model.

        - `output: int`

          Number of output tokens generated by this model.

    - `organization_id: str`

      ID of the organization that owns the Claude Code usage.

    - `terminal_type: str`

      Type of terminal or environment where Claude Code was used.

    - `tool_actions: Dict[str, DataToolActions]`

      Breakdown of tool action acceptance and rejection rates by tool type.

      - `accepted: int`

        Number of tool action proposals that the user accepted.

      - `rejected: int`

        Number of tool action proposals that the user rejected.

    - `subscription_type: Optional[Literal["enterprise", "team"]]`

      Subscription tier for subscription customers. `null` for API customers.

      - `"enterprise"`

      - `"team"`

  - `has_more: bool`

    True if there are more records available beyond the current page.

  - `next_page: Optional[str]`

    Opaque cursor token for fetching the next page of results, or null if no more pages are available.

### Messages Usage Report

- `class MessagesUsageReport: …`

  - `data: List[Data]`

    - `ending_at: datetime`

      End of the time bucket (exclusive) in RFC 3339 format.

    - `results: List[DataResult]`

      List of usage items for this time bucket.  There may be multiple items if one or more `group_by[]` parameters are specified.

      - `api_key_id: Optional[str]`

        ID of the API key used. `null` if not grouping by API key or for usage in the Anthropic Console.

      - `cache_creation: DataResultCacheCreation`

        The number of input tokens for cache creation.

        - `ephemeral_1h_input_tokens: int`

          The number of input tokens used to create the 1 hour cache entry.

        - `ephemeral_5m_input_tokens: int`

          The number of input tokens used to create the 5 minute cache entry.

      - `cache_read_input_tokens: int`

        The number of input tokens read from the cache.

      - `context_window: Optional[Literal["0-200k", "200k-1M"]]`

        Context window used. `null` if not grouping by context window.

        - `"0-200k"`

        - `"200k-1M"`

      - `inference_geo: Optional[str]`

        Inference geo used matching requests' `inference_geo` parameter if set, otherwise the workspace's `default_inference_geo`.
        For models that do not support specifying `inference_geo` the value is `"not_available"`. Always `null` if not grouping by inference geo.

      - `model: Optional[str]`

        Model used. `null` if not grouping by model.

      - `output_tokens: int`

        The number of output tokens generated.

      - `server_tool_use: DataResultServerToolUse`

        Server-side tool usage metrics.

        - `web_search_requests: int`

          The number of web search requests made.

      - `service_tier: Optional[Literal["standard", "batch", "priority", 3 more]]`

        Service tier used. `null` if not grouping by service tier.

        - `"standard"`

        - `"batch"`

        - `"priority"`

        - `"priority_on_demand"`

        - `"flex"`

        - `"flex_discount"`

      - `speed: Optional[Literal["standard", "fast"]]`

        Speed of the usage (research preview). `null` if not grouping by speed.
        Only returned when the `fast-mode-2026-02-01` beta header is provided.

        - `"standard"`

        - `"fast"`

      - `uncached_input_tokens: int`

        The number of uncached input tokens processed.

      - `workspace_id: Optional[str]`

        ID of the Workspace used. `null` if not grouping by workspace or for the default workspace.

    - `starting_at: datetime`

      Start of the time bucket (inclusive) in RFC 3339 format.

  - `has_more: bool`

    Indicates if there are more results.

  - `next_page: Optional[datetime]`

    Token to provide in as `page` in the subsequent request to retrieve the next page of data.

# Cost Report

## Retrieve

`admin.cost_report.retrieve(CostReportRetrieveParams**kwargs)  -> CostReport`

**get** `/v1/organizations/cost_report`

Get Cost Report

### Parameters

- `starting_at: Union[str, datetime]`

  Time buckets that start on or after this RFC 3339 timestamp will be returned.
  Each time bucket will be snapped to the start of the minute/hour/day in UTC.

- `bucket_width: Optional[Literal["1d"]]`

  Time granularity of the response data.

  - `"1d"`

- `ending_at: Optional[Union[str, datetime, null]]`

  Time buckets that end before this RFC 3339 timestamp will be returned.

- `group_by: Optional[List[Literal["workspace_id", "description"]]]`

  Group by any subset of the available options.

  - `"workspace_id"`

  - `"description"`

- `limit: Optional[int]`

  Maximum number of time buckets to return in the response.

- `page: Optional[Union[str, datetime, null]]`

  Optionally set to the `next_page` token from the previous response.

- `anthropic_beta: Optional[Sequence[str]]`

  Optional header to specify the beta version(s) you want to use.

  To use multiple betas, use a comma separated list like `beta1,beta2` or specify the header multiple times for each beta.

### Returns

- `class CostReport: …`

  - `data: List[Data]`

    - `ending_at: str`

      End of the time bucket (exclusive) in RFC 3339 format.

    - `results: List[DataResult]`

      List of cost items for this time bucket. There may be multiple items if one or more `group_by[]` parameters are specified.

      - `amount: str`

        Cost amount in lowest currency units (e.g. cents) as a decimal string. For example, `"123.45"` in `"USD"` represents `$1.23`.

      - `context_window: Optional[Literal["0-200k", "200k-1M"]]`

        Input context window used. `null` if not grouping by description or for non-token costs.

        - `"0-200k"`

        - `"200k-1M"`

      - `cost_type: Optional[Literal["tokens", "web_search", "code_execution"]]`

        Type of cost. `null` if not grouping by description.

        - `"tokens"`

        - `"web_search"`

        - `"code_execution"`

      - `currency: str`

        Currency code for the cost amount. Currently always `"USD"`.

      - `description: Optional[str]`

        Description of the cost item. `null` if not grouping by description.

      - `inference_geo: Optional[str]`

        Inference geo used matching requests' `inference_geo` parameter if set, otherwise the workspace's `default_inference_geo`.
        For models that do not support specifying `inference_geo` the value is `"not_available"`. Always `null` if not grouping by inference geo.

      - `model: Optional[str]`

        Model name used. `null` if not grouping by description or for non-token costs.

      - `service_tier: Optional[Literal["standard", "batch"]]`

        Service tier used. `null` if not grouping by description or for non-token costs.

        - `"standard"`

        - `"batch"`

      - `speed: Optional[Literal["standard", "fast"]]`

        Speed used (research preview). `null` if not grouping by speed, or for non-token costs.
        Only returned when the `fast-mode-2026-02-01` beta header is provided.

        - `"standard"`

        - `"fast"`

      - `token_type: Optional[Literal["uncached_input_tokens", "output_tokens", "cache_read_input_tokens", 2 more]]`

        Type of token. `null` if not grouping by description or for non-token costs.

        - `"uncached_input_tokens"`

        - `"output_tokens"`

        - `"cache_read_input_tokens"`

        - `"cache_creation.ephemeral_1h_input_tokens"`

        - `"cache_creation.ephemeral_5m_input_tokens"`

      - `workspace_id: Optional[str]`

        ID of the Workspace this cost is associated with. `null` if not grouping by workspace or for the default workspace.

    - `starting_at: str`

      Start of the time bucket (inclusive) in RFC 3339 format.

  - `has_more: bool`

    Indicates if there are more results.

  - `next_page: Optional[datetime]`

    Token to provide in as `page` in the subsequent request to retrieve the next page of data.

### Example

```python
import os
from datetime import datetime
from anthropic_admin import AnthropicAdmin

client = AnthropicAdmin(
    admin_api_key=os.environ.get("ANTHROPIC_ADMIN_API_KEY"),  # This is the default and can be omitted
)
cost_report = client.admin.cost_report.retrieve(
    starting_at=datetime.fromisoformat("2024-10-30T23:58:27.427722"),
)
print(cost_report.data)
```

## Domain Types

### Cost Report

- `class CostReport: …`

  - `data: List[Data]`

    - `ending_at: str`

      End of the time bucket (exclusive) in RFC 3339 format.

    - `results: List[DataResult]`

      List of cost items for this time bucket. There may be multiple items if one or more `group_by[]` parameters are specified.

      - `amount: str`

        Cost amount in lowest currency units (e.g. cents) as a decimal string. For example, `"123.45"` in `"USD"` represents `$1.23`.

      - `context_window: Optional[Literal["0-200k", "200k-1M"]]`

        Input context window used. `null` if not grouping by description or for non-token costs.

        - `"0-200k"`

        - `"200k-1M"`

      - `cost_type: Optional[Literal["tokens", "web_search", "code_execution"]]`

        Type of cost. `null` if not grouping by description.

        - `"tokens"`

        - `"web_search"`

        - `"code_execution"`

      - `currency: str`

        Currency code for the cost amount. Currently always `"USD"`.

      - `description: Optional[str]`

        Description of the cost item. `null` if not grouping by description.

      - `inference_geo: Optional[str]`

        Inference geo used matching requests' `inference_geo` parameter if set, otherwise the workspace's `default_inference_geo`.
        For models that do not support specifying `inference_geo` the value is `"not_available"`. Always `null` if not grouping by inference geo.

      - `model: Optional[str]`

        Model name used. `null` if not grouping by description or for non-token costs.

      - `service_tier: Optional[Literal["standard", "batch"]]`

        Service tier used. `null` if not grouping by description or for non-token costs.

        - `"standard"`

        - `"batch"`

      - `speed: Optional[Literal["standard", "fast"]]`

        Speed used (research preview). `null` if not grouping by speed, or for non-token costs.
        Only returned when the `fast-mode-2026-02-01` beta header is provided.

        - `"standard"`

        - `"fast"`

      - `token_type: Optional[Literal["uncached_input_tokens", "output_tokens", "cache_read_input_tokens", 2 more]]`

        Type of token. `null` if not grouping by description or for non-token costs.

        - `"uncached_input_tokens"`

        - `"output_tokens"`

        - `"cache_read_input_tokens"`

        - `"cache_creation.ephemeral_1h_input_tokens"`

        - `"cache_creation.ephemeral_5m_input_tokens"`

      - `workspace_id: Optional[str]`

        ID of the Workspace this cost is associated with. `null` if not grouping by workspace or for the default workspace.

    - `starting_at: str`

      Start of the time bucket (inclusive) in RFC 3339 format.

  - `has_more: bool`

    Indicates if there are more results.

  - `next_page: Optional[datetime]`

    Token to provide in as `page` in the subsequent request to retrieve the next page of data.