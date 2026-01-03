# godot_builds
A library of github build actions for godot

# Usage

```
name: Release

on:
  push:
    tags: ['v*']

jobs:
  release:
    uses: YOUR_USERNAME/godot-workflows/.github/workflows/reusable-release.yml@main
    with:
      game_name: 'MyGame'
      version: ${{ github.ref_name }}
    secrets: inherit
    permissions:
      contents: write
```

# Setting options

Each game repo defines options in GitHub settings:
Secrets

Settings > Secrets and variables > Actions > Secrets
Secret	When Needed
FTP_SERVER	FTP upload enabled
FTP_USERNAME	FTP upload enabled
FTP_PASSWORD	FTP upload enabled
BUTLER_CREDENTIALS	itch.io upload enabled
Passing Secrets to Reusable Workflows

Two options in the caller workflow:

Option 1: secrets: inherit (passes all repo secrets automatically)

jobs:
  release:
    uses: you/godot-workflows/.github/workflows/reusable-release.yml@main
    with:
      game_name: 'MyGame'
      version: ${{ github.ref_name }}
    secrets: inherit

Option 2: Explicit (only pass what's needed)

jobs:
  release:
    uses: you/godot-workflows/.github/workflows/reusable-release.yml@main
    with:
      game_name: 'MyGame'
      version: ${{ github.ref_name }}
      itch_upload_enabled: true
      itch_target: 'myuser/mygame'
    secrets:
      butler_credentials: ${{ secrets.BUTLER_CREDENTIALS }}

Inputs (not secrets)

These are passed directly in the with: block - no repo settings needed:

with:
  game_name: 'SpaceBlaster'
  version: ${{ github.ref_name }}
  godot_version: '4.5-stable'
  test_script: './run-tests.sh'
  main_scene: 'scenes/main.tscn'
  itch_upload_enabled: true
  itch_target: 'myuser/space-blaster'