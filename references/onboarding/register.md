# Create a StudioTwin account and API key

Use this when a user reaches StudioTwin without an account or key. The operator creates and stores the secret. The agent explains the steps and verifies only what the connector reports.

## Establish the host

Ask where the user is working: Blender, Unreal Engine, or a web pipeline. Then establish whether they already have a StudioTwin account and an active `st_` API key.

For Blender, also check whether Blender MCP is connected. For Unreal, check the StudioTwin plugin version and whether Unreal MCP is running.

Keep the exchange short. Ask only for information needed to choose the next setup step.

## Create the account

Open [app.studiotwin.ai](https://app.studiotwin.ai) and sign in with Google or continue with an email address. There is no application or waiting list.

## Create an API key

Open [Get Started](https://app.studiotwin.ai/dashboard/get-started/) or [API Keys](https://app.studiotwin.ai/dashboard/api-keys):

1. Select **Create key**.
2. Name the key for its machine or project. Separate keys make suspension less disruptive.
3. Add an optional description.
4. Create the key and copy it immediately.

The secret starts with `st_`, belongs to the user's organization, and is shown once.

[API-key documentation](https://docs.studiotwin.ai/docs/dashboard/pages/api-keys)

## Connect the key

For Unreal Engine, open **Edit > Project Settings > Plugins > StudioTwin**, paste the key into **API Key**, and leave **API Endpoint URL** empty unless StudioTwin provided another value.

For Blender or an approved remote client, store the key in the connector's secret configuration. Do not paste it into chat, prompts, logs, or source control.

## Verify

For Blender, list the live Blender and StudioTwin tools, then request a read-only scene summary. A paid generation is not required to prove the Blender MCP link.

For Unreal, open **Tools** and confirm the StudioTwin toolkits are present. An optional end-to-end cloud test can use **Motion Toolkit > Text to Motion**, but it consumes credits and requires approval.

If the key is rejected, check for copied whitespace, confirm that it is active, confirm the organization, and verify the endpoint setting.

## Key hygiene

- Use a separate key per machine or project.
- Store keys in secret configuration, not repositories or documents.
- Suspend a leaked key immediately. Suspension is reversible; deletion is permanent.
