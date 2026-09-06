# Jobs, imports, and editor changes

## Run asynchronous generation safely

1. Read the live submission schema.
2. Confirm inputs, cost, and authorization.
3. Submit once.
4. Record the job ID exactly.
5. Poll the existing job at the interval returned by the service.
6. Inspect warnings, outputs, and asset metadata when it completes.
7. Keep the job ID and error text when it fails.

A timeout or disconnect does not prove that the service rejected the submission. Recover the original job before considering another paid call.

A polling interval is not a completion estimate. Do not promise a runtime unless the live service provides one.

## Import by asset ID

A completed job can produce an asset UUID and one or more files. Prefer the asset UUID as the durable handoff between connectors.

Before importing:

- confirm the target Blender file or Unreal project;
- confirm the destination collection, folder, or content path;
- resolve the current asset metadata;
- check file type, expected roles, scale, and orientation;
- distinguish import from placement and saving.

If generation succeeded but import failed, retry the import with the same asset. Do not rerun generation unless the source asset itself is unusable.

## Verify Blender imports

Capture the actual names created by Blender. Check:

- object and data-block types;
- collection placement;
- transforms, dimensions, normals, and axis orientation;
- material slots and node links;
- texture paths and image color spaces;
- world-node assignment for environment maps;
- missing external files;
- the final viewport or rendered appearance.

Blender may suffix names on collision. Keep direct references instead of reconstructing names later.

Saving is a separate mutation. Confirm the destination path and permission before saving, then verify the file path and dirty state.

## Verify Unreal imports

StudioTwin may download source files into `<ProjectRoot>/.studiotwin/` before import. After completion, check:

- Unreal object paths and asset classes;
- expected materials, textures, meshes, sounds, or animations;
- missing roles and unresolved source files;
- actors, sequences, or animation changes in the active level;
- destination naming and path.

An import can be partly successful. Report missing pieces instead of reducing the result to a single success state.

## Keep a provenance record

Record:

- connector and operation;
- job ID and asset ID;
- source paths or media references;
- imported object names or Unreal object paths;
- scene or project mutations;
- warnings and verification status.

Exclude credentials, API keys, and signed download URLs.
