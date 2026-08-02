---
name: Configure an AI calling agent and review its calls
description: Create and update TalkPush AI voice agents, list their calls with transcripts and extractions, and audit configuration changes.
api: openapi/talkpush-openapi.json
operations:
- GET /agents                              # list agents
- POST /agents                             # create agent
- GET /agents/{id}                         # get agent
- PUT /agents/{id}                         # update agent
- DELETE /agents/{id}                      # delete agent
- GET /agents/{agent_id}/calls             # list agent calls
- GET /agents/{agent_id}/changelog         # agent audit log
- GET /company/candidate_attributes        # attribute keys for extractions
---

# Configure an AI calling agent and review its calls

TalkPush AI agents (providers such as `elevenlabs`, `vapi`) conduct automated
candidate calls/interviews and extract structured data into candidate
attributes.

## Auth
Company `api_key` query parameter on every call (see
`authentication/talkpush-authentication.yml`).

## Steps
1. **Discover attribute keys** — `GET /company/candidate_attributes` to get the
   keys you will reference in `data_extractions`.
2. **Create** — `POST /agents`. In the create request, `data_extractions` uses
   the candidate-attribute **key** string (e.g. `"english_level"`).
3. **Read / list** — `GET /agents` (filter by category/provider/active) and
   `GET /agents/{id}` for the full prompt, provider info, and extractions.
4. **Update** — `PUT /agents/{id}`. All fields optional; supplying
   `data_extractions` or `prompt` **replaces** them entirely (not a merge). On
   update, `data_extractions` uses `{ "id": "..." }` objects.
5. **Delete** — `DELETE /agents/{id}` returns 204.
6. **Review calls** — `GET /agents/{agent_id}/calls` for records with
   transcript, extractions, interview insights, and a recording link
   (filter by `status` and `start_date`+`end_date`).
7. **Audit** — `GET /agents/{agent_id}/changelog`; entries show who changed what
   (`changed_by` is a manager email, or `"TalkPush API"` for API changes).

## Error handling
See `errors/talkpush-problem-types.yml`. 404 for unknown agent ids.
