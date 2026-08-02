---
name: Manage a ZeroTier organization and its members
description: Inspect an organization, list its members, and invite a new user by email via the Central API.
api: openapi/zerotier-central-openapi-original.json
operations: [getOrganization, getOrganizationMembers, inviteUserByEmail, getOrganizationInvitationList]
---

# Manage a ZeroTier organization and its members

Authenticate with `Authorization: token <API_TOKEN>` (or Bearer). Base `https://api.zerotier.com/api/v1`.

## Steps

1. **Get the organization** — `GET /org` (`getOrganization`) to retrieve the caller's organization,
   including its `id`.
2. **List members** — `GET /org/{orgID}/user` (`getOrganizationMembers`) to enumerate current
   organization members and their roles.
3. **Invite a user** — `POST /org-invitation` (`inviteUserByEmail`) with the invitee's email address
   to send an organization invitation.
4. **Track invitations** — `GET /org-invitation` (`getOrganizationInvitationList`) to see the status
   of outstanding invitations.

## Rules
- Organization endpoints require an org-scoped token (Essential+ plan / service account).
- 401 = invalid token, 403 = insufficient permission, 404 = unknown org/invite.
- See `authentication/zerotier-authentication.yml` and `conventions/zerotier-conventions.yml`.
