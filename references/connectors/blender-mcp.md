# Blender connector

**Status: live.** The StudioTwin Blender add-on generates materials, environment maps, meshes, and sound from the 3D View sidebar. It downloads completed outputs and imports supported assets into the current scene.

Agent control uses the third-party [MCP for Blender](https://github.com/ahujasid/blender-mcp) project. It is not made by Blender or the Blender Foundation.

## Components

The StudioTwin add-on contains:

- a REST client for job submission, cost estimates, polling, asset resolution, and downloads;
- a sidebar operator that polls jobs with `bpy.app.timers` without blocking Blender;
- importers for glTF meshes, world environments, and Principled BSDF materials;
- a bridge that registers StudioTwin verbs when a supported MCP for Blender package is present.

MCP for Blender has two processes:

```text
MCP client <-> MCP over stdio <-> blender-mcp <-> TCP socket <-> Blender add-on
```

The default socket is `localhost:9876`. Keep it local.

## Install

StudioTwin requires Blender 4.2 or newer.

1. Download the StudioTwin Blender add-on zip supplied with the release.
2. In Blender, open **Edit > Preferences > Add-ons > Install**.
3. Select the zip and enable **StudioTwin**.
4. Open the StudioTwin add-on preferences and enter the `st_` API key.
5. Leave the production API URL at `https://api.studiotwin.ai` unless StudioTwin supplied another deployment URL.

For agent control, install MCP for Blender:

```bash
uvx blender-mcp install-addon
```

Configure the MCP client to launch:

```text
uvx blender-mcp
```

Enable **Interface: MCP for Blender** in Blender, open the **MCP for Blender** sidebar, and start its server. Run one MCP server instance per Blender session.

## Confirm the bridge

Discover the live tool set before using StudioTwin through an agent. The StudioTwin add-on currently defines these bridge verbs:

- `studiotwin_generate(function_name, inputs, kind)` submits a job and schedules automatic import;
- `studiotwin_job_status(job_id)` returns platform status;
- `studiotwin_import_asset(asset_id, kind)` resolves and imports an existing asset;
- `studiotwin_import_outputs(job_id, kind)` imports the outputs of a completed job.

Treat the live schemas as authoritative. If these verbs do not appear, the MCP bridge is not active. The StudioTwin sidebar can still generate and import assets, but the agent must stop rather than inventing a substitute call.

The bridge uses MCP for Blender's internal registration surface. Upstream releases can change that surface. Check Blender's console for either a registration list or a message that the bridge needs an update.

The current StudioTwin source probes for `register_handler` or `add_handler`. MCP for Blender 1.9.1 does not expose either symbol. Treat 1.9.1 as unverified for agent control until a matching StudioTwin add-on ships. This does not affect the StudioTwin sidebar workflow.

## Generate from Blender

Open **3D View > Sidebar > StudioTwin > Generate Asset**. Choose a function and enter the prompt. The add-on submits the job immediately and polls in the background.

Current automatic import paths are:

- glTF or GLB mesh into the scene;
- HDR, EXR, or PNG environment map into the current world;
- PBR texture outputs into a new Principled BSDF material;
- other outputs, including audio, downloaded to the local StudioTwin temporary directory.

The `kind` value controls the importer. If it is wrong or unknown, the add-on downloads files without attaching them to the scene. Audio is downloaded but is not added to the sequencer in the current release.

## Use asset IDs

A StudioTwin asset UUID is the durable handoff between tools. `studiotwin_import_asset` resolves the asset through the platform, downloads its current file, and passes it to the selected Blender importer.

Generate once and reuse the same asset in Blender or Unreal Engine. Do not pay for another generation just to transfer it.

## Latest MCP for Blender changes

This guidance was checked against MCP for Blender package version 1.9.1 at commit `c5f35d9cc54451d785ac4c00c48bf9e98a2e8db9` from 2026-09-05.

Recent upstream changes include:

- the project name changed to **MCP for Blender** to distinguish it from Blender Foundation's separate MCP project;
- `BLENDER_MCP_SAFE_MODE=1` adds bounded validation before generated Python runs;
- Docker packaging for the MCP server;
- Poly Pizza search and import with license and attribution metadata;
- revised add-on layout and capability categories;
- row-size, bounding-box, UTF-8 socket, and screenshot handling fixes;
- telemetry and trajectory-capture changes.

MCP for Blender telemetry is enabled by default. Disable it in the add-on preferences or set `DISABLE_TELEMETRY=true` in the MCP server environment when collection is not wanted. Upstream states that collected prompts, code, screenshots, and trajectory data may be used for research and model training.

Safe mode reduces the scope of arbitrary Python execution but does not remove the need to inspect the scene, confirm mutations, and save deliberately.

## Verify the result

Before editing, inspect the active scene, mode, object, selection, units, and collections.

For environment maps, check the world nodes, projection, color space, and rendered lighting.

For materials, check every returned texture role, image color space, node link, UV scale, and target material slot. The current importer wires base color, normal, roughness, and metallic maps.

For meshes, check imported object names, geometry, normals, transforms, dimensions, material links, texture paths, and collection placement.

Blender may append `.001` or another suffix on collision. Capture the names returned by the import instead of guessing.

A completed cloud job is not enough. Verify the imported scene state and render a thumbnail or viewport proof when appearance matters. Save only with the user's approval.

## Authorities

- [MCP for Blender](https://github.com/ahujasid/blender-mcp)
- [MCP for Blender package](https://pypi.org/project/blender-mcp/)
