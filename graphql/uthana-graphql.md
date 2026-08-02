---
generated: '2026-07-21'
method: searched
source: https://uthana.com/graphql (introspection, unauthenticated)
---

# Uthana GraphQL API

Uthana's API is a single GraphQL endpoint at `https://uthana.com/graphql`, documented at
https://uthana.com/docs/api. Introspection is publicly enabled; the schema in
`uthana-schema.graphql` (SDL) and `uthana-introspection.json` (raw introspection result)
was captured live on 2026-07-21.

- **Endpoint:** `https://uthana.com/graphql`
- **Auth:** HTTP Basic — API key as username, empty password (official Python/JS clients handle this)
- **Surface:** 71 named types, 22 root queries, 33 mutations
- **Reference docs:** https://uthana.com/docs/api/graphql

## Root queries (22)

`org`, `user`, `payg_prices`, `payg_ledger`, `locomotion_styles`, `character`, `characters`,
`motion`, `motions`, `label_search`, `job`, `jobs`, `motion_downloads`, `motion_download_summary`,
`motion_download_allowed`, `motion_rating`, `motion_favorite`, `deepgram_token`, `sunburst`,
`subscription_plans`, `subscription`, `character_rtcc_retarget`

## Mutations (33)

Generation: `create_text_to_motion`, `create_text_to_motion_job`, `create_video_to_motion`,
`create_locomotion`, `create_stitched_motion`, `create_enhanced_stitched_motion`,
`create_motion_from_gltf`, `trim_and_loop_motion`, `create_looped_motion`, `enhance_prompt`,
`rtcc_text_generate`, `create_rtcc_session`

Characters: `create_character`, `create_character_from_image`, `create_image_from_text`,
`create_image_from_image`, `update_character`

Assets & feedback: `update_motion`, `rate_motion`, `favorite_motion`, `create_label`,
`update_label`, `create_share_link`

Account & billing: `update_user`, `reset_user_apikey`, `create_subscription`,
`change_subscription`, `cancel_subscription`, `resume_subscription`,
`create_billing_portal_session`, `payg_create_payment_session`, `payg_create_setup_session`,
`payg_update_auto_recharge`

## Companion REST download endpoints

Motion/character files (FBX, GLB, BVH, Unitree G1 CSV) are downloaded over REST, same host,
same Basic auth (docs: https://uthana.com/docs/api/capabilities/downloading-motion):

- `GET /motion/file/motion_viewer/{characterId}/{motionId}/{fbx|glb|bvh}/{filename}` — motion with character mesh
- `GET /motion/animation/{characterId}/{motionId}/glb/{filename}` — animation track only (does not count against download quota)
- `GET /motion/bundle/{characterId}/{motionId}/{character.glb|character.fbx}` — bundle (fixed filename)
