<!-- source: ExploCallout USER-GUIDE 1.0.0, synced 2026-09-22 -->

# User Guide

Numbered callout markers and leader lines for technical Blender scenes.
This guide covers installation, day-to-day usage, known limits, and
troubleshooting.

## What's in the package

- `explocallout-1.0.0.zip` — the ExploCallout add-on: a small Blender
  extension that automates numbering, layout, and camera framing from a
  side panel. Optional, but it's the recommended workflow to use the
  asset (see "How it works" below). Source included, GPL-licensed —
  see
  [License](license.md).
- `explocallout.blend` — the asset file. It contains one asset: the
  `TO_Callouts` Geometry Nodes node group (marked in the Asset Browser
  under **RevealForge › Callouts**), plus a small example assembly
  scene with callouts already applied so you can see the result before
  touching anything.
- `blender_assets.cats.txt` — the asset catalog definition. Keep it in
  the same folder as the `.blend` file so Blender's Asset Browser can
  read it.
- This guide, the license, and the license files.

See [Compatibility](compatibility.md) for the supported Blender
versions and how that's been verified.

## Installation

The add-on is optional, but it's the recommended workflow to get
consistent numbering, layout, and camera framing — Option A below. If you'd rather
work with the node group directly, Options B and C skip the add-on
entirely. They aren't mutually exclusive: you can install the add-on and
still append/link the node group into other files.

### Option A — Install the add-on (recommended)

1. In Blender, open **Edit > Preferences > Get Extensions**, click the
   dropdown next to the search field, and choose **Install from Disk**.
2. Select `explocallout-1.0.0.zip` from the package and confirm. Blender
   installs and enables the add-on — nothing else to toggle on.
3. In any **3D Viewport**, press **N** to open the side panel and look
   for the **ExploCallout** tab. That's the panel described in "How it
   works" below.
4. The add-on needs `explocallout.blend` (from this same package) to
   find the `TO_Callouts` node group. If it's already loaded in your
   current file (you appended or linked it before), that's used
   directly. Otherwise it looks for `explocallout.blend` automatically,
   in order: the **Library** field in the panel (if you set one), the
   add-on's own install folder, the root folder of any Blender asset
   library you've registered (Option B below — the `.blend` must sit
   directly in that folder, not in a subfolder), and finally the folder
   of the `.blend` file you currently have open. The simplest setup:
   put `explocallout.blend` and `blender_assets.cats.txt` directly in an
   asset library's root folder (Option B, step 1-2) and the add-on finds
   them without any extra configuration. Otherwise, point the panel's
   **Library** field at wherever you put `explocallout.blend`.

### Option B — Add as an Asset Library (node group only)

1. Put the `.blend` file (and `blender_assets.cats.txt`) in a folder you
   control, e.g. `Documents/BlenderAssets/ExploCallout/`.
2. In Blender, open **Edit > Preferences > File Paths > Asset Libraries**
   and click **+** to add a new library pointing at that folder.
3. Open the **Asset Browser** in any 3D Viewport (the icon in the
   top-left of the editor type dropdown), pick your new library, and
   you'll see the **TO_Callouts** node group under **RevealForge ›
   Callouts** (that's the asset's technical/internal name — it's what
   the Asset Browser tile shows, distinct from the "ExploCallout"
   product name on this guide's cover).
4. Drag the node group onto any mesh object in your scene, or use the
   **Add > Node Group** menu inside a Geometry Nodes modifier.

### Option C — Append or Link directly (node group only)

1. In the file you're working on, go to **File > Append** (or **Link**
   if you want to keep the asset updated from the source file), navigate
   into `explocallout.blend > NodeTree`, and select `TO_Callouts`.
2. The node group is now available in your file's Geometry Nodes
   modifier node group picker.

Append copies the node group into your file (safe, self-contained).
Link keeps a live reference to the original `.blend` file (useful if you
want updates to the node group to propagate automatically, but the
original file must stay reachable at its path).

## How it works

With the add-on installed (Option A), the fast path is:

1. Open the **ExploCallout** tab in the 3D Viewport's side panel (**N**).
2. In the Targets section, leave **Scope** on its default, **Scene**, to
   label every eligible mesh in the scene — or set it to **Selected**
   and select just the parts you want to label. **Start Index** sets the
   first number used; parts are always numbered in name order, character
   by character (shown as **Order: Name (A-Z)**, not editable in this
   version) — so `Part10` sorts before `Part2`. Use zero-padded names
   (`Part01`, `Part02`, ...) to get the numbering you expect.
