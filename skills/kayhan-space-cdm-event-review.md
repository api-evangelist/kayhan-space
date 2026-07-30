---
name: Ingest CDMs and review a conjunction event
description: >-
  Upload third-party CCSDS Conjunction Data Messages to Satcat, attach them to a
  conjunction event, and review the event's CDMs to support a mitigation
  decision.
api: openapi/kayhan-space-satcat-openapi-original.json
operations:
- upload_cdms_conjunction_messages_post
- list_events_events_get
- add_cdm_to_event_events__event_key__ccsds_cdm_post
- get_conjunction_message_by_id_conjunction_messages__cdm_id__get
---

# Ingest CDMs and review a conjunction event

Bring external CDMs into Satcat and work them as a conjunction event.

## Auth
OAuth2 bearer token (client-credentials flow), base
`https://api.satcat.com/api/satcat`. See
`conventions/kayhan-space-conventions.yml`.

## Steps
1. **Upload CDMs** — `upload_cdms_conjunction_messages_post`
   (`POST /conjunction_messages`) with CCSDS-format CDM data.
2. **Find the event** — `list_events_events_get` (`GET /events`) and select the
   conjunction event by its `event_key`.
3. **Attach a CDM to the event** — `add_cdm_to_event_events__event_key__ccsds_cdm_post`
   (`POST /events/{event_key}/ccsds_cdm`).
4. **Inspect a specific CDM** — `get_conjunction_message_by_id_conjunction_messages__cdm_id__get`
   (`GET /conjunction_messages/{cdm_id}`) to read miss distance, probability of
   collision, and covariance for the decision.

## Rules
- CDMs follow the CCSDS Conjunction Data Message standard
  (`conformance/kayhan-space-conformance.yml`).
- Validation failures return `422` with a `detail` array.
- Mitigation intent for an event is captured through the Events / Mitigation
  endpoints once a maneuver decision is made.
