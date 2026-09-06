# Remote web connector

**Status: available to approved connector deployments, not yet marketed as a standalone public connector.** Do not advertise a production endpoint until StudioTwin publishes one.

## Purpose

The remote MCP exposes StudioTwin's cloud generation and asset library without requiring an open editor. It serves agents, pipelines, and DCC connectors that handle scene work separately.

A remote generation returns a job ID. Completed jobs expose asset references and temporary download URLs. Blender or Unreal can then import the asset by UUID.

## Transport and authentication

The current deployment contract uses:

- Streamable HTTP at `POST /mcp`;
- one JSON-RPC message per request;
- stateless, tools-only behavior;
- an `x-api-key` header containing a StudioTwin `st_` key;
- per-key rate limits;
- StudioTwin server information and usage instructions during initialization.

Keep the API key in the MCP client's secret configuration. Never place it in chat, logs, tool arguments, or repository files.

## Tool discovery

Generation tools come from StudioTwin's public function registry. Their names, schemas, and credit costs can change with the platform, so discover them through `tools/list`.

The platform surface also covers:

- credit estimates and wallet balance;
- job status, history, and cancellation;
- asset listing and resolution;
- asset upload and upload completion.

Use the live names and schemas. This document describes the workflow rather than freezing the catalog.

## Workflow

1. Estimate the generation cost and check the wallet when useful.
2. Submit one generation and record the job UUID.
3. Poll that job until completion.
4. Resolve the asset UUID.
5. Download the result or import it through Blender or Unreal.
6. For uploads, create the upload session, transfer the file to the returned destination, then complete the upload.

A low-balance response means the user needs more credits. Report it directly and link to [credits.md](../onboarding/credits.md).

## Web pipelines

A web application can use the same resolved assets. Common consumers include HDR environments, PBR texture sets, and GLB meshes in three.js or React Three Fiber.

The connector generates and resolves assets. Scene composition, optimization, and publishing remain separate steps.
