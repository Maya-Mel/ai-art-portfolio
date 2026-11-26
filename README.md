# AI Art Workflow – Maya Melamed

Portfolio for a junior AI Art Specialist role: a consistent casual-game character with three expressions, a matching icon set, a background model comparison, and the ComfyUI workflow behind them.

**Live page:** see the GitHub Pages link in the repository's About section.

## Setup
- ComfyUI on a cloud GPU (ThinkDiffusion, 24 GB)
- Model: `ThinkDiffusionXL` (SDXL) unless noted
- Sampler: euler, scheduler normal, steps 25, cfg 7

## Character
| Asset | Method | Seed | Key settings |
|---|---|---|---|
| Neutral (source) | text-to-image | 156680208700286 | 1024×1024 |
| Happy | inpainting, mask: eyebrows + mouth | batch of 4, best picked | denoise 0.85, ImageCompositeMasked onto source |
| Surprised | inpainting, mask: eyebrows + mouth | batch of 4, best picked | denoise 0.85, ImageCompositeMasked onto source |

**Source prompt**
```
cute cartoon character, casual mobile game style, vibrant colors, soft lighting, clean outlines, full body, white background
```
**Negative**
```
realistic, photo, blurry, text, watermark, deformed, extra limbs
```

**Surprised (added to the source prompt)**
```
(shocked surprised face:1.4), (small round O-shaped open mouth:1.5), (highly raised arched eyebrows:1.5), gasp
```
Negative additions: `(smile:1.4), (grin:1.3), (frown:1.4), (sad:1.4), closed mouth, happy`

**Happy (added to the source prompt)**
```
(big happy open smile:1.4), (happy raised eyebrows:1.3), joyful
```
Negative additions: `(frown:1.4), (sad:1.4), closed mouth`

## Icons
Seed `725758901226107` (fixed for the whole set), 1024×1024, batch of 4.

Shared suffix:
```
game icon, casual mobile game style, vibrant colors, soft shading, clean outlines, centered, single object, white background
```
Negative:
```
realistic, photo, blurry, text, watermark, deformed, multiple objects, character, person, hands
```
| Icon | Subject |
|---|---|
| Coin | `(round gold coin:1.4), thick rim, small embossed star in the center, game currency icon` (+ negative `star shape, star icon`) |
| Chest | `(wooden treasure chest:1.4), open lid, overflowing with gold coins, golden trim, game reward icon` |
| Gem | `(sparkling blue gem:1.4), faceted diamond shape, shiny highlights, game currency icon` |
| Crown | `(royal golden crown:1.4), encrusted with blue gems and red rubies, shiny highlights, sparkles, game reward icon` (+ negative `head, face`) |

## Model comparison (background)
1344×768, seed 725758901226107, same prompt on ThinkDiffusionXL, Juggernaut XL v8 and SDXL 1.0 base.
```
cozy fantasy kingdom village, colorful cottages with round roofs, small castle on a hill in the background, winding path, trees and flowers, sunny day, soft clouds, casual mobile game background, vibrant colors, soft shading, clean outlines, bright and cheerful
```
Negative: `realistic, photo, dark, gloomy, blurry, text, watermark, people, characters, deformed buildings`

## Workflow
`workflow/expression_inpaint.json` – Load Image (+ mask) → VAE Encode → Set Latent Noise Mask → KSampler → VAE Decode → ImageCompositeMasked (onto the source) → Save Image.

How to use it:
1. Load the approved character in **Load Image** (the file is named `source_character.png` in the workflow).
2. Right-click the image → **Open in MaskEditor** → paint only the **eyebrows and mouth**. Do not paint the eyes or nose.
3. Replace the expression part of the positive prompt and the blocked expressions in the negative prompt.
4. Run a batch of 4 and keep the clearest result.

## How this was made
I generated every image in ComfyUI and built the workflow above. The final color correction (removing the slight tone shift inside the mask) and restoring the source eyes on the surprised expression were done after export with help from Claude, which also helped build the portfolio page.
