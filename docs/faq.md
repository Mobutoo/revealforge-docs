<!-- source: ExploCallout USER-GUIDE 1.0.0, synced 2026-09-22 -->

# FAQ

## Which Blender versions does this work on?

Blender 4.5 LTS (tested on 4.5.13 LTS) and Blender 5.2 LTS (tested on
5.2.1 LTS) are tested. Blender 4.2–4.4 and other 4.5.x/5.x releases can
install the add-on (the extension system requires 4.2+) but have not
been tested. See [Compatibility](compatibility.md) for the full table
and how it's verified.

## Do I need to install an add-on?

No, but it's recommended. The `TO_Callouts` node group works on its own
as a native asset — append or link it like any other Blender asset
(Options B/C in the [User Guide](guide.md#installation)), nothing to
enable in Preferences. The add-on (Option A) is a companion tool,
included in the package as a separate zip (GPL-3.0-or-later, source
included), that automates numbering, layout, and camera framing for
you; skip it if you'd rather build the anchors and labels by hand.

## What does it NOT do?

- It doesn't choose label order for you — parts are numbered by object
  name, character by character (`Part10` sorts before `Part2`); use
  zero-padded names for the order you expect.
- It doesn't manage label readability for you — you choose where each
  label sits, and very dense layouts (roughly 20+ converging leader
  lines on a small assembly) get hard for a human to trace even without
  technical overlap.
- It doesn't keep manually-dragged label positions across an Apply —
  clicking **Apply Callouts** rebuilds everything from scratch.
- It doesn't combine baked (Realize) geometry with further manual label
  moves in this version.
- It doesn't export to `.glb`/`.gltf`/`.fbx` — that's a manual
  Convert-to-Mesh-then-export step on your side.
- It doesn't write a render file — pressing **F12** is still up to you.
- It isn't built for organic, single-mesh models without separable
  parts — callouts label distinct parts, not regions of a continuous
  surface.

See [Known limits](guide.md#known-limits) in the User Guide for the full
list with context.

## Can I use this in client work or paid projects?

Yes, within the terms of the license included in the package — see
[License](license.md) for the exact terms and where to find them.

## How are updates handled?

Details on updates will be published here at launch. See
[Contact / Help](contact.md) if you have a question in the meantime.

## What's your purchase and refund policy?

Purchase and refund details will be published here at launch. See
[Contact / Help](contact.md) if you have a question before buying.

## How many parts/callouts can I label in one scene?

The node group itself draws markers and leader lines correctly
regardless of anchor count — up to 50 anchors with no geometry issues
during development. In practice, readability depends on your own label
layout: once more than about 20 leader lines converge on a small
assembly, tracing which line goes to which part gets hard for a human
viewer, even if nothing technically overlaps. See the
[User Guide](guide.md#known-limits) for details and a recommended
approach for larger assemblies.

## Does it work on organic/sculpted models?

It works on any mesh assembly where you can identify clear anchor
points (part origins). Highly organic or single-mesh models without
separable parts are outside the intended use case — callouts label
distinct parts, not regions of a continuous surface.

## Can I change the marker/leader look (color, thickness)?

Yes — `Marker Radius` and `Leader Radius` are exposed inputs on the
Geometry Nodes modifier. For color, the simplest option is the object's
own **Material Properties** tab: assign a material to the object's
first material slot, and Blender applies it to any geometry generated
by the modifier that doesn't already have its own per-face material —
no need to edit the node group itself. With the add-on, assign it
*after* your last **Apply Callouts** — Apply deletes and rebuilds that
object each time, so a material assigned before an Apply is lost.

## Something isn't working — what do I do?

Start with the [User Guide troubleshooting section](guide.md#troubleshooting).
If that doesn't resolve it, see [Contact / Help](contact.md).
