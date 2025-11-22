# ARVR_MeshManipulation
CS139_AM124_CS010
## Interactive tearable cloth with organic rips, spring-driven physics, and falling pieces

## Mass–Spring Cloth Model
Particles (Nodes)

- Each point in the cloth grid is a Node:
  pos (current position)
  prev (last position)
  acc (accumulated acceleration)
  pinned (corner anchors)
  detached (used for tearing)

- Verlet Integration
  Verlet is used for stability under tearing:
  vel = pos - prev;
  nextPos = pos + vel + acc * dt²;
  
  This handles:
  gravity
  sudden spring breaks
  large deformations
  soft cloth motion
  Much better than Euler for destruction.

## Spring System

Each spring stores:
  -endpoints a, b
  -rest length
  -stiffness k
  -type (structural, shear, bend, thickness)
  -active flag
  -Constraint solving enforces spring lengths:
    delta = p2 - p1
    corr = delta * (k * (currentLength - restLength))
    Each iteration pulls the cloth back into shape.
    More iterations = stiffer, less floppy cloth.

## Cutting Algorithm
Cutting is driven by raycasting:
worldPoint = raycast(mouse, clothMesh)

Then:
Break springs inside cut radius
Every spring whose midpoint is near the cut breaks.

## Probabilistic node detachment

- Nodes near the cut are detached using:
  proximity
  pressure
  randomness
  This makes holes ragged, not perfect circles.

- Rim widening
  Pushes edge vertices away from center:
  node.pos += direction * widenFactor
  This makes holes open clearly even before springs settle.

## Geometry Updating

The cloth uses two PlaneGeometry meshes (for two layers).
Every frame:
update geometry vertex positions
recompute normals
render
No geometry deletion — the physics creates the holes.


## Installation & Running
This is a pure client-side project.
There is no build system, bundler, or server requirement.
VS Code Live Server
Right-click → Open with Live Server
