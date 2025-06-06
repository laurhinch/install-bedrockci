# Install BedrockCI Action

This GitHub Action installs [BedrockCI](https://github.com/laurhinch/bedrockci) for validating Minecraft Bedrock resource and behavior packs in CI pipelines.

## Usage

```yaml
- uses: laurhinch/install-bedrockci@v1
```

## Features

- Installs BedrockCI binary for Linux environments
- Supports Ubuntu runners (recommended)
- No configuration required

## EULA and Privacy Policy

BedrockCI downloads official Minecraft Bedrock server directly from Microsoft. The software is not proxied or modified during download.

By using this tool, you must accept:
- [Minecraft End User License Agreement](https://minecraft.net/eula)
- [Microsoft Privacy Policy](https://go.microsoft.com/fwlink/?LinkId=521839)

The `--accept-eula` flag is required for server downloads. Without it, the download will fail.

## Example Workflow

```yaml
name: Validate Bedrock Packs

on:
  push:
    paths:
      - 'packs/**'

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - uses: laurhinch/install-bedrockci@v1
      
      - name: Download Bedrock Data
        run: bedrockci download --accept-eula
      
      - name: Validate Packs
        run: |
          bedrockci validate \
            --rp ./packs/resource_pack \
            --bp ./packs/behavior_pack \
            --fail-on-warn
```

## License

MIT
