# Install the connector

Choose the host first. Unreal Engine and Blender use different local components but share the same StudioTwin account and cloud asset library.

## Blender

StudioTwin's Blender sidebar handles generation and import. The third-party MCP for Blender add-on can control Blender, but the current StudioTwin bridge cannot register its own tools with MCP for Blender 1.9.1.

Requirements:

- Blender 4.2 or newer for the StudioTwin add-on;
- the StudioTwin Blender add-on release package;
- the MCP for Blender add-on installed and enabled;
- the `blender-mcp` server configured in the MCP client;
- a StudioTwin account and `st_` API key supplied through the connector configuration, never through chat.

Install StudioTwin:

1. Download the StudioTwin Blender add-on zip supplied with the release.
2. Open **Edit > Preferences > Add-ons > Install** and select the zip.
3. Enable **StudioTwin**.
4. Open the StudioTwin preferences and enter the API key. The default production API URL is `https://api.studiotwin.ai`.

Install MCP for Blender:

```bash
uvx blender-mcp install-addon
```

Configure the MCP client to run `uvx blender-mcp`, enable **Interface: MCP for Blender**, then start the socket server from the Blender sidebar. The default host is `localhost` and the default port is `9876`. Run only one MCP server instance for a Blender session.

MCP for Blender is a third-party project, not a Blender Foundation product. Its current release enables telemetry by default. Disable it in the add-on preferences or set `DISABLE_TELEMETRY=true` in the MCP server environment when that collection is not wanted.

Installing both add-ons does not currently expose StudioTwin MCP verbs. Use MCP for Blender for its own scene tools and use the StudioTwin sidebar for StudioTwin generation and import. Do not attempt StudioTwin agent calls until a compatible bridge release is documented.

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
