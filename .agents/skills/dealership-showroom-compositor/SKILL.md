---
name: dealership-showroom-compositor
description: Process every vehicle image under autos into a photorealistic image that looks photographed inside the fixed background.png showroom, preserving exact vehicle identity, source framing where appropriate, and showroom branding while replacing source-scene lighting and reflections.
---

# Dealership showroom car compositor

## Mission

This skill is the repository's workflow for turning a batch of vehicle photographs into consistent dealership-showroom imagery.

For every supported image in `autos/<vehicle>/`, create a corresponding image that looks as though that photograph was taken inside the exact showroom shown in `background.png`.

The central distinction is:

- The vehicle folder supplies identity evidence, materials, details, and—especially for interior/detail photos—the subject and viewpoint.
- `background.png` supplies the authoritative showroom, camera, lighting environment, floor geometry, and branding.
- The vehicle photograph's original environment, ground, shadows, highlights, and reflections are not identity and must not be carried into the result.

Use the available image-editing capability automatically when the required inputs are present. Do not ask for per-image confirmation.

## Input and output contract

- The only supported batch input root is `autos/`.
- Each vehicle is a folder directly below it: `autos/<vehicle>/`.
- Each vehicle folder may contain multiple exterior, interior, and detail photographs. Use the whole folder as one identity set; do not treat its images as unrelated cars.
- The required showroom plate is `background.png` at the repository root. Use its native canvas, currently 1536×1024, for every output.
- Write successful results under the mirrored path `output/<vehicle>/`, keeping each source basename traceable.
- Do not modify source images or `background.png`.
- Process supported raster images and ignore non-image metadata or sidecar files. One bad source must not stop the rest of the batch.
- If `autos/` is missing or empty, or `background.png` is missing, stop and report the missing input. Do not fall back to another directory, loose files, a different showroom, or invented references.
- If a source cannot pass quality control after the allowed correction, do not write a misleading output; report that source and the failure reason.
- Do not use web images or substitute vehicles, showrooms, logos, or architectural elements.

## Reference roles

### `background.png` — locked showroom plate

Treat `background.png` as the fixed base scene and camera. Preserve its pixels everywhere outside the vehicle and the smallest physically necessary local edit area for occlusion, contact shadow, and floor reflection.

Do not regenerate, redesign, resize, crop, reframe, zoom, recolor, or improve the wider plate. Preserve exactly what is actually present, including:

- ceiling, wall, floor, tile seams, and wall/floor boundary
- warm spotlights and their falloff
- exposure, gradients, texture, and photographic noise
- dealership branding, logo geometry, lettering, and placement

Keep `NICO ALBLAS`, `NA`, and `AUTOMOBIELEN` readable and unchanged. Do not redraw or regenerate the logo. Do not invent furniture or other objects that are not in the plate.

### Vehicle folder — identity set

Use every image in the vehicle folder to resolve the same car's identity and details. Preserve, where visible and supported by the references:

- make, model, generation, body style, proportions, and wheelbase
- paint color, finish, trim, and exterior package
- wheel design, tire proportions, grille, lamps, bumpers, glazing, mirrors, handles, and roof details
- badges, lettering, plates, vents, moldings, chrome, black trim, and interior materials

Never merge details from different vehicles. Do not redesign, beautify, restyle, add accessories, remove features, or replace the source vehicle with a similar one. Preserve the source plate and badges; never add an implicit dealership plate or logo.

The vehicle's material and finish are identity. Its original outdoor illumination and reflections are not.

## Exterior/full-vehicle images

For a source that shows the exterior or whole vehicle:

1. Use `background.png` as the base plate.
2. Analyze the plate's floor plane, horizon, vanishing directions, tile grid, wall/floor boundary, and open staging area.
3. Place the vehicle in the consistent center-left staging zone at a medium-to-rear depth so the logo and meaningful room context remain visible.
4. Let the actual floor geometry and the vehicle's body type determine projected scale. Do not use a universal shrink factor, fixed frame-width percentage, or source pixel size as a scale reference.
5. Keep the same showroom camera and general three-quarter presentation across the batch, adapting modestly for vehicle length, height, wheelbase, and body style.
6. Reorient or reproject the vehicle when needed to match the showroom camera. Use the least aggressive plausible change; do not invent a dramatic unseen side merely to improve composition.
7. Seat all four tires on the same receding floor plane. The car must have believable ground clearance, wheel geometry, and contact with the tiles.

Prefer a position that leaves the dealership branding unobstructed whenever a physically plausible option exists. Do not move the logo or background to accommodate the car.

## Interior and detail images

For a source that shows the cabin or a detail such as a dashboard, seat, wheel, lamp, or trim:

