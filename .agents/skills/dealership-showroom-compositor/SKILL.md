---
name: dealership-showroom-compositor
description: Turn vehicle photos under autos/ into 1536×1024 showroom images using background.jpeg as a loose guide; keep each car in one showroom position as the camera moves, preserve visible details, and add the fixed logo.svg overlay.
---

# Dealership showroom compositor

## Inputs and outputs

Process each supported photo in `autos/<vehicle>/` as its own image. Other photos in the same folder may help identify the vehicle, but each source photo determines what is visible in its frame. Treat `test/` images only as prior outputs to critique, never as approved targets or substitutes for source photos.

Use `background.jpeg` as a loose guide to a bright showroom with large windows, white surfaces, and a pale reflective stone floor. Adapt the room to the source camera; do not use the reference as a locked plate. Outdoor scenery is allowed beyond plausible showroom windows.

For each vehicle folder, first compare its original photos with `background.jpeg` and choose the view whose camera angle, height, and floor perspective fit the reference best. Use that photo as the spatial anchor for the car's fixed place and heading in the showroom. Treat the car as stationary at that same real-world position across the folder: other photos show the photographer moving around it. Render each room view from its source camera, so walls, windows, and floor perspective may look quite different while remaining a plausible extension of the same showroom. Do not translate, rotate, or re-stage the car between outputs. The anchor sets showroom placement only; each source photo remains authoritative for its own crop and visible car details.

Write a 1536×1024 PNG per successful source under `output/<vehicle>/`, using a source-traceable filename and avoiding collisions. Do not alter `autos/`, `background.jpeg`, `logo.svg`, or `test/`. If there are no supported photos, or either required reference (`background.jpeg` or `logo.svg`) is missing, report that and stop. If one photo is unusable, omit it with the reason and continue the batch.

## Preserve the source vehicle

The individual source photo is authoritative for that frame. Keep its viewpoint, pose, crop, apparent size, placement, and every visible vehicle feature, including paint/material colors, bodywork, wheels, trim, lights, glazing, badges, interior components, and license plate. Never add, remove, complete, replace, or reconstruct vehicle parts. Do not reveal cropped or occluded parts, even if another photo in the folder shows them. Use other photos only to confirm identity, not to copy details into this frame.

Do not repaint, recolor, change materials, or apply hue/saturation shifts. The only permitted changes to the depicted vehicle are physically plausible lighting and reflections that match the new scene; preserve its underlying paint and material colors. Reflections on glass or glossy surfaces may change to fit the showroom. Do not erase shape-defining highlights or regenerate vehicle details to force a uniform look.

## Adapt the room to the photo

For exterior photos, replace the original surroundings and ground with a plausible showroom view based on `background.jpeg`, matched to the source perspective. Preserve the complete source frame: scale proportionally and extend only the surrounding scene to reach 1536×1024. Never stretch, crop, zoom, rotate, or reposition the car to fit the canvas.

Keep the floor plane and shadows consistent with the camera. Give visible tires natural contact shadows; add subdued floor reflections only where the floor finish and view support them. Use the pale polished-floor character of `background.jpeg`; do not impose the old dark-room, warm-spotlight, or tiled-floor treatment.

For engine, trunk, interior, and other detail photos, preserve the original crop and every visible component. Do not turn a close-up into a full-car view or invent room context where the source shows none. Change surroundings only where they are visible in the source, such as beyond windows or at frame edges. Outdoor views may remain beyond showroom windows; reflections on the car should remain physically plausible.

## Add the logo overlay

After composing each image, place `logo.svg` on every output, including close-ups. Render its original black artwork with transparency, about 160 px wide, 32 px from the left edge and 32 px from the bottom edge. Do not add a backing, recolor, distort, or integrate it into the showroom scene. Keep this overlay fixed even if it covers part of the car; this watermark is the sole explicit overlay exception to preserving the source image content.

## Workflow and acceptance

1. Inventory supported images in `autos/`, inspect each source, and map it to a unique output path.
2. Composite a camera-consistent showroom around the source while preserving the vehicle under the limits above.
3. Add the fixed logo overlay and inspect the 1536×1024 result against its source. Check framing, visible vehicle details and colors, reflections, room perspective, grounding, and logo placement.
4. Deliver successful outputs and report any omitted source with its specific reason. Correct concrete failures and inspect again.

Reject an image if vehicle content is invented, removed, recolored, or otherwise changed beyond plausible lighting/reflections; if the source framing is altered; if the room or reflections conflict with the source view; if the logo is missing or changed; or if the result looks pasted, synthetic, or heavily regenerated.
