# StudioTwin credits

StudioTwin charges credits for cloud generation. Local scene inspection, placement, and verification should not be confused with paid generation.

The figures below came from StudioTwin's published dashboard documentation on 2026-08-14. Models and prices can change. For an MCP call, use the cost reported by the live tool or cost estimator.

Documentation: https://docs.studiotwin.ai/docs/dashboard/guides/how-credits-work

## Free monthly credits

New accounts receive 50 subscription credits each billing cycle.

Completing the profile raises the monthly allocation to 100 credits and adds 50 credits immediately. Required fields are first name, last name, team size, and use case. Company is optional.

## Credit types

| Type | Behavior |
| --- | --- |
| Subscription | Used first and reset each billing cycle. |
| Top-up | Purchased separately, used after subscription credits, and does not expire. |

Credits can be purchased at [app.studiotwin.ai/dashboard/buy-credits](https://app.studiotwin.ai/dashboard/buy-credits).

## Published reference costs

| Toolkit or operation | Model | Approximate credits |
| --- | --- | ---: |
| Text to Motion | HY Motion 1.0 or NVIDIA Kimodo | 5 per action |
| Motion edit, stitch, or trajectory | NVIDIA Kimodo | 5 each |
| Text to environment map | FLUX.1-schnell | 20 |
| Image to environment map | FLUX.1-schnell | 25 |
| Environment map to world | Tencent HY World 1.0 | 80 |
| Image to 3D mesh | Hunyuan 3D V2.1, Tripo3D P1, or Tripo3D V3 | 25, 75, or 45 |
| Texture to material | FAL Patina | 20 |
| Text or image to material | FAL Patina | Resolution-based formula |
| Text to sound effect | ElevenLabs | Duration-based formula |

For text-to-material, the published formula is `round(2 + 10 * MP + 3 * MP * upscale)`. For image-to-material, it is `round(13 + 10 * MP + 3 * MP * upscale)`. The upscale term is ignored at 1x; 1 MP is 1024 by 1024.

For sound effects, the published formula is `round(max(seconds, 5) * 0.6)`. Automatic duration reserves 18 credits for the 30-second maximum and refunds the unused balance after generation.

Use these figures for rough onboarding estimates only. Quote the live connector before a paid call.
