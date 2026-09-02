<!-- source: products/callouts/dist/PACKAGING.md @ d72248d (timeout repo, internal build/QA doc) -->

# Compatibility

## Supported Blender versions

| Blender version | Status |
|---|---|
| **4.5 LTS** (tested on 4.5.13 LTS) | Supported — built and tested. |
| **5.2 LTS** (tested on 5.2.1 LTS) | Supported — verified via automated QA, including the full 40-case regression sweep. |

Earlier 4.x versions with Geometry Nodes and the Asset Browser will
likely work but have not been verified.

## What this covers

ExploCallout ships as a native `.blend` asset library — the
`TO_Callouts` Geometry Nodes node group, its bundled example scene, and
the asset catalog file. It is not a Python add-on, so there is no
extension-system compatibility risk to track separately from the
Blender version itself.

The `.blend` file included in the package opens, applies, and passes
the full automated regression sweep cleanly on both 4.5.13 LTS and
5.2.1 LTS — no re-download or version-specific package variant is
needed.

## Format and rendering

- **Format:** native `.blend` asset library (Geometry Nodes node group,
  cataloged for the Asset Browser) + example scene.
- **Renderer:** works with any Blender render engine (EEVEE, Cycles,
  Workbench) — the callouts are geometry and text objects, not
  engine-specific effects.
- **No external add-on, plugin, or paid dependency required.**

## Questions about your specific setup

If you're on a Blender version not listed above, or run into an issue
that looks version-related, see [Contact / Help](contact.md) — include
your exact Blender version so we can help.