3. Click **Apply Callouts**. In one step, it numbers your parts, builds
   the anchor points, applies `TO_Callouts`, adds the numbered labels,
   and (if **Create Preview Camera** is checked, on by default in the
   Camera section) frames a preview camera. **Clear Callouts** removes
   everything the add-on generated, if you want to start over.
4. Adjust **Label Distance**, **Label Spread**, **Tiers**, **Marker
   Radius**, **Leader Radius**, and **Text Height** in the panel, then
   click **Apply Callouts** again to rebuild with the new values.
5. Drag a label to a better spot in the viewport, then click **Sync
   Leaders** — the leader line follows it to the new position.
6. Click **Frame Camera** whenever you change the camera preset or
   aspect ratio, to reframe on the current layout.
7. Press **F12** to render. The add-on never writes a render file on its
   own — it only sets up the scene for your usual render step.

Use **Check Layout** after an Apply or Frame Camera (it needs an active
camera to measure against, and reports an error if there isn't one) to
look for overlapping labels or anything outside a 5% safe-area margin —
it reports the number of overlapping label pairs, shows the safe-area
status in the panel, and selects the labels at fault (a piece outside
the safe area only shows up in the warning message, not in the
selection).

`TO_Callouts` itself does the actual geometry work: it takes a cloud of
**anchor points** (one per part you want to label) and, for each anchor,
generates a small marker (an icosphere) and a leader line running out to
a label position. The add-on builds that point cloud, applies the node
group, and creates the numbered text objects (with a camera-facing
constraint) for you — see "Without the add-on" below if you'd rather
build all of that by hand.

The bundled example scene shows the result end-to-end for a 6-piece
mounting bracket assembly — open it, select the objects, and inspect the
modifier stack and the anchor points object to see a working reference.

### Without the add-on

If you installed the node group without the add-on (Option B or C), the
same result is possible, but every step below is manual. `TO_Callouts`
expects a `label_offset` vector attribute on the anchor points — a
vector from the anchor to where you want the label to sit.

There are two starting points — pick one, the steps that follow differ:

**Starting from the example scene's points object (duplicate):**

1. Duplicate the example scene's anchor point object (**Object >
   Duplicate**). It already carries the `label_offset` attribute and the
   `TO_Callouts` Geometry Nodes modifier — you don't add either again
   (see "Starting from scratch" below for why re-adding the attribute is
   a mistake on a duplicate). Edit its vertices and `label_offset`
   values for your own parts, keeping the transform rule in step 2.
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

**Starting from scratch (a plain mesh, single vertex per part):**

1. Create a new mesh object with one vertex per part, placed at each
   part's world position (same transform rule as step 2 above).
2. Add a `label_offset` attribute and set each point's value with a
   short script, from the **Scripting** workspace's Python console (with
   the points object selected):
   ```python
   import bpy
   obj = bpy.context.object
   attr = obj.data.attributes.get("label_offset")
   if attr is None:
       attr = obj.data.attributes.new(name="label_offset", type="FLOAT_VECTOR", domain="POINT")
   attr.data[0].vector = (0.3, 0.0, 0.2)  # repeat per point index, one call per vertex
   ```
   Each vector points away from the part center toward where you want
   its number to sit; `attr.data[i]` follows the same vertex order as
   the points object's vertices. Use `attributes.get(...)` first, not
   `attributes.new(...)` directly — on a fresh object it's a no-op (the
   attribute doesn't exist yet), but calling `new` on an object that
   already has a `label_offset` attribute (e.g. a duplicate of the
   example points object) creates a second, differently-named attribute
   that `TO_Callouts` never reads.
3. Add the `TO_Callouts` node group as a Geometry Nodes modifier on that
   points object, and adjust **Marker Radius** / **Leader Radius** to
   match your scene scale.

**Either way, finish with:**

Add a numbered text object (Text object, `Add > Text`) at each label
position, with a **Track To** constraint pointing at your camera, so
labels always face it.

## Presets and framing

With the add-on, the panel's **Camera** section is a built-in preset
picker: choose **Hero (3/4)**, **Overhead (3/4)**, or **Flank (3/4)**,
pick an **Aspect** (**16:9**, **9:16**, or **Scene** to match your
current render resolution), and click **Frame Camera** to (re)position
the preview camera for that combination. Run it again any time you
change a setting or move labels, to reframe on the current layout.

**Aspect and your render resolution.** Whenever the add-on frames the
preview camera — clicking **Frame Camera**, or **Apply Callouts** with
**Create Preview Camera** on (the default) — choosing **16:9** or
**9:16** also sets your scene's render resolution to a fixed 1280x720 or
720x1280 and makes the preview camera the active camera. If you're
rendering at a specific resolution (e.g. 4K) and don't want it
overwritten, choose **Scene** instead, which reads and keeps your
current render resolution. Turning off **Create Preview Camera** skips
this entirely on Apply (no camera framed, no resolution change); any
preview camera from an earlier Apply is removed too, so set your own
scene camera as active before rendering.

