# Tool Pocket Generator V139 — Release Notes

## Provenance
Built directly from `Tool_Pocket_Generator_V138_FINAL.zip`. V138 is the sole source build.

## Problems addressed
1. In closed mode, the outline was centered and positioned from the sampled profile, but point markers remained at their pre-close workspace coordinates. This could make the dots appear not to move with the tool outline.
2. `physicalOutlineMm()` used a separate curve calculation that skipped consecutive Curve points, so its measured shape could disagree with the canvas and geometry pipeline.
3. The profile dimension display measured the unsampled point/control-point bounds rather than the sampled final outline, causing a difference from the Geometry Check envelope.

## V139 changes
- Closed-mode point markers and their number labels now use the same scale and centering transform as the displayed closed outline.
- `physicalOutlineMm()` now delegates to `sampled()` after converting points to millimetres, so the same curve interpretation is used.
- `finalDims()` now measures the sampled profile envelope, matching the geometry pipeline's envelope calculation.
- No changes to the curve definition algorithm itself; this aligns the existing shared curve interpretation across display and measurement paths.

## Process / order
Use the existing workflow: import or create → confirm grid / detect if needed → edit points and Curve/Straight types → confirm editing → set dimensions from physical grid if applicable → Close Profile → Geometry Check → export.

## What was not changed
- Separate final-dimension storage and workspace handling from V137.
- Curve/Straight point type rules and curve construction in `sampled()`.
- Finger Relief implementation.
- JSON structure and importer/exporter, except version strings.
- STL mesh generation and Geometry Check rules.

## Verification performed
- V138 ZIP used directly as source.
- Exact `physicalOutlineMm()` and `finalDims()` functions replaced.
- Closed-mode marker drawing now derives positions from the same `scaledProfile()` points and sampled-envelope centering used for the outline.
- ZIP integrity checked and exactly one README included.

## Test status
Source-level checks only; not run in live Safari/iPad. Retest:
1. Import the JSON with master dimensions 132.59 × 35.63 mm.
2. Confirm the orange rectangle/workspace size separately.
3. Observe dots before Close Profile, then close and confirm dots remain attached to the corresponding tool points.
4. Confirm the displayed Profile envelope agrees with Geometry Check's Final profile envelope.
5. Verify the tool silhouette visually and test JSON/STL export.
