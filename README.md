# Gaussifier 3D: live browser demo

Supplementary material for an anonymous submission. This static page runs the planar 3D Gaussian
model in the browser with WebGPU: pick one of the Kodak images or drop in your own, and it predicts
a thin slab of 3D Gaussians, refines them, and shows them in a viewer you can turn around. Nothing is
uploaded.

- **Model** picks the fast model, whose correction loop reads the frontal render, or the quality
  model, which also reads the residual backprojected through the rasterizer from the frontal view
  and a 30° view.
- **Refine** sets the number of refinement steps after the initializer (the model is trained for 6).
- **Count** scales the Gaussian count relative to the model's own count, from ×¼ to ×4.
- **Refinement** shows any refinement of the run; Source image, Difference ×4, and Density compare
  the frontal view with the input, and Centers shows every Gaussian as a dot.
- **Export .sog** saves the Gaussians of the shown refinement as a compressed SOG bundle and
  **Export .ply** as a standard 3DGS PLY. Both open upright in PlayCanvas viewers and SuperSplat,
  and in this page under *Open a saved scene*.

The page runs the model at the picture's own aspect ratio: pictures are scaled so the short side is
512 pixels (the long side at most 1024), and both sides are trimmed to a multiple of 16.

## Requirements

A browser with WebGPU (recent Chrome or Edge on desktop; Safari and Firefox with WebGPU enabled). A
run takes well under a second on a discrete GPU, a few seconds on an integrated one.
This WebGPU version is slower than the C++ TensorRT pipeline behind the paper's timings.

## View locally

WebGPU needs a secure context, and `localhost` counts as one:

```
python3 -m http.server 8000
```

then open <http://localhost:8000/>.

## Hosting

Static files only, with relative paths, ready for GitHub Pages from the repository root.
`models/` holds the two exported models (ONNX graphs plus a manifest each); `ort/` is the ONNX
Runtime Web runtime (MIT license); `oversampler/` is the WebAssembly point sampler. The 3D view uses
the PlayCanvas engine (MIT license), the SOG writer is splat-transform (MIT license), and the
rasterizer in the correction loop is a WebGPU port of the Faster-GS rasterizer.

## Images

The samples are the Kodak Lossless True Color Image Suite, released by Eastman Kodak for
unrestricted use.
