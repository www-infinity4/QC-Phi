# QC Phi owner repair architecture

QC Phi scans **Infinity Phi** and **Omni Phi** and emits repair tickets.

## Owner-only Take Action

The browser must never contain GitHub write credentials or GPT/API secrets. The Take Action button hands a QC ticket to a private authenticated repair worker. That worker should:

1. Resolve the authenticated wallet/user identity.
2. Verify that identity owns or has explicit write permission for the target repository.
3. Read the failing repository and deployed evidence.
4. Ask GPT for a minimal patch.
5. Apply the patch through a narrowly scoped GitHub App installation or equivalent repository authorization.
6. Record commit SHA, files changed, QC ticket ID and owner/site identity.
7. Rerun QC. Stop if the repair causes a regression.

Generated customer websites get the same **wallet protocol and action-receipt protocol**, not the platform owner's credentials.

## Token identity

A ledger entry should distinguish the currency family from where it was earned:

```json
{
  "currency": "Infinity",
  "siteId": "stable-generated-site-id",
  "ownerId": "wallet-authenticated-owner-id",
  "userId": "wallet-user-id",
  "generator": "search|share|watch|site-action",
  "amount": 1,
  "createdAt": "ISO timestamp",
  "receiptId": "unique id"
}
```

This allows one unified wallet to display Infinity tokens earned across different generated sites while preserving the identity of the site/action that created them. Daily issuance limits must be enforced by the trusted ledger service, not browser code.
