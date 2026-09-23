---
name: dealership-showroom-compositor
description: Turn vehicle photos under autos/ into 1536×1024 dealership-showroom images, keeping each source photo's vehicle, viewpoint, and framing while using background.png as a visual guide.
---

# Dealership showroom compositor

## Purpose and inputs

Process each supported vehicle image in `autos/<vehicle>/` into a believable showroom photograph. Use the images in one vehicle folder together to check the vehicle's identity and details.

The **original vehicle photo is the primary image**. Keep its camera view, vehicle position and apparent size, crop, and visible vehicle details. `background.png` is a guide to the dealership's dark showroom mood, warm lighting, and tiled floor. It is not a locked plate or a camera template. Any images in `test/` are previous outputs to critique, not approved visual targets or substitute vehicle references.

Write a 1536×1024 PNG for each successful source under `output/<vehicle>/`, with a filename traceable to the source. Avoid overwriting outputs when source stems collide. Do not alter `autos/`, `background.png`, `logo.png`, or `test/`. If `autos/` has no vehicle photos or `background.png` is missing, report that and stop. One unusable photo must not halt the rest of the batch.

Use the available image-editing capability when the inputs exist; routine processing does not require per-image confirmation.

## Composition: follow the source camera

Inspect each original photo before choosing a showroom treatment. Preserve the visible side of the car, the viewing height, perspective, pose, and relative placement in frame. Do not rotate the car into a standard three-quarter view, invent unseen bodywork, or shrink or enlarge it to fit a predetermined staging spot. If the source intentionally crops part of the vehicle, retain that crop rather than generating the missing part.

Adapt the surrounding room to the source camera. Use the literal `background.png` scene only when its viewpoint and floor geometry fit naturally. Otherwise create a plausible variation with the same dark dealership atmosphere, warm overhead light, and floor material. The room layout, wall visibility, spotlight positions, and framing may vary to suit the source angle. Do not force a wall, horizon, or wide room view behind a source that could not have photographed one.

For source shapes other than 3:2, scale proportionally and extend the surrounding scene to reach 1536×1024. Do not stretch the car, crop away source vehicle content, or materially change its prominence. If the source shows a close-up, keep it a close-up.

### Exterior photos

- Replace the original surroundings and ground with a showroom scene consistent with the source perspective. Keep vehicle geometry and identity anchored to the original pixels and the other photos in its folder.
- Give every tire that is visible a convincing contact patch. Project tile seams and soft shadows on the same floor plane; occlude seams behind the car and tires.
- Keep one implied physical tile size across the batch. Apparent tile width may change with camera depth and perspective, but must remain plausible relative to the vehicle and consistent between results. Avoid oversized or miniature tiles.
- Add floor reflection only when the floor finish and camera angle support it. Keep it subdued and perspective-correct.

### Interior and detail photos

Keep the original close-up viewpoint, subject, and crop. Change visible outside scenery or surrounding surfaces only where they could naturally appear through windows, doors, or frame edges. Match the showroom's warm lighting and material reflections without turning a detail into a full-car image or inventing a wide room behind it.

## Vehicle and lighting fidelity

Preserve the source vehicle's make, model, body shape, proportions, paint, wheels, trim, lights, glazing, badges, and **original license plate**. Preserve visible interior materials, controls, and stitching in close-ups. Use other photos in the same vehicle folder to resolve uncertain details; never borrow details from a different car. Do not add dealership plates, accessories, or logos to the vehicle.

Match the subject to the new room with restrained, photographic changes to exposure, color temperature, shadow, and reflections. Remove clearly recognizable outdoor scenery from glass or glossy paint when it can be replaced without distorting the vehicle. Do not repaint the car, erase its shape-defining highlights, or regenerate identity-bearing parts to achieve a uniform lighting effect. The result should look photographed in the showroom, without a cutout edge, halo, artificial rim light, or glossy CGI finish.

## Dealership logo

Decide from the **source photo's camera and background** whether a believable, visible wall area exists for the dealership logo. Show it only when it fits that geometry, scale, and composition. `logo.png` supplies the emblem; `background.png` shows the complete sign with its lettering. Keep their actual shapes and the exact `NICO ALBLAS`, `NA`, and `AUTOMOBIELEN` wording. Do not generate approximate letters, a warped emblem, or a new brand mark. If a clean, legible placement is not possible, omit the logo. Never move or reshape the car merely to make room for it.

## Workflow and acceptance

1. Inventory `autos/`, inspect each vehicle folder as one identity set, and map each source to a distinct output path.
2. For each photo, note its vehicle viewpoint, crop, apparent size, camera height, visible background, and floor perspective. Decide how much of the showroom can plausibly be visible and whether the logo can fit.
3. Edit from the source photo as the primary image, using `background.png` for room character and tile scale. Keep protected vehicle details and the original plate intact.
4. Compare the result with the source and other photos of that car. Check vehicle identity, view, crop, apparent size, floor perspective and tile scale, tire contact, lighting, edge quality, and any logo. Correct a concrete failure locally and inspect again.
5. Deliver only results that pass. Report any omitted source and the specific reason instead of writing a misleading image.

Reject an image if it changes the car's visible details or plate; changes its viewpoint, crop, or prominence without a source-based reason; uses room geometry or tile scale inconsistent with the source camera; floats above the floor; contains obvious outdoor scenery/reflections; alters or invents the logo; or looks pasted, synthetic, or heavily regenerated.
