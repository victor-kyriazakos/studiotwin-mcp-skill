# Unreal Engine connector

**Status: live.** StudioTwin runs inside Unreal Editor and exposes its toolkits through Epic's Unreal MCP plugin.

## Architecture

The StudioTwin plugin handles cloud generation and editor integration. Epic's `ModelContextProtocol` plugin runs the local MCP server inside Unreal Editor and advertises the available toolsets to clients such as Claude Code, Cursor, VS Code, Gemini, and Codex.

The documented default endpoint is `http://127.0.0.1:8000/mcp`. It has no transport authentication and must remain local.

StudioTwin's cloud API uses the `st_` key stored in Unreal Project Settings. Never place that key in chat, MCP arguments, logs, or source control.

## Requirements

- Unreal Engine 5.6, 5.7, or 5.8;
- the StudioTwin build compiled for that exact engine version;
- StudioTwin plugin 3.0.0 or newer;
- Epic's Unreal MCP plugin enabled;
- a valid StudioTwin API key;
- the Unreal MCP server started in the intended project.

StudioTwin builds before 3.0.0 expose editor toolkits but do not register MCP tools. Update before troubleshooting discovery further.

## Capability groups

Discover the current tools at runtime. The stable product groups are:

- Motion: generate, edit, stitch, import, and retarget animation where supported.
- Environment: generate or expand environment maps and derive world content.
- Mesh: generate 3D meshes from source images and import them.
- Material: generate PBR material content from text, images, or textures.
- Audio: generate sound effects from a written brief.

The live definition controls each tool's schema, cost, and async behavior.

## Operating notes

Most cloud generations are asynchronous. Submit once, record the job ID, and poll it. Some workflows also import assets, add actors, load animation, or create sequences in the open project.

Before a mutation, confirm the active project, level, source asset, destination content path, and whether saving is allowed. StudioTwin may place downloaded source files under `<ProjectRoot>/.studiotwin/`.

Afterward, verify object paths, asset classes, expected material or animation roles, actors, sequences, and warnings. A completed job can still leave a partial import.

## Setup

See [../setup.md](../setup.md) for installation, server startup, client configuration, and first discovery.

## Authorities

- [Epic Unreal MCP](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-mcp-in-unreal-editor)
- [StudioTwin installation](https://docs.studiotwin.ai/docs/plugin/installation/)
- [StudioTwin toolkits](https://docs.studiotwin.ai/docs/plugin/toolkits/)
- [StudioTwin credits](https://docs.studiotwin.ai/docs/dashboard/guides/how-credits-work)
