<!-- source: ExploCallout USER-GUIDE 1.0.0, synced 2026-09-22 -->

# Quick Start

The recommended workflow from "I have the package" to "I have a labeled
scene" — using the add-on. Prefer to work with the node group directly,
without installing the add-on? See
[Without the add-on](guide.md#without-the-add-on) in the User Guide
instead.

## 1. Install the add-on

In Blender, open **Edit > Preferences > Get Extensions**, click the
dropdown next to the search field, and choose **Install from Disk**.
Select `explocallout-1.0.0.zip` from the package and confirm. Blender
installs and enables the add-on — nothing else to toggle on.

## 2. Make `explocallout.blend` available

The add-on needs `explocallout.blend` (from this same package) to find
the `TO_Callouts` node group. It looks for it automatically, in order:
the **Library** field in the panel (if you set one), the add-on's own
install folder, the root folder of any Blender asset library you've
registered, and finally the folder of the `.blend` file you currently
have open. Simplest setup: point the panel's **Library** field at
wherever you put `explocallout.blend`.

## 3. Open the panel

In any **3D Viewport**, press **N** to open the side panel and look for
the **ExploCallout** tab.

## 4. Apply Callouts

Leave **Scope** on **Scene** to label every eligible mesh, or set it to
**Selected** and select just the parts you want. Click **Apply
Callouts** — in one step it numbers your parts, builds the anchor
points, applies `TO_Callouts`, adds the numbered labels, and frames a
preview camera.

## 5. Adjust

Tweak **Label Distance**, **Label Spread**, **Tiers**, **Marker
Radius**, **Leader Radius**, and **Text Height**, then click **Apply
Callouts** again to rebuild with the new values. Drag a label to a
better spot, then click **Sync Leaders** to follow it. Pick a camera
preset (**Hero**, **Overhead**, **Flank**) and an aspect ratio, then
click **Frame Camera** to reframe.

## 6. Render

Press **F12**. The add-on never writes a render file on its own — it
only sets up the scene for your usual render step.

For the full explanation of every panel option, the manual node-group
workflow, known limits, and a FAQ, see the [User Guide](guide.md).
