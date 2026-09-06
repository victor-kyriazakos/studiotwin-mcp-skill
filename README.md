# StudioTwin MCP skill

An [Agent Skill](https://modelcontextprotocol.io) for generating production assets with StudioTwin and using them in Unreal Engine, Blender, and web pipelines.

The skill focuses on operating judgment rather than copying MCP schemas into documentation. Connected servers provide the current tool names, inputs, outputs, limits, and costs.

## Install

Download the entry file:

```bash
curl -fsSL https://raw.githubusercontent.com/realtwin/studiotwin-mcp-skill/main/SKILL.md -o SKILL.md
```

Clone the full skill when you need its setup and troubleshooting references:

```bash
git clone https://github.com/realtwin/studiotwin-mcp-skill
```

## Connectors

- **Blender sidebar implementation is ready for release recipients; public setup is pending.** The StudioTwin add-on supports sidebar generation and scene import, but this repository cannot point users to a public package. Its current bridge also cannot register tools with MCP for Blender 1.9.1.
- **Unreal Engine is live.** StudioTwin toolkits run through Epic's Unreal MCP plugin inside the Editor.
- **Remote web MCP is not yet sold as a standalone public connector.** It supports connector workflows without duplicating the cloud generation contract.

A generation creates a reusable asset ID. The same asset can move into Blender, Unreal Engine, or a web runtime without paying for the generation again.

## What the skill covers

- choosing the correct connector from live discovery;
- onboarding and API-key hygiene;
- credit-aware generation and job polling;
- asset resolution and cross-host transfer;
- Blender scene inspection, import, placement, and render verification;
- Unreal asset import, level placement, animation, and sequence work;
- failure recovery without duplicate paid jobs.

## Repository layout

```text
SKILL.md
references/
  connectors/
    blender-mcp.md
    ue-mcp.md
    web-mcp.md
  onboarding/
    register.md
    plugins.md
    credits.md
  setup.md
  capabilities.md
  operations.md
  content-guidance.md
  troubleshooting.md
```

## Sources used for Blender GA

The StudioTwin guidance was checked against the add-on source at commit `86966c2`. The MCP guidance tracks the third-party [MCP for Blender](https://github.com/ahujasid/blender-mcp) project at upstream commit `c5f35d9cc54451d785ac4c00c48bf9e98a2e8db9` and package version 1.9.1. This project is not made by Blender.

## License

[MIT](./LICENSE) © 2026 RealTwin Solutions Inc.
