# Choose a capability

Map the requested deliverable to the smallest set of live tools. These categories help with routing; they do not replace the schemas returned by MCP discovery.

## Audio

Generate sound effects from a written brief. Capture the event, setting, intensity, perspective, duration, and whether the result must loop. Audition the imported result before accepting it.

## Environments and worlds

Generate environment maps from text or images, increase resolution, expand a source image, or derive world content. Treat generation, world derivation, scene assignment, and level placement as separate stages.

In Blender, verify the world nodes, projection, image color space, strength, and a rendered or viewport result. In Unreal, verify the imported object paths and any actors added to the level.

## Materials

Generate PBR texture sets from text, images, or source textures. Check the returned map roles rather than assuming a complete set.

In Blender, inspect image color spaces, node links, UV scale, and material slots. In Unreal, inspect the material asset or instance and its texture parameters.

## Meshes

Generate a 3D mesh from a clear source image. If the connected StudioTwin surface cannot create the source image, use an authorized image-generation tool or ask the user to supply one.

After import, check geometry, normals, scale, orientation, transforms, materials, texture paths, and destination collection or content path.

## Motion and animation

Generate, edit, or stitch motion and import it into the target host. Read the live constraints for frame rate, frame range, skeleton, trajectory, and retargeting.

Verify the resulting action, animation sequence, skeleton binding, timing, contacts, and transitions in the host application.

## Asset library and job status

Use the returned job ID to poll an asynchronous generation. Use the asset ID to resolve, download, or import an existing result.

Do not submit another generation to check progress or to transfer the same result into another host.

## Blender scene tools

Blender MCP provides scene summaries, object inspection, screenshots, navigation, renders, bundled documentation search, and Python execution. Use those tools to understand the scene and prove the StudioTwin import.

Choose purpose-built inspection tools before arbitrary Python. Background CLI tools can inspect a `.blend` file without an interactive session, but deferred operations require interactive Blender.

## Selection rules

1. Reuse an acceptable existing asset.
2. Separate paid generation from local processing.
3. Separate import from placement, saving, rendering, and export.
4. Verify each stage before using its output downstream.
5. Ask only when a missing choice changes cost, destination, mutation scope, or creative direction.
