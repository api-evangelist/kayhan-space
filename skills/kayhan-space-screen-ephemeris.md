---
name: Screen an ephemeris for conjunctions
description: >-
  Upload an operator ephemeris to Satcat, operationalize it, run an on-orbit
  conjunction screening, and review the resulting conjunctions/CDMs and plot.
api: openapi/kayhan-space-satcat-openapi-original.json
operations:
- create_ephemeris_ephemerides_post
- operationalize_ephemeris_ephemerides__id__operationalize_put
- create_screening_screenings_post
- get_conjunction_plot_screenings_conjunction_plot_get
- list_conjunction_messages_conjunction_messages_get
---

# Screen an ephemeris for conjunctions

Use the Satcat Service API to screen a satellite's predicted trajectory against
the catalog and review close approaches.

## Auth
Get an OAuth2 access token from the client-credentials flow
(`tokenUrl` `oauth/token`, base `https://api.satcat.com/api/satcat`) and send it
as `Authorization: Bearer <token>` on every call. Credentials (client id +
secret) are created in the Satcat Control Center. See
`conventions/kayhan-space-conventions.yml`.

## Steps
1. **Upload the ephemeris** — `create_ephemeris_ephemerides_post`
   (`POST /ephemerides`) with the trajectory in a supported file format.
2. **Operationalize it** — `operationalize_ephemeris_ephemerides__id__operationalize_put`
   (`PUT /ephemerides/{id}/operationalize`) so it becomes the object's current
   operational ephemeris used for screening.
3. **Create a screening** — `create_screening_screenings_post`
   (`POST /screenings`) against the operational ephemeris.
4. **Review conjunctions** — `list_conjunction_messages_conjunction_messages_get`
   (`GET /conjunction_messages`) to pull the CDMs produced, then
   `get_conjunction_plot_screenings_conjunction_plot_get`
   (`GET /screenings/conjunction_plot`) for the visualization.

## Rules
- Requests are validated; a `422` returns `{"detail": [...]}` naming the invalid
  fields — fix and resubmit (`errors/kayhan-space-problem-types.yml`).
- No idempotency-key contract is documented; do not assume safe silent retries
  on POST.
- List endpoints paginate with an `offset` query parameter.
