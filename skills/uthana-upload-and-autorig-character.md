---
name: Upload and auto-rig a character
description: Upload a 3D character (FBX/glTF) to Uthana, auto-rig it when it has no
  skeleton, and validate rig quality before generating motion.
api: graphql/uthana-schema.graphql
operations: [create_character, update_character, characters, character]
generated: '2026-07-21'
method: generated
source: https://uthana.com/docs/api/capabilities/auto-rig-and-add-character + graphql/uthana-schema.graphql
---

# Upload and auto-rig a character

## Steps

1. Prepare the model as FBX (`.fbx`) or glTF (`.glb`/`.gltf`).
2. Call the `create_character` mutation to upload. If the character has no skeleton rig,
   Uthana auto-rigs it (typically 30-60 extra seconds). Upload options include
   `rerig_target` (re-rig to a specific target skeleton) and `include_fingers`.
3. Read `auto_rig_confidence` (0.0-1.0) on the response. A low score signals likely
   auto-rig failure — non-humanoid shapes, extreme proportions, limb intersections,
   flowing clothing, or extra appendages. Surface the response `message` to the user.
4. Verify by generating a short motion against the new character id and viewing it, or
   list characters via the `characters` query.
5. Use the character id in generation mutations and download URLs from then on.

## Rules

- Character uploads/auto-rigs are metered per plan (10/month free tier; $0.15/rig
  pay-as-you-go).
- Alternatively generate characters: `create_image_from_text` / `create_image_from_image`
  then `create_character_from_image` ($0.75/character on pay-as-you-go).
