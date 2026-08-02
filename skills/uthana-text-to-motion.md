---
name: Generate a motion from text and download it
description: Create an AI-generated 3D character animation from a text prompt with the
  Uthana GraphQL API, then download it as FBX/GLB.
api: graphql/uthana-schema.graphql
operations: [create_text_to_motion, create_text_to_motion_job, job, motion_download_summary]
generated: '2026-07-21'
method: generated
source: https://uthana.com/docs/api + graphql/uthana-schema.graphql
---

# Generate a motion from text and download it

## Auth

HTTP Basic with your API key as the username, empty password (`curl -u $API_KEY:`).
Endpoint: `POST https://uthana.com/graphql`. Verify first:

```bash
curl 'https://uthana.com/graphql' -u $API_KEY: -H "Content-Type: application/json" \
  -d '{"query": "{ __typename }"}'   # expect {"data":{"__typename":"Query"}}
```

## Steps

1. Pick a character. Use the published default character **Tar** (`cXi2eAP19XwQ`) if you
   have not uploaded one.
2. Generate. For fast models call the `create_text_to_motion` mutation:
   ```graphql
   mutation { create_text_to_motion(prompt: "A person walking down the street") { motion { id name } } }
   ```
   For the `text-to-motion-3.0` diffusion model (pay-as-you-go plans), call
   `create_text_to_motion_job(prompt: ..., model: "text-to-motion-3.0")` — it returns a
   `Job`; poll the `job(id:)` query until `status` is complete (use `est_processing_time`
   to pace polling).
3. Check quota before downloading: query `motion_download_summary` /
   `motion_download_allowed` — downloads are metered per plan in seconds.
4. Download over REST (same Basic auth). With character mesh:
   `GET https://uthana.com/motion/file/motion_viewer/{characterId}/{motionId}/{fbx|glb}/motion.{ext}`.
   Animation-only (does NOT count against download quota):
   `GET https://uthana.com/motion/animation/{characterId}/{motionId}/glb/{filename}`.

## Rules

- No idempotency keys exist; do not blind-retry mutations that bill credits.
- Errors arrive in the GraphQL `errors[]` envelope; unauthenticated account queries
  resolve to `null` rather than HTTP 401 — always run the auth check in step 0.
- Prefer the official clients: Python `uthana` (`client.ttm.create`, async; `_sync`
  variants exist) or `@uthana/client` (`client.ttm.create`).
