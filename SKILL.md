---
name: "studiotwin-mcp"
description: "Generate environments, PBR materials, 3D meshes, character motion, and sound with StudioTwin, then bring the results into Unreal Engine, Blender, or a web pipeline. Use when an agent needs StudioTwin setup, generation, asset transfer, scene import, or troubleshooting through MCP."
author: RealTwin Solutions Inc.
version: "2.0.0"
license: MIT
---

# StudioTwin MCP

StudioTwin turns prompts and source media into production assets. This skill tells an agent how to choose a connector, run paid generation safely, move assets between tools, and verify the result inside the target application.

The connected MCP server remains the authority for tool names, schemas, defaults, costs, and limits. Read each live definition before calling it.

## Get the skill

```bash
curl -fsSL https://raw.githubusercontent.com/realtwin/studiotwin-mcp-skill/main/SKILL.md -o SKILL.md
```

This command downloads the entry file. The linked references live in the same repository. Clone the repository when the task needs connector setup or troubleshooting:

```bash
git clone https://github.com/realtwin/studiotwin-mcp-skill
```

## Choose the connector

StudioTwin uses one cloud asset library across its connectors.

- **Blender: live.** The StudioTwin Blender add-on generates and imports assets from the 3D View sidebar. Its agent bridge works with the third-party MCP for Blender project when StudioTwin verbs appear in live discovery. See [references/connectors/blender-mcp.md](references/connectors/blender-mcp.md).
- **Unreal Engine: live.** The StudioTwin plugin exposes its toolkits through Epic's Unreal MCP plugin inside the Editor. See [references/connectors/ue-mcp.md](references/connectors/ue-mcp.md).
- **Remote web MCP: not yet a standalone public connector.** StudioTwin uses this host-neutral surface behind connector workflows. Do not advertise a public endpoint until StudioTwin publishes one. See [references/connectors/web-mcp.md](references/connectors/web-mcp.md).

If the host exposes a newer StudioTwin surface, use live discovery and follow its definitions instead of guessing from this file.

## Asset IDs connect the workflow

A successful generation creates an asset with a UUID. Keep that ID. It lets the user generate once and import the same result into Blender, Unreal Engine, or a web pipeline.

Do not rerun a paid generation just to move an asset between hosts. Resolve or import the existing asset by ID.

## Start each session

1. Discover the available MCP servers and tools.
2. Identify the host from the tools that are actually present:
   - StudioTwin toolkits behind `ModelContextProtocol` at a local Unreal endpoint: use the Unreal connector.
   - Blender scene, object, screenshot, or code-execution tools: use the Blender connector. Confirm the separate `studiotwin_*` verbs before attempting StudioTwin work through the agent.
   - `studiotwin_*` platform tools without an editor: use the remote connector only if the deployment is authorized.
3. Read the selected connector reference.
4. Read the live definition for every tool you intend to call.
5. Confirm the requested deliverable, cost boundary, destination, and allowed scene changes.

If no StudioTwin tools appear, help the operator connect the right surface. Account and API-key setup starts in [references/onboarding/register.md](references/onboarding/register.md).

## Operating policy

### Separate the stages

Treat these as distinct actions:

- paid cloud generation;
- job polling and asset resolution;
- downloading or importing an asset;
- changing a Blender scene or Unreal project;
- saving, rendering, exporting, or publishing.

Authorization for one action does not cover the others. Reusing an existing asset is preferable to generating a replacement.

### Inspect before calling

Read the current tool definition immediately before use. Check required inputs, path formats, accepted media, job behavior, expected outputs, and credit cost.

If a paid operation does not expose a cost or estimate, say the cost is unknown and ask for approval before submitting it. Never copy a price from another connector.

### Submit once, then poll

For asynchronous jobs:

1. Submit one generation.
2. Record the returned job ID exactly.
3. Poll that job at the interval provided by the server.
4. Do not resubmit after a timeout or ambiguous transport response until you have checked whether the original job exists.
5. Continue to import or scene work only after the job has completed successfully.

### Protect the working scene

Before an editor mutation, confirm the open file or project, destination collection or content path, source asset, scale and orientation expectations, and whether saving is allowed.

Inspect first. In Blender, prefer the purpose-built summary and navigation tools before `execute_blender_code`. In Unreal, verify object paths and the active level before placement or sequence work.

### Verify the deliverable

A completed cloud job is not the finished task. Confirm the requested files or in-editor objects exist and inspect warnings, missing material roles, scale, orientation, naming, and scene placement.

Do not claim that a file was saved, a scene was clean, or an operation was undoable unless the current host proves it.

## Report back

Include:

- connector and tools used;
- generation job ID and asset ID, when applicable;
- source and destination paths or object names;
- scene or project changes;
- warnings and partial results;
- what you verified and what still needs inspection.

Never expose API keys, credentials, signed URLs, or other temporary secrets.

## References

- [Connector setup](references/setup.md)
- [Capability selection](references/capabilities.md)
- [Jobs, imports, and editor changes](references/operations.md)
- [Prompting and source preparation](references/content-guidance.md)
- [Troubleshooting](references/troubleshooting.md)
- [Account and API key](references/onboarding/register.md)
- [Plugin installation](references/onboarding/plugins.md)
- [Credits](references/onboarding/credits.md)
