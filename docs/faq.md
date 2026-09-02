<!-- source: products/callouts/dist/STORE-LISTING.md @ 0fe9194 (timeout repo, FAQ section) -->

# FAQ

## Which Blender versions does this work on?

Blender 4.5 LTS (tested on 4.5.13 LTS) and Blender 5.2 LTS (tested on
5.2.1 LTS). It's a native `.blend` asset, not a Python add-on, so
there's no extension-system compatibility risk to worry about. See
[Compatibility](compatibility.md) for the full statement.

## Do I need to install an add-on?

No. ExploCallout is a native asset library — append or link the node
group like any other Blender asset. Nothing to enable in
**Preferences > Add-ons**.

## Can I use this in client work or paid projects?

Yes, within the terms of the included license — see [License](license.md)
for the exact terms.

## What's your refund policy?

Purchases are covered by Superhive's standard marketplace refund
policy. Check the current terms on your order/receipt page, or reach
out to us at `support@revealforge.com` (see [Contact / Help](contact.md))
if anything's unclear before or after buying.

## How many parts/callouts can I label in one scene?

The node group itself draws markers and leader lines correctly
regardless of anchor count — up to 50 anchors with no geometry issues
during development. In practice, readability depends on your own label
layout: once more than about 20 leader lines converge on a small
assembly, tracing which line goes to which part gets hard for a human
viewer, even if nothing technically overlaps. See the
[User Guide](guide.md#known-limits-honest-not-marketing) for details
and a recommended approach for larger assemblies.

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
no need to edit the node group itself.

## Something isn't working — what do I do?

Start with the [User Guide troubleshooting section](guide.md#troubleshooting).
If that doesn't resolve it, see [Contact / Help](contact.md).
