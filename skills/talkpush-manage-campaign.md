---
name: Create and manage a recruiting campaign
description: Create a TalkPush campaign, inspect its questions and folders, associate custom folders, and archive or reactivate it.
api: openapi/talkpush-openapi.json
operations:
- POST /campaigns                                        # create_campaign
- GET /campaigns                                         # list active campaigns
- GET /campaigns/{id}                                    # get campaign
- PUT /campaigns/{id}                                    # update campaign
- GET /campaigns/{id}/questions                          # get questions
- GET /campaigns/{id}/folders                            # get folders
- POST /campaigns/{campaign_id}/folders/{folder_id}      # associate folder
- PUT /campaigns/{id}/archive                            # archive
- PUT /campaigns/{id}/activate                           # activate
---

# Create and manage a recruiting campaign

A campaign is a recruiting process (typically a `job_application`). A common
use case is auto-creating a campaign when a job requisition is opened in an
integrated ATS.

## Auth
Company `api_key` query parameter on every call (see
`authentication/talkpush-authentication.yml`).

## Steps
1. **Create** — `POST /campaigns`. Note: permission settings, questions,
   messages, smart filters and notifications are configured in the CRM, not via
   the API.
2. **List / read** — `GET /campaigns` (filter by `type`; defaults to
   `job_application` only) and `GET /campaigns/{id}`.
3. **Update** — `PUT /campaigns/{id}`.
4. **Inspect** — `GET /campaigns/{id}/questions` (text/short_text/multiple-choice
   only) and `GET /campaigns/{id}/folders`.
5. **Custom folders** — create a company folder with `POST /company/folders`,
   then associate it via `POST /campaigns/{campaign_id}/folders/{folder_id}`.
6. **Lifecycle** — `PUT /campaigns/{id}/archive` and
   `PUT /campaigns/{id}/activate`; browse archived campaigns with
   `GET /campaigns/archive`.

## Error handling
See `errors/talkpush-problem-types.yml`. 404 if the campaign id is unknown.
