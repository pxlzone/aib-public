# AIB public banner assets

Public image files and replacement JSON for AIB marketing banners. The AIB application repository was used only as a reference and was not changed.

- [JSON preserving the supplied schema](data/banners.json): all seven banners; the nine existing image URLs were replaced, and savings text changed from black to white to match the new photography and activate the app's dark overlay. Text content, actions, order, icons, and nested information content are unchanged.
- [JSON for the inspected marketing branch](data/banners.marketing-branch.json): all seven banners using the fields that branch currently renders. Form offers receive a larger `descriptionImage` and a `descriptionText` fallback. Nested `info` content is omitted because the inspected branch does not consume it; it remains in the primary JSON.
- [Visual preview](index.html): open through a local static HTTP server. `python3 -m http.server 8765` from this repository is sufficient. The preview includes mobile cards, large information images and an optional desktop crop.
- [Dimensions and image provenance](assets/manifest.json).
- [Photography sources](provenance/photography-sources.json).

## Direct URLs

Use raw file URLs in the app, rather than GitHub HTML/blob URLs:

`https://raw.githubusercontent.com/pxlzone/aib-public/main/<asset path from assets/manifest.json>`

JSON:

`https://raw.githubusercontent.com/pxlzone/aib-public/main/data/banners.json`

`https://raw.githubusercontent.com/pxlzone/aib-public/main/data/banners.marketing-branch.json`

The supplied images use `assets/banners/v1/`; the photography replacements use `assets/banners/v3/`. The JSON always points to the current selected artwork. Future artwork revisions should use a new version directory so clients with disk caches see the new files.

## Supplied images

The six original files are hosted byte-for-byte, retaining their dimensions and branding:

| Topic | Mobile banner | Larger information image |
| --- | --- | --- |
| QR payments | `pay-by-qr-banner.jpg` (original `phone.jpg`) | `pay-by-qr-info.jpg` (original `qr.jpg`) |
| Prepaid card | `prepaid-card-top-up-banner.png` (original `Card.png`) | `prepaid-card-top-up-info.png` (original `Card-1.png`) |
| Mobile banking | `mobile-banking-banner.jpg` (original `banner.jpg`) | `mobile-banking-info.jpg` (original `image.jpg`) |

The card artwork is the supplied Islamic Visa Signature visual. Its use for the sample prepaid-card offer is a mapping from the sample request, not a verification of the pictured card's product classification.

Four additional topics use photographic images sourced directly from AIB's public website: a phone user in a mountain landscape for international transfers, a valley road for personal plans/loan, a bank consultation for savings, and a merchant in his shop for business banking. These are existing site images, exported as mobile crops and larger information images using Pillow; their scenes were not generated or altered for this request. Original source URLs and page references are recorded in the photography provenance file. This records where the images came from, not whether the source images were originally camera photographs or AI-assisted imagery.

The style reference was [AIB's public website](https://aib.af), inspected on 2026-09-30: blue and pale blue surfaces, spacious layouts, product imagery, and Afghan landscape photography. The supplied sample product copy remains unverified and includes placeholder navigation/icon examples; publishing these files does not approve that copy for production.

## Inspected app contract and integration requirements

Reference: `pxlzone/aib-app`, branch `marketing-banners`, commit `ebae0c370bbb4c21867ce4c80a151fe135fedd8c`.

- `OfferCard.tsx` renders a cover image with a 230 px-wide text block. Native mobile cards are 108 px high; carousel items are approximately 300 px wide. White text receives a 35% black scrim; black text does not.
- Desktop cards are 230 px high and also use centered `cover`. A wide image can lose its right-side subject in that taller crop. Inspect the desktop crop in the preview before using these assets in that layout; a dedicated desktop image field or changed image fitting would be an app change.
- `OfferForm.tsx` displays `descriptionImage` with `contain`, inside a 1.6 aspect-ratio area. It does not render the supplied JSON's `info.header.banner` or `info.sections`.
- The file-source Zod schema does not declare `info`, so that object is stripped during file parsing.
- The inspected `/api/banners` endpoint has `USE_MOCK_BANNERS = true`. The hard-coded response must be replaced or disabled before external JSON becomes effective. This repository does not deploy or change that endpoint.
- The inspected `scripts/server.ts` has `img-src 'self' data:`. The app's web deployment must allow `https://raw.githubusercontent.com` in `img-src` to display these hosted assets. Production headers were not tested.

The original-schema JSON is intended for the richer contract supplied in the request. The current-branch companion provides only the fields presently supported by the inspected branch and cannot reproduce the richer information sections without app work.
