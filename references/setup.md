# Set up StudioTwin MCP

Use this guide when onboarding a user or when discovery fails. The operator handles installation and secrets; the agent connects, discovers tools, and verifies only what the host reports.

## Blender setup

See [connectors/blender-mcp.md](connectors/blender-mcp.md) for the architecture and [onboarding/plugins.md](onboarding/plugins.md) for installation.

The shortest working sequence is:

1. Run Blender 5.1 or newer.
2. Add `https://lab.blender.org/` to Blender's Extensions repositories.
3. Install and enable the MCP add-on.
4. Start the add-on's local server or enable auto-start.
5. Configure the MCP client to launch `blender-mcp`.
6. Add the StudioTwin connector configuration without placing the `st_` key in chat or source control.
7. Start the MCP client and list live tools.
8. Request a blend-file summary before making changes.

The client talks to `blender-mcp` over stdio. The Python server talks to the add-on over a local TCP socket. Keep that socket local.

## Unreal Engine setup

### Requirements

- a project on Unreal Engine 5.6, 5.7, or 5.8;
- the matching StudioTwin build, version 3.0.0 or newer;
- Epic's Unreal MCP plugin (`ModelContextProtocol`);
- an MCP client configuration generated from the intended project;
- a StudioTwin account and API key.

### Install StudioTwin

Use the [StudioTwin installation guide](https://docs.studiotwin.ai/docs/plugin/installation/) or [StudioTwin on Fab](https://www.fab.com/listings/db820954-ce06-47de-bdc0-054b669c1727).

The build must match the project's Unreal version. Under **Edit > Plugins**, confirm StudioTwin is version 3.0.0 or newer, enable it, and restart the Editor.

### Configure the StudioTwin key

If the user needs an account or key, send them to [StudioTwin Get Started](https://app.studiotwin.ai/dashboard/get-started/).

In Unreal Editor:

1. Open **Edit > Project Settings > Plugins > StudioTwin**.
2. Paste the API key into **API Key**.
3. Leave **API Endpoint URL** empty unless StudioTwin supplied a deployment-specific value.
4. Confirm Unreal accepts the key.

Never ask the user to paste the key into chat, logs, or repository files.

### Enable Unreal MCP

1. Open **Edit > Plugins**.
2. Enable **Unreal MCP**.
3. Enable **All Toolsets** only when the user also wants Unreal's default toolsets.
4. Restart Unreal Editor.

The Toolset Registry dependency is enabled automatically.

### Start the local server

Open the Unreal console and run:

```text
ModelContextProtocol.StartServer
```

To select the port explicitly:

```text
ModelContextProtocol.StartServer 8000
```

The documented default endpoint is `http://127.0.0.1:8000/mcp`. Do not expose it remotely.

### Generate client configuration

From the Unreal console, generate the configuration for the user's client:

```text
ModelContextProtocol.GenerateClientConfig Codex
```

Epic documents `ClaudeCode`, `Cursor`, `VSCode`, `Gemini`, `Codex`, and `All`. Use `All` only when several clients need configuration.

The command writes into the project or workspace root. JSON configurations are merged. Epic documents the Codex TOML path as write-once, so inspect an existing stale entry before regenerating it.

## Connect and discover

1. The operator opens the intended Blender file or Unreal project and starts the local connector.
2. The agent starts from the directory containing the client configuration.
3. The agent connects and lists live tools.
4. The agent confirms StudioTwin capabilities and reads their current definitions.
5. Work begins only after the host, target, cost boundary, and allowed mutations are clear.

If discovery fails, use [troubleshooting.md](troubleshooting.md).
