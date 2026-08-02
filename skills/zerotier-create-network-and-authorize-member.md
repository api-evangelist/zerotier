---
name: Create a ZeroTier network and authorize a member
description: Provision a new ZeroTier virtual network via the Central API, then authorize a node that has joined it.
api: openapi/zerotier-central-openapi-original.json
operations: [newNetwork, getNetworkMemberList, getNetworkMember, updateNetworkMember]
---

# Create a ZeroTier network and authorize a member

Use the ZeroTier Central API (base `https://api.zerotier.com/api/v1`). Authenticate every request
with the header `Authorization: token <API_TOKEN>` (legacy) or `Authorization: Bearer <API_KEY>`
(new Central service-account token).

## Steps

1. **Create the network** — `POST /network` (`newNetwork`) with a JSON body containing the desired
   `config` (name, IPv4/IPv6 assignment mode, routes). The response returns the new `id`
   (16-hex-digit network ID). Save it as `networkID`.
2. **Have the node join** — out of band, the target device runs `zerotier-cli join <networkID>`.
   Until authorized it will appear as an unauthorized member.
3. **Find the member** — `GET /network/{networkID}/member` (`getNetworkMemberList`) and locate the
   node by its 10-hex-digit `nodeId` / member ID. Optionally `GET /network/{networkID}/member/{memberID}`
   (`getNetworkMember`) to inspect it.
4. **Authorize the member** — `POST /network/{networkID}/member/{memberID}` (`updateNetworkMember`)
   with body `{"config": {"authorized": true}}`.

## Rules
- Errors are HTTP-status-only (401 bad token, 403 forbidden, 404 unknown network/member) — there is
  no problem+json body. See `errors/zerotier-problem-types.yml`.
- There is no idempotency-key; updates are last-write-wins on the resource ID.
- IDs are opaque hex strings (network = 16 hex, member = 10 hex).
