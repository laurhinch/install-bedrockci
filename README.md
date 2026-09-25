# Install BedrockCI Action

This GitHub Action installs [BedrockCI](https://github.com/laurhinch/bedrockci) for validating Minecraft Bedrock resource and behavior packs in CI pipelines.

## Usage

```yaml
- uses: laurhinch/install-bedrockci@v1
```

Pin a release to keep builds reproducible:

```yaml
- uses: laurhinch/install-bedrockci@v1
  with:
    version: cli-v2.1.0
```

## Inputs

| Name | Default | Description |
| --- | --- | --- |
| `version` | `latest` | Release tag of the BedrockCI CLI to install, e.g. `cli-v2.1.0`. |
| `token` | `${{ github.token }}` | Token used to resolve `latest`. Only reads the public releases API. |

Resolving `latest` costs one GitHub API call. That call is authenticated with
`github.token` by default, because the anonymous limit is 60 requests per hour
per IP and GitHub-hosted runners share addresses. If your workflow restricts
permissions so that no token is available, pin `version` instead.

## Features

- Installs BedrockCI binary for Linux environments
- Supports Ubuntu runners (recommended)
- Verifies the download and fails the step if the binary is bad

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
