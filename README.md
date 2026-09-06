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

- **Blender is live.** The workflow pairs StudioTwin's cloud generation and asset library with Blender Lab's official Blender MCP v1.0 surface.
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

The Blender connector guidance tracks Blender Lab's official project at [projects.blender.org/lab/blender_mcp](https://projects.blender.org/lab/blender_mcp). The GA update was checked against upstream `main` at commit `4309a39646e644261624bfcd2bca669b343b7621`, the v1.0.0 release manifest, current installation notes, and the generated tool reference.

## License

[MIT](./LICENSE) © 2026 RealTwin Solutions Inc.
