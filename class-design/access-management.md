# Access Management System (RBAC)

## Prompt

Design a role-based access management system. Users are assigned roles; roles grant permissions; the system answers "does this user have permission X?"

## Clarifying Questions

- Role hierarchy (admin > editor > viewer)?
- Are permissions per-resource or system-wide?
- Multiple roles per user?
- Time-bounded permissions / expirations?
- Audit log of grants and revokes?
- Performance target — synchronous check on every request?

## Class Skeleton

```
Permissions (Enum)             # READ, WRITE, DELETE, ADMIN, ...

User (Class)
    id
    name
    roles: Set[Role]

Role (Class)
    name
    permissions: Set[Permissions]

AccessManager (Class)
    users: Dict[id, User]
    roles: Dict[name, Role]
    createRole(name, permissions)
    _getRole(name)
    grantPermission(roleName, permission)
    assignRole(userId, roleName)
    revokeRole(userId, roleName)
    hasPermission(userId, permission) -> bool
```

## Design Notes

- **Permission resolution**: `hasPermission` should iterate all of the user's roles and union their permissions — short-circuit on first match.
- **Caching**: for hot paths, cache `userId → Set[Permissions]` and invalidate on role/permission changes.
- **Audit**: wrap mutating methods (`grantPermission`, `assignRole`, `revokeRole`) with logging — useful for compliance.
- **Extension points**:
  - **Resource-scoped permissions** (`hasPermission(userId, permission, resourceId)`) — common follow-up.
  - **Role hierarchy** — child roles inherit parent permissions.
  - **ABAC** (attribute-based) — extend with policy evaluators if RBAC becomes too coarse.
