# Troubleshooting

## No StudioTwin tools appear

First identify the intended host.

For Blender:

1. Confirm Blender 5.1 or newer is running with the MCP add-on enabled.
2. Confirm the add-on's local server is running and the host and port match the MCP server configuration.
3. Confirm the MCP client launches `blender-mcp` and can list Blender tools.
4. Check Blender's system console and the MCP client's logs for socket or startup errors.
5. Reconnect after fixing the host-side issue.

For Unreal Engine:

1. Confirm the correct project is open.
2. Confirm StudioTwin 3.0.0 or newer matches the Editor version.
3. Confirm StudioTwin and Epic's Unreal MCP plugin are enabled.
4. Confirm `ModelContextProtocol.StartServer` succeeded and the client uses the same local endpoint.
5. Check the Unreal Output Log for module-load or registration errors.
6. Restart the Editor after plugin changes, then reconnect.

Do not invent a server command, port, package path, or tool name.

## Blender tools appear but StudioTwin tools do not

The Blender MCP connection is working; the StudioTwin surface is not configured or authenticated. Check the connector's StudioTwin configuration and API-key status without pasting the key into chat or logs.

Do not use arbitrary Blender Python as a substitute for a missing paid StudioTwin generation tool.

## A tool differs from this guide

Follow the live definition. Connectors and plugin versions can change names, inputs, costs, and constraints.

## The submission response is ambiguous

Keep the response and transport logs. Look for a job or correlation ID, reconnect if needed, and query the original job. Ask before making another paid submission.

## A job remains running

Follow the retry hint returned by the service. Preserve the job ID and report its current state without estimating a finish time.

## A job failed

Capture the job ID, error, warnings, and sanitized inputs. Correct the specific validation, source, authentication, or balance problem before proposing another attempt.

## The job completed but the asset is missing

Treat this as partial success. Resolve the asset ID and verify the download or import stage separately. Retry import rather than generation when the cloud asset is intact.

In Blender, inspect missing external files, collection hierarchy, created data blocks, and material or world links. In Unreal, inspect object paths, `.studiotwin/` sources, and the Output Log.

## Blender changed the wrong object or context

Stop. Record the current mode, active object, selection, collection, and dirty state. Do not undo, delete, save, or overwrite until the recovery scope is clear.

## Paths are rejected

Determine whether the tool expects a Blender file path, Unreal object path, local filesystem path, URI, asset UUID, or another connector-specific reference. These values are not interchangeable.

## Cost is unclear

Use the live estimate or tool description. If neither provides a cost, state that it is unknown and request approval before the paid call.
