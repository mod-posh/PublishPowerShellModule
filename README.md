# Publish PowerShell Module GitHub Action

This action Publishes a PowerShell module.

## Inputs

### `ModuleName`

**Required** The name of the PowerShell module.

### `Source`

**Optional** The source directory containing the module (relative to the workspace).

### `Output`

**Optional** TThe output directory for storing the module (relative to the workspace).

### `ApiKey`

**Required** The PowerShell Gallery API Key.

### `Debug`

**Optional** Enable debug mode. The default is `false``.

## Example usage

```yaml
name: Build PowerShell Module

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: windows-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          path: 'Repo'

      - name: Create PowerShell Manifest
        uses: mod-posh/CreatePowerShellManifest@v0.0.1.0
        with:
          ModuleName: 'SpnLibrary'
          Source: 'Repo'
          Imports: 'public'
          Debug: 'true'

      - name: Create PowerShell Module
        uses: mod-posh/CreatePowerShellModule@v0.0.1.0
        with:
          ModuleName: 'SpnLibrary'
          Source: 'Repo'
          Imports: 'public'
          Debug: 'true'

      - name: Test PowerShell Module
        uses: mod-posh/TestPowerShellModule@v0.0.1.0
        with:
          ModuleName: 'SpnLibrary'
          Source: 'Repo'
          Debug: 'true'

      - name: Publish PowerShell Module
        uses: mod-posh/PublishPowerShellModule@v0.0.1.0
        with:
          ModuleName: 'SpnLibrary'
          Source: 'Repo'
          ApiKey: ${{ secrets.APIKEY }}
          Debug: 'true'
```
