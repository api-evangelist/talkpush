---
name: Source and manage a recruiting lead
description: Create a candidate lead in a TalkPush campaign, find leads by search, then update, comment, re-status, move, or reassign them.
api: openapi/talkpush-openapi.json
operations:
- POST /campaigns/{id}/campaign_invitations   # create_lead
- GET /campaign_invitations                    # search leads
- PATCH /campaign_invitations/{id}             # update lead + labels
- POST /campaign_invitations/{id}/comments     # add comment
- PUT /campaign_invitations/{id}/{status}      # change status
- PUT /campaign_invitations/{id}/move          # move to another campaign
- PUT /campaign_invitations/{id}/reassign      # reassign recruiter
---

# Source and manage a recruiting lead

A "lead" in TalkPush is a candidate's application to a campaign — internally a
`campaign_invitation`.

## Auth
Every call requires the company `api_key` as a **query parameter** against your
workspace base URL `https://{subdomain}.talkpush.com/api/talkpush_services`.
See `authentication/talkpush-authentication.yml`.

## Steps
1. **Create the lead** — `POST /campaigns/{id}/campaign_invitations` with the
   candidate payload, where `{id}` is the target campaign.
2. **Find leads** — `GET /campaign_invitations` using either the free-text
   `query` parameter or a combination of advanced-search parameters.
3. **Update details / labels** — `PATCH /campaign_invitations/{id}`. To change
   labels, send `candidate.labels` with **both** an `add` and a `remove` array
   (empty array if unused). Every name must already exist as a company label
   (create first via `POST /company/labels`), or the whole request fails with
   **422** and nothing changes.
4. **Comment** — `POST /campaign_invitations/{id}/comments` to add a note.
5. **Change status** — `PUT /campaign_invitations/{id}/{status}`.
6. **Move / reassign** — `PUT /campaign_invitations/{id}/move` to another
   campaign (cannot target another campaign's inbox folder);
   `PUT /campaign_invitations/{id}/reassign` to change the assigned recruiter.

## Error handling
409 = uniqueness conflict; 422 = validation error (offending fields listed);
401/403 = bad or unauthorized api_key. See `errors/talkpush-problem-types.yml`.
No idempotency key is supported — retries are not deduplicated by the server.
