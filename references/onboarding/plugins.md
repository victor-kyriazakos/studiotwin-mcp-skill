# Install the connector

Choose the host first. Unreal Engine and Blender use different local components but share the same StudioTwin account and cloud asset library.

## Blender

StudioTwin's Blender workflow is live on the official Blender MCP baseline.

Requirements:

- Blender 5.1 or newer;
- Blender Lab's Extensions repository: `https://lab.blender.org/`;
- the MCP add-on installed and enabled in Blender;
- the `blender-mcp` server configured in the MCP client;
- a StudioTwin account and `st_` API key supplied through the connector configuration, never through chat.

Install the add-on:

1. Open Blender Preferences and go to Extensions.
2. Add the Blender Lab repository using `https://lab.blender.org/`.
3. Find the MCP extension, install it, and enable it.
4. Open the add-on preferences. Confirm the local host and port, then start the server or enable auto-start.

Install the MCP server from source when the client does not provide an MCPB package:

```bash
pip install git+https://projects.blender.org/lab/blender_mcp.git#subdirectory=mcp
```

Follow [Blender Lab's MCP documentation](https://www.blender.org/lab/mcp-server/) for client-specific configuration. The server talks to Blender over a local TCP socket; keep that listener local.

After setup, discover tools and request a blend-file summary before changing the scene.

## Unreal Engine

Supported engine versions are UE 5.6, 5.7, and 5.8. The StudioTwin build must match the project's exact engine version.

MCP requires StudioTwin plugin 3.0.0 or newer. Older builds still expose the in-editor toolkits but do not register MCP tools.

Check the installed version under **Edit > Plugins > StudioTwin**. Update using the same method used for the original installation:

- **Fab:** open **Fab > My Library > StudioTwin**, update or reinstall it into the correct engine slot, then enable the plugin and restart Unreal.
- **Manual:** download the current build for the exact engine version and replace the existing `StudioTwin` folder under `Plugins/`.

Do not mix Fab and manual installations. Two copies can cause module-load and discovery failures.

New installs can use [StudioTwin on Fab](https://www.fab.com/listings/db820954-ce06-47de-bdc0-054b669c1727) or the [StudioTwin installation guide](https://docs.studiotwin.ai/docs/plugin/installation/).

Enable both StudioTwin and Epic's Unreal MCP plugin. See [../setup.md](../setup.md) for the server and client configuration.

## Remote or web clients

The remote MCP is not yet marketed as a standalone public connector. Do not publish an endpoint or direct users to an unsupported setup. Follow the deployment-specific StudioTwin documentation when the surface is explicitly available.
