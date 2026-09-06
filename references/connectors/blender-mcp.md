# Blender connector

**Status: sidebar implementation available to release recipients; public setup pending; MCP agent bridge blocked.** The StudioTwin Blender add-on generates materials, environment maps, meshes, and sound from the 3D View sidebar. It downloads completed outputs and imports supported assets into the current scene. StudioTwin has not published a public package location, so this skill cannot offer a self-service GA install path.

Planned agent control uses the third-party [MCP for Blender](https://github.com/ahujasid/blender-mcp) project. It is not made by Blender or the Blender Foundation.

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

1. Obtain the StudioTwin Blender add-on zip through the StudioTwin release channel available to your account. Stop if no package was supplied; there is no verified public download location.
2. In Blender, open **Edit > Preferences > Add-ons > Install**.
3. Select the zip and enable **StudioTwin**.
4. Open the StudioTwin add-on preferences and enter the `st_` API key.
5. Leave the production API URL at `https://api.studiotwin.ai` unless StudioTwin supplied another deployment URL.

MCP for Blender itself can be installed with:

```bash
uvx blender-mcp install-addon
```

Configure the MCP client to launch:

```text
uvx blender-mcp
```

Enable **Interface: MCP for Blender** in Blender, open the **MCP for Blender** sidebar, and start its server. Run one MCP server instance per Blender session.

## Agent bridge status

The StudioTwin add-on defines these intended bridge verbs:

- `studiotwin_generate(function_name, inputs, kind)` submits a job and schedules automatic import;
- `studiotwin_job_status(job_id)` returns platform status;
- `studiotwin_import_asset(asset_id, kind)` resolves and imports an existing asset;
- `studiotwin_import_outputs(job_id, kind)` imports the outputs of a completed job.

These verbs do not register through the current setup. The StudioTwin bridge runs inside Blender and imports `blender_mcp.server`, but `uvx` runs that package in a separate Python environment. MCP for Blender 1.9.1 also declares its tools statically with `@mcp.tool()` and exposes neither `register_handler` nor `add_handler`.

Blender agent control is therefore blocked until StudioTwin ships a compatible integration in the external MCP server or another supported registration contract. Do not tell users that installing both add-ons enables the four StudioTwin verbs. The StudioTwin sidebar remains available.

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

MCP for Blender telemetry is enabled by default. Turning telemetry off in the add-on preferences withholds private payloads, but the server still sends minimal anonymous usage records. Set `DISABLE_TELEMETRY=true` in the MCP server environment to disable collection completely. With consent enabled, upstream states that prompts, code, screenshots, and trajectory data may be used for research and model training.

`BLENDER_MCP_SAFE_MODE=1` validates only Python sent through the MCP server's `execute_blender_code` tool. It is not a Blender sandbox and does not cover commands sent directly to the local socket. Without safe mode, that tool runs arbitrary Python with the Blender process's filesystem, network, process, and scene privileges. Never execute untrusted code. Require explicit approval for those privileges, destructive scene changes, and saves.

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
