# Quick Start

The fastest path from "I have the package" to "I have a labeled scene."

## 1. Install as an Asset Library (recommended)

1. Put `explocallout.blend` and `blender_assets.cats.txt` in a folder you
   control, e.g. `Documents/BlenderAssets/ExploCallout/`.
2. In Blender, open **Edit > Preferences > File Paths > Asset Libraries**
   and click **+** to add a library pointing at that folder.
3. Open the **Asset Browser** in any 3D Viewport, pick your new library,
   and you'll see the **TO_Callouts** node group under the **Callouts**
   catalog.
4. Drag the node group onto any mesh object in your scene, or use
   **Add > Node Group** inside a Geometry Nodes modifier.

Prefer Append/Link instead? See the full [User Guide](guide.md#installation)
for that option.

## 2. Build an anchor points object

Create a single **Mesh** object where each vertex sits at a part's
world-space origin (keep the object's own transform at Location
`(0, 0, 0)`, Rotation `0`, Scale `1`). Add a `Float Vector` attribute
named `label_offset` on that mesh — one vector per point, pointing from
the anchor toward where you want its label to sit.

## 3. Apply the node group

Add `TO_Callouts` as a Geometry Nodes modifier on the anchor points
object, then adjust **Marker Radius** and **Leader Radius** to match
your scene scale.

## 4. Add numbered labels

Add a Text object at each label position, with a **Track To** constraint
pointing at your camera, so numbers stay readable as you reframe.

## 5. Inspect the example

The package ships with a worked 6-part assembly example — open it and
look at the anchor points object and modifier stack to see a known-good
reference before building your own.

For the full explanation of how the node group works, known limits, and
a FAQ, see the [User Guide](guide.md).
