# action-publish-nuget-package

This reusable action simplifies the process of packaging NuGet packages and publishing them to GitHub.

## Publish NuGet package workflow

This GitHub composite action restores, builds, packs, and publishes a .NET project as a NuGet package to GitHub Packages. It is intended for repositories that publish packages under the owner of the repository running the workflow.

### Calling the action

```yaml
# actions.yml in a consumer repository
name: Publish NuGet package

on:
  push:
    branches:
      - main

permissions:
  contents: read
  packages: write

jobs:
  publish:
    uses: glueckkanja/action-publish-nuget-package@sha-hash # v1.2.3
    with:
      dotnet_version: "10.x" # required: version of .NET SDK to use
      project_path: "project/project.csproj" # required: path to the .NET project file
      package_output: "project/bin/Release" # required: output directory for the NuGet package
      package_version: "0.0.1" # required: version of the NuGet package to publish
      assembly_version: "0.0.1" # optional: set specific assembly version
      github_pat: your-pat-token # required: GitHub Personal Access Token with 'write:packages' scope - use GitHub's secret vars for this
```

### Permissions

- `contents: read` allows `actions/checkout` to read the repository files needed to restore, build, and package the project.
- `packages: write` allows the workflow to publish the generated NuGet package to GitHub Packages.

### Inputs

- `dotnet_version` _(string, default: empty)_ – Version of .NET SDK to use (e.g., '10.x').
- `project_path` _(string, default: empty)_ – Path to the .NET project file (e.g., 'project/project.csproj').
- `package_output` _(string, default: empty)_ – Output directory for the NuGet package (e.g., 'project/bin/Release').
- `package_version` _(string, default: empty)_ – Version of the NuGet package to publish (e.g., '1.0.0').
- `assembly_version` _(string, default: package version)_ – (Optional) Set specific assembly version (e.g., '1.0.0'). If not set, it will default to package version. This version usually has the same value as the package_version.
- `github_pat` _(string, default: empty)_ – GitHub Personal Access Token with 'write:packages' scope. Use it as secret variable -> `${{ secrets.GITHUB_TOKEN }}`.

### Outputs

This action has no outputs.
