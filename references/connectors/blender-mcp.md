# Blender connector

**Status: live.** StudioTwin's Blender workflow combines its cloud generation and asset library with Blender Lab's official Blender MCP.

## How the pieces fit

Blender MCP has two processes:

```text
MCP client <-> MCP over stdio <-> blender-mcp <-> local TCP socket <-> Blender add-on
```

The Blender add-on runs inside Blender. The `blender-mcp` Python server is launched by the MCP client and relays requests to the add-on.

StudioTwin adds the cloud side of the workflow:

1. Generate or resolve an asset with StudioTwin.
2. Keep the returned job ID and asset UUID.
3. Download or import that asset into Blender using the tools exposed by the connected StudioTwin surface.
4. Use Blender MCP to inspect the imported data, place it, and verify the scene.

The asset UUID is the handoff contract. It also works with StudioTwin's Unreal connector, so a team can reuse one generation across both DCCs.

## Current Blender MCP baseline

The GA guidance is based on Blender Lab's official Blender MCP v1.0.0 and upstream `main` at commit `4309a39646e644261624bfcd2bca669b343b7621`.

Current requirements and packaging:

- Blender 5.1 or newer;
- Python 3.10 or newer for the MCP server;
- the Blender Lab Extensions repository at `https://lab.blender.org/`;
- the MCP add-on installed and enabled in Blender;
- the `blender-mcp` server launched by the MCP client;
- a local TCP connection between the server and Blender.

The official server can also be installed from source:

```bash
pip install git+https://projects.blender.org/lab/blender_mcp.git#subdirectory=mcp
```

Use the package or client-specific instructions published at [blender.org/lab/mcp-server](https://www.blender.org/lab/mcp-server/) when available. Do not expose the add-on's local TCP listener beyond the machine.

## What changed for v1.0 and current main

Blender Lab's v1.0 work added or consolidated:

- an MCPB package and corrected MCPB entry point;
- a Blender 5.1 minimum version in the add-on manifest;
- safety annotations and tool names;
- bundled Blender Python API and user-manual documentation;
- `get_python_api_docs`, `search_api_docs`, and `search_manual_docs`;
- a server layout under `mcp/` with `blender-mcp` as the entry point;
- matching v1.0.0 versions for the add-on and server.

Changes after the v1.0 tag updated the generated tool reference, fixed the documented source-install command, refreshed Ruff settings, and corrected screenshot size accounting for the MCP JSON envelope.

## Use purpose-built tools first

The current upstream tool reference exposes 26 tools. They cover:

- scene and blend-file summaries, including missing files and linked libraries;
- object summaries and collection hierarchy;
- window and area screenshots;
- workspace and viewport navigation;
- thumbnail and viewport renders;
- Blender Python API and manual search;
- Python execution in an interactive Blender session;
- background CLI variants for blend-file inspection and code execution.

Discover the live list at runtime. Prefer a summary, navigation, documentation, or render tool when one fits. Use `execute_blender_code` only when a narrower tool cannot do the job.

## Safe Blender workflow

1. Confirm the intended `.blend` file and whether Blender is in interactive or background mode.
2. Inspect the scene hierarchy, active object, selection, mode, units, and linked data before editing.
3. Resolve or generate the StudioTwin asset. For paid work, confirm the live cost before submission.
4. Import the completed asset once. Capture the actual object, collection, material, image, or world names Blender creates.
5. Check scale, axis orientation, transforms, materials, image paths, color space, and collection placement.
6. Render a thumbnail or viewport proof when visual quality matters.
7. Save only when the user has authorized it, then verify the file path and dirty state.

## Blender details that matter

- Objects and their data blocks are separate. Check whether a data block has multiple users before modifying it.
- Operators depend on context, mode, active object, and selection. Set these deliberately before calling `bpy.ops`.
- Blender appends `.001`, `.002`, and similar suffixes on name collisions. Keep direct references instead of guessing the final name.
- Link objects created through the data API to a collection or they will not appear in the scene.
- Use `bmesh` for edit-mode mesh changes and flush updates back to the mesh.
- Update the dependency graph before reading computed transforms or modifier results.
- Interactive Blender supports deferred responses for long operations. Background mode requires synchronous completion and rejects deferred results.

## StudioTwin asset checks

For environment maps, verify world-node assignment, projection, color space, and viewport or rendered lighting.

For materials, verify each imported texture role, image color space, node links, UV scale, and the target material slots.

For meshes, verify geometry, normals, transforms, dimensions, material links, texture paths, and collection placement.

Do not report success from the cloud job alone. The Blender scene is the final proof.

## Authorities

- Blender MCP project: https://projects.blender.org/lab/blender_mcp
- Blender MCP documentation: https://www.blender.org/lab/mcp-server/
- Blender extension repositories: https://docs.blender.org/manual/en/latest/editors/preferences/extensions.html#repositories
