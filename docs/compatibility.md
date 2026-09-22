<!-- source: ExploCallout USER-GUIDE 1.0.0, synced 2026-09-22 -->

# Compatibility

## Supported Blender versions

| Blender version | Status | How it's verified |
|---|---|---|
| **4.5 LTS** (tested on 4.5.13 LTS) | Tested | Automated 40-case regression suite, plus packaging and extension-manifest checks. Last run 22 Sep 2026. |
| **5.2 LTS** (tested on 5.2.1 LTS) | Tested | Same automated suite; the `.blend` file (built under 4.5.13 LTS) opens and passes under 5.2.1 LTS. Last run 22 Sep 2026. |
| Blender 4.2 – 4.4, and other 4.5.x / 5.x releases | Installs, not tested | The add-on's manifest declares a minimum of Blender 4.2 (the first version with the extension system), so Blender will install it. No test pass has been run on these versions. |
| Earlier than 4.2 | Not supported | No extension system to install the add-on into. The node group on its own (Append/Link, without the add-on) has not been tested on these versions either. |

## What this covers

ExploCallout ships two components:

- **The add-on** (`explocallout-1.0.0.zip`) — a Blender extension
  (GPL-3.0-or-later) that requires Blender's extension system, available
  from Blender 4.2 onward.
- **`explocallout.blend`** — the `TO_Callouts` Geometry Nodes node
  group, its bundled example scene, and the asset catalog file. This
  works with Append/Link/Asset Library on its own, with or without the
  add-on installed.

The add-on is optional but recommended — see the [User Guide](guide.md)
for the difference between the two ways of using the package.

## Format and rendering

- **Format:** Blender extension (add-on) + native `.blend` asset
  (Geometry Nodes node group, cataloged for the Asset Browser) + example
  scene.
- **Renderer:** works with EEVEE, Cycles and Workbench — the callouts
  are geometry and text objects, not engine-specific effects.

## Questions about your specific setup

If you're on a Blender version not listed above, or run into an issue
that looks version-related, see [Contact / Help](contact.md) — include
your exact Blender version so we can help.