- Preserve the source subject, viewpoint, framing, and recognizable scale.
- Keep the output on the native `background.png` canvas, but do not force a wide room view behind a close-up where a real camera could not see it.
- Replace only the visible original surroundings where geometrically possible. Let the showroom appear through windows, doors, or natural frame edges only when that is physically plausible.
- Use the showroom as the environmental and lighting reference even when little of it is visible: match its exposure, warm color temperature, contrast, reflections, and depth cues.
- Do not turn an interior/detail photograph into a newly invented exterior vehicle photograph.
- Preserve interior materials, trim, controls, stitching, badges, and other identity-bearing details; do not combine incompatible interiors from different references.

## Remove source-scene lighting and reflections

The result must not retain evidence that the source car was photographed outdoors or under unrelated lighting. Replace, do not merely tint, the source environment on the vehicle.

Remove or replace:

- sky, clouds, trees, roads, lane markings, buildings, open-air horizons, and daylight spill
- blue or cool outdoor ambient light
- source-camera glare and highlights that reveal the original location
- source ground, cast shadow, horizon, dirt, and road or pavement reflection

Rebuild the vehicle's visible illumination from the showroom plate:

- warm overhead spotlight highlights on paint, roof, hood, glass, chrome, and glossy trim
- dark-wall and tiled-floor reflections appropriate to the car's material
- showroom-matched exposure, white balance, contrast, sharpness, and noise/grain
- no artificial rim light, studio setup, HDR treatment, or CGI sheen absent from the plate

Preserve the car's paint color and material response while changing the environment reflected in it. Outdoor reflections in windows, paint, chrome, or mirrors are a hard failure even if the vehicle geometry is correct.

## Grounding and floor interaction

For exterior vehicles, integrate the car into the existing tile plane rather than painting a generic shadow beneath it:

1. Use the darkest, tightest occlusion at each tire contact patch and directly under the rocker panels.
2. Add a broader, soft shadow consistent with the warm overhead lights.
3. Add a low-contrast, diffused floor reflection aligned to the tile perspective and softened with distance.
4. Keep the original floor seams and reflections around the car. Occlude seams only where the vehicle physically covers them; never draw tile lines through tires or bodywork.

The reflection must remain secondary to the car and must not be mirror-like, copied from the source, or strong enough to erase the tile pattern. Do not use a single uniform black shadow, halo, floating tires, or a cutout edge.

## Mandatory workflow

1. Inventory `autos/` and map every source image to its vehicle folder and output path.
2. Inspect the complete vehicle folder as an identity set and classify each image as exterior/full-vehicle or interior/detail.
3. Lock the original `background.png` plate and infer its camera and floor geometry before editing.
4. Perform the smallest appropriate masked edit: exterior images use the showroom plate as the base; interior/detail images preserve their subject viewpoint and reveal showroom context only where plausible.
5. Remove source-scene ground, lighting, and reflections; relight and ground the subject using the showroom cues above.
6. Run the quality-control checklist below against both the complete vehicle identity set and the untouched background.
7. If a hard failure is found, make one focused corrective pass using the original background and source references. Do not broadly regenerate the scene.
8. Deliver only passing outputs and report any omitted source with its reason.

## Quality control

Reject or correct an output if any of the following is visible:

- any change to the wider showroom, floor pattern, spotlights, wall texture, logo, or lettering
- an added, removed, substituted, or distorted vehicle identity detail
- outdoor sky, trees, road, building, daylight, blue ambient light, or source-camera reflection
- source ground, source shadow, or source reflection carried into the showroom
- a vehicle that is too near, too dominant, or inconsistent with the fixed staging zone without a geometric reason
- inconsistent showroom camera, floor perspective, wheel ellipses, vehicle height, or depth
- tires not sharing one floor plane, implausible ground clearance, floating/intersecting geometry, or missing contact occlusion
- mirror-like or misaligned floor reflection, tile seams drawn through the vehicle, haloing, or an obvious cutout
- interior/detail framing replaced by an invented exterior view or an impossible wide-room background
- mismatched exposure, white balance, sharpness, contrast, or photographic noise
- AI-generated, CGI, over-sharpened, pasted, or synthetic appearance

When correcting, change only the failing region. Always preserve the original plate, exact car identity, source detail framing where applicable, and all unrelated pixels.

## Priority order

Resolve conflicts in this order:

1. Preserve the original showroom plate and branding outside the local edit area.
2. Preserve the exact vehicle identity, materials, and source details.
3. Preserve the appropriate source framing for interior/detail images and the fixed showroom camera for exterior images.
4. Achieve physically plausible floor placement, scale, perspective, and grounding.
5. Replace source-scene lighting and reflections with showroom-consistent environmental cues.
6. Improve composition and polish only when the preceding requirements are already satisfied.

## Output behavior

Write one corresponding result per passing source image under `output/<vehicle>/`, without changing the source files. Keep the outputs traceable to their source basenames and report any source that could not pass.
