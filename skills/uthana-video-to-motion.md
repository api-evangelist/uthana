---
name: Generate a motion from a reference video
description: Turn a 2D reference video into character-ready motion with the async
  video-to-motion pipeline (Job polling) on the Uthana GraphQL API.
api: graphql/uthana-schema.graphql
operations: [create_video_to_motion, job, jobs]
generated: '2026-07-21'
method: generated
source: https://uthana.com/docs/api + graphql/uthana-schema.graphql
---

# Generate a motion from a reference video

## Auth

Same as all Uthana calls: HTTP Basic, API key as username, empty password, against
`POST https://uthana.com/graphql`.

## Steps

1. Call the `create_video_to_motion` mutation with your reference video. The default
   model is `video-to-motion-2.0`; `video-to-motion-2.1` adds post-processing refinements
   and returns refined motion IDs. (`video-to-motion-v2` is a backward-compatible alias;
   `video-to-motion-v1` was removed 2026.)
2. Video-to-motion is asynchronous: it returns a `Job`. Poll the `job(id:)` query and
   inspect `status`, `est_processing_time`, and `result` until the motion id is available.
3. Download the finished motion exactly as in the text-to-motion skill (REST
   `/motion/file/...` or quota-free `/motion/animation/...`).

## Rules

- Pay-as-you-go billing is per second of generated output ($0.05/s for 2.0/2.1) — check
  `payg_ledger` / `org` wallet fields when running at volume.
- Retargeting to the addressed character happens automatically at download time.
