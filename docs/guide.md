<!-- source: products/callouts/dist/USER-GUIDE.md @ 8edbaa2 (timeout repo) -->

# User Guide

Numbered callout markers and leader lines for technical Blender scenes.
This guide covers installation, day-to-day usage, and known limits.

## What's in the package

- `explocallout.blend` — the asset file. It contains one asset: the
  `TO_Callouts` Geometry Nodes node group (marked in the Asset Browser
  under the "Callouts" catalog), plus a small example assembly scene
  with callouts already applied so you can see the result before
  touching anything.
- `blender_assets.cats.txt` — the asset catalog definition. Keep it in
  the same folder as the `.blend` file so Blender's Asset Browser can
  read it.
- This guide, plus the store listing copy for reference.

See [Compatibility](compatibility.md) for the supported Blender
versions.

## Installation

You have two options — pick whichever fits your workflow.

### Option A — Add as an Asset Library (recommended)

1. Put the `.blend` file (and `blender_assets.cats.txt`) in a folder you
   control, e.g. `Documents/BlenderAssets/ExploCallout/`.
2. In Blender, open **Edit > Preferences > File Paths > Asset Libraries**
   and click **+** to add a new library pointing at that folder.
3. Open the **Asset Browser** in any 3D Viewport (the icon in the
   top-left of the editor type dropdown), pick your new library, and
   you'll see the **TO_Callouts** node group under the **Callouts**
   catalog (that's the asset's technical/internal name — it's what the
   Asset Browser tile shows, distinct from the "ExploCallout" product
   name on this guide's cover).
4. Drag the node group onto any mesh object in your scene, or use the
   **Add > Node Group** menu inside a Geometry Nodes modifier.

### Option B — Append or Link directly

1. In the file you're working on, go to **File > Append** (or **Link**
   if you want to keep the asset updated from the source file), navigate
   into `explocallout.blend > NodeTree`, and select `TO_Callouts`.
2. The node group is now available in your file's Geometry Nodes
   modifier node group picker.

Append copies the node group into your file (safe, self-contained).
Link keeps a live reference to the original `.blend` file (useful if you
want updates to the node group to propagate automatically, but the
original file must stay reachable at its path).

## How it works — the short version

`TO_Callouts` takes a cloud of **anchor points** (one per part you want
to label) and, for each anchor, generates:

- a small marker (an icosphere) at the anchor position, and
- a leader line running from the anchor out to a label position.

It expects a `label_offset` vector attribute on those points — a vector
from the anchor to where you want the label to sit. You build that
point cloud yourself: one **Mesh** object where each vertex sits at a
part's world-space origin (a good name is `Callout_Anchors`, matching
the example scene), with a `Float Vector` attribute named `label_offset`
per point, then `TO_Callouts` applied to it as a Geometry Nodes
modifier. The numbered text objects and their camera-facing constraint
are also a manual step you set up per scene — the node group itself only
draws the marker and leader line from the anchors you give it.

The recommended workflow:

1. Duplicate the example scene's anchor point object as a starting
   template (**Object > Duplicate**), or start from scratch with a
   single vertex per part.
2. Keep this points object's own transform at the default — **Location
   (0, 0, 0), Rotation 0, Scale 1** (`Object > Apply > All Transforms`
   if you moved it). Mesh vertex coordinates are always in the object's
   *local* space; with an identity transform, local space and world
   space line up, so a vertex placed at a part's world position (snap
   to origin, `Shift+S > Selection to Cursor` with the 3D cursor at the
   part's origin) is stored correctly. If you ever give this object its
   own location/rotation, you'll need to place vertices in *its* local
   space instead, not world space — easiest to just leave the transform
   at default and avoid the conversion entirely.
3. Add a `label_offset` attribute: **Properties editor > Object Data
   Properties** (the green triangle-mesh icon) **> Geometry Data >
   Attributes > +**, set **Name** to `label_offset`, **Type** to
   `Vector`, **Domain** to `Point`. Then set each point's actual value
   in the **Spreadsheet Editor** (switch an editor to Spreadsheet,
   select the object, pick the `label_offset` column, edit each row) —
   a vector pointing away from the part center toward where you want
   its number to sit.
4. Add the `TO_Callouts` node group as a Geometry Nodes modifier on
   that points object, and adjust **Marker Radius** / **Leader Radius**
   to match your scene scale.
5. Add a numbered text object (Text object, `Add > Text`) at each label
   position, with a **Track To** constraint pointing at your camera, so
   labels always face it.

The bundled example scene shows this end-to-end for a 6-piece mounting
bracket assembly — open it, select the objects, and inspect the
modifier stack and the anchor points object to see a working reference.

## Presets and framing

The example scene demonstrates a **3/4 hero angle** camera. There is no
built-in "preset picker" operator in this version — camera framing is a
manual step, same as any other camera placement in Blender. As a
starting point, three angles work well with a ring-style callout layout
(avoiding a flat profile view, which tends to compress leader lines and
cause label overlap):

- **Hero 3/4** — classic three-quarter product angle, moderate
  elevation.
- **Overhead 3/4** — steep top-down angle, good for flat/plan-style
  parts.
- **Flank 3/4** — three-quarter from the opposite side, same elevation
  as hero (useful when the hero angle occludes a part you need
  visible).

For output framing, both **16:9** (landscape, e.g. video/slides) and
**9:16** (portrait, e.g. social/vertical video) work well with this
node group — keep at least a 5% margin from the frame edges for both
geometry and labels so nothing gets clipped at render time.

## Known limits (honest, not marketing)

- **Readability depends on your own label layout — there's a practical
  ceiling.** The node group itself draws markers and leader lines
  correctly regardless of anchor count — we verified that up to 50
  anchors with no geometry issues during development. But since *you*
  choose where each label sits (the `label_offset` per anchor, see "How
  it works" above), the real-world ceiling is about how tightly your
  own layout packs leader lines, not something the node group manages
  for you. As a rule of thumb, once more than about 20 leader lines
  converge on a small assembly, visually tracing "which line goes to
  which part" gets hard for a human viewer even if nothing technically
  overlaps. For larger assemblies, consider labeling in groups/zones
  rather than one single dense cluster.
- **No flat "profile" camera preset.** A near-0-degree elevation angle
  flattens the ring layout into a near-straight line and risks label
  collisions — stick to a 3/4-style angle (see above). A future update
  may add a dedicated safeguard for this case.
- **The numbered text is regular Blender text objects, not procedural
  Geometry Nodes text.** This is intentional — it keeps text
  billboarding and inspection simple and reliable. It also means
  duplicating/renumbering labels is a manual step (or your own script)
  rather than automatic.
- **File formats.** The package ships as a native `.blend` asset. Export
  to `.glb`/`.gltf`/`.fbx` from your own project after applying the
  modifier (**Convert to Mesh** first, since exporters don't always
  preserve live Geometry Nodes modifiers) — not a distributed
  conversion step in this version.

## Troubleshooting

**A part doesn't get a callout, or labels overlap in my scene.**
Double-check that (a) the part is a mesh with a real vertex at its
intended anchor world position, and (b) your `label_offset` vector
isn't pointing toward another part's label position. The example scene
is the reference for a known-good setup.

For anything not covered here, see [FAQ](faq.md) or
[Contact / Help](contact.md).
