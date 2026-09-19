# APIVerve Release Assets Action

> Generate QR codes, barcodes, and badges for your GitHub releases

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Release_Assets-blue?logo=github)](https://github.com/apiverve/action-release-assets)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**[Browse All APIs](https://apiverve.com/marketplace?utm_source=github&utm_medium=action&utm_campaign=release-assets)** | **[Get Free API Key](https://dashboard.apiverve.com/signup?utm_source=github&utm_medium=action&utm_campaign=release-assets)** | **[Documentation](https://docs.apiverve.com?utm_source=github&utm_medium=action&utm_campaign=release-assets)**

---

## What does this action do?

This action provides access to APIVerve's Release Assets APIs directly in your GitHub workflows:

- Generate QR codes linking to release downloads
- Create barcodes for version tracking
- Add scannable codes to release notes

### Available APIs

| API | Description |
|-----|-------------|
| `qrcodegenerator` | QR Code Generator creates customizable QR codes with support for colors, gradients, logos, and various styling options. Generate professional QR codes for marketing, packaging, and digital experiences. |
| `barcodegenerator` | Barcode Generator creates downloadable barcode images from text or product codes in Code 128 or Code 39 formats. Each response provides a temporary image download link, file format, and expiration timestamp. |
| `qrcodereader` | QR Code Reader decodes text and web links from uploaded QR code images. Send a JPG, PNG, or GIF file up to 10 MB to retrieve the raw encoded payload, with paid plans adding corner coordinates. |

---

## Quick Start

```yaml
- name: Release Assets
  uses: apiverve/action-release-assets@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: qrcodegenerator
    params: '{"value": "https://github.com/${{ github.repository }}/releases/tag/${{ github.ref_name }}", "size": 300}'
    output_file: ./release-qr.png
```

---

## Setup

### 1. Get Your API Key

Sign up for a free account at [dashboard.apiverve.com/signup](https://dashboard.apiverve.com/signup?utm_source=github&utm_medium=action&utm_campaign=release-assets) and create an API key.

### 2. Add Secret to Repository

Go to your repository **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

- Name: `APIVERVE_KEY`
- Value: Your API key from the dashboard

### 3. Use in Workflow

```yaml
- name: Release Assets
  uses: apiverve/action-release-assets@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: qrcodegenerator
    params: '{"your": "parameters"}'
```

---

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `api_key` | Your APIVerve API key (or set `APIVERVE_API_KEY` env var) | Yes* | - |
| `api` | API to use: `qrcodegenerator`, `barcodegenerator`, `qrcodereader` | No | `qrcodegenerator` |
| `params` | JSON parameters for the API | No | `{}` |
| `output_file` | Path to save binary output (images, PDFs) | No | - |
| `format` | Response format: `json`, `yaml`, or `xml` | No | `json` |
| `fail_on_error` | Fail workflow if API returns error | No | `true` |
*\*API key is required but can be provided via input OR `APIVERVE_API_KEY` / `APIVERVE_KEY` environment variable.*

## Outputs

| Output | Description |
|--------|-------------|
| `result` | Full API response as JSON |
| `data` | The `data` field from response as JSON |
| `status` | API status (`ok` or `error`) |
| `file` | Path to downloaded file (if `output_file` was used) |
---

## Examples

### QR Code for Release

Generate a QR code linking to the release page

```yaml
- name: QR Code for Release
  id: release-assets-0
  uses: apiverve/action-release-assets@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: qrcodegenerator
    params: '{"value": "https://github.com/${{ github.repository }}/releases/tag/${{ github.ref_name }}", "size": 300}'
    output_file: ./release-qr.png

- name: Upload artifact
  uses: actions/upload-artifact@v4
  with:
    name: qrcodegenerator-output
    path: ./release-qr.png
```

### Barcode for Version

Generate a barcode with the version number

```yaml
- name: Barcode for Version
  id: release-assets-1
  uses: apiverve/action-release-assets@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: barcodegenerator
    params: '{"data": "${{ github.ref_name }}", "type": "code128"}'
    output_file: ./version-barcode.png

- name: Upload artifact
  uses: actions/upload-artifact@v4
  with:
    name: barcodegenerator-output
    path: ./version-barcode.png
```


---

## Full Workflow Example

```yaml
name: Release Assets Workflow

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  release-assets:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Release Assets
        id: result
        uses: apiverve/action-release-assets@v1
        with:
          api_key: ${{ secrets.APIVERVE_KEY }}
          api: qrcodegenerator
          params: '{"value": "https://github.com/${{ github.repository }}/releases/tag/${{ github.ref_name }}", "size": 300}'
          output_file: ./release-qr.png

      - name: Show result
        run: |
          echo "Status: ${{ steps.result.outputs.status }}"
          echo "Data: ${{ steps.result.outputs.data }}"
```

---

## Related Actions

Looking for more APIVerve actions?

- [apiverve/action](https://github.com/apiverve/action) - Generic action for all 350+ APIs
- [apiverve/action-visual-testing](https://github.com/apiverve/action-visual-testing) - Capture screenshots and generate PDFs for visual regression testing and documentation
- [apiverve/action-dns-monitor](https://github.com/apiverve/action-dns-monitor) - Verify DNS configuration, check propagation, and validate DNSSEC after deployments
- [apiverve/action-domain-health](https://github.com/apiverve/action-domain-health) - Monitor domain expiration, WHOIS changes, and domain availability

**[Browse all APIVerve Actions →](https://github.com/marketplace?query=apiverve)**

---

## Pricing

- **Free tier** - Get started with generous free limits
- **Pro plans** - Higher rate limits and priority support for production use

Check out [pricing details](https://apiverve.com/pricing?utm_source=github&utm_medium=action&utm_campaign=release-assets).

---

## Resources

- **API Documentation**: [docs.apiverve.com](https://docs.apiverve.com?utm_source=github&utm_medium=action&utm_campaign=release-assets)
- **API Marketplace**: [apiverve.com/marketplace](https://apiverve.com/marketplace?utm_source=github&utm_medium=action&utm_campaign=release-assets)
- **Issues & Support**: [GitHub Issues](https://github.com/apiverve/action-release-assets/issues)
- **Email**: support@apiverve.com

---

## License

MIT - see [LICENSE](LICENSE)

---

Built by [APIVerve](https://apiverve.com?utm_source=github&utm_medium=action&utm_campaign=release-assets) - 350+ APIs for developers