The example scene demonstrates the **Hero (3/4)** angle. All three
presets avoid a flat profile view, which tends to compress leader lines
and cause label overlap:

- **Hero (3/4)** — classic three-quarter product angle, moderate
  elevation.
- **Overhead (3/4)** — steep top-down angle, good for flat/plan-style
  parts.
- **Flank (3/4)** — three-quarter from the opposite side, same elevation
  as hero (useful when the hero angle occludes a part you need
  visible).

Without the add-on, camera framing is a manual step, same as any other
camera placement in Blender — the three angles above are still a good
starting point. For output framing, both **16:9** (landscape, e.g.
video/slides) and **9:16** (portrait, e.g. social/vertical video) work
well with this node group — keep at least a 5% margin from the frame
edges for both geometry and labels so nothing gets clipped at render
time (the add-on's **Check Layout** button checks this margin for you).

## Known limits

- **Readability depends on your own label layout — there's a practical
  ceiling.** The node group's marker/leader geometry scales with anchor
  count — we verified clean results up to 50 anchors during development
  and haven't pushed past that. But since *you* choose where each label
  sits (the `label_offset` per anchor, cf. "How it works" above), the
  real-world ceiling is about how tightly your own layout packs leader
  lines, not something the node group manages for you. As a rule of
  thumb, once more than about 20 leader lines converge on a small
  assembly, visually tracing "which line goes to which part" gets hard
  for a human viewer even if nothing technically overlaps. For larger
  assemblies, consider labeling in groups/zones rather than one single
  dense cluster.
- **No flat "profile" camera preset.** A near-0-degree elevation angle
  flattens the ring layout into a near-straight line and risks label
  collisions — stick to a 3/4-style angle (see above).
- **Apply Callouts rebuilds everything from scratch, every time.**
  Clicking **Apply Callouts** deletes and regenerates every marker,
  leader line, and label — including any you dragged into a custom
  position with **Sync Leaders**. There's no way to change one setting
  and keep your manual moves; the next Apply always starts over. Do your
  slider adjustments first, drag labels into their final spots last, and
  only click **Apply Callouts** again after that if you change a
  setting — otherwise you'll lose the manual moves.
- **Realize (bake) and manual label moves don't combine in this
  version.** If **Realize (bake) geometry** is on, the leader-line
  geometry is converted to a fixed mesh at the next **Apply Callouts**,
  and **Sync Leaders** becomes unavailable (greyed out) afterwards —
  there's nothing live left to update. Turning Realize on always
  requires one more click on **Apply Callouts** to actually perform the
  bake, and that Apply wipes any manual label moves you made first
  (previous point) — there's no way around this in this version. So: if
  you want the baked, static geometry, accept the computed (non-dragged)
  label positions and turn Realize on before your last Apply. If you
  want your own manual label positions, leave **Realize** off — Sync
  Leaders will keep working, but the leader-line geometry stays live
  (not baked).
- **The numbered text is regular Blender text objects, not procedural
  Geometry Nodes text.** This is intentional — it keeps text
  billboarding and inspection simple and reliable. With the add-on,
  clicking **Apply Callouts** regenerates and renumbers the whole set
  automatically. What's still a manual step (or your own script) is
  adding or duplicating a *single* extra label without a full re-apply —
  e.g. splitting one part into two callouts by hand.
- **File formats.** The package ships as a native `.blend` asset. To
  export to `.glb`/`.gltf`/`.fbx`, duplicate the object first, then
  **Convert to Mesh** on the duplicate before exporting (exporters don't
  always preserve live Geometry Nodes modifiers, and Convert to Mesh
  replaces the live modifier with a static mesh on whichever object you
  run it on) — not a distributed conversion step in this version.

## Troubleshooting

**I found a part that doesn't get a callout / labels overlap in my
scene.** With the add-on: the anchor is the part's own object origin,
not a vertex — double-check (a) the part is a **mesh object** with its
origin placed where you want the callout to point (**Object > Set
Origin** if it's off), and (b) if **Scope** is set to **Selected**, that
the part is actually selected. Without the add-on (manual flow, see
[Without the add-on](#without-the-add-on) above): double-check (a) the
anchor points object has a real vertex at the part's intended anchor
world position, and (b) your `label_offset` vector isn't pointing toward
another part's label position. The example scene is the reference for a
known-good setup.

For anything not covered here, see [FAQ](faq.md) or
[Contact / Help](contact.md).
