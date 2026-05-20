# CAM Releases

Binary-only public releases for CAM (Code Agent Monitor).

This repository hosts release assets published from
`jcyLite/openclaw-plugin-agent-monitor`. Source code and development workflow
live in the source repository; this repository is for downloadable installers
and packages.

## Latest Release

Open the latest GitHub Release:

```bash
gh release view --repo jcyLite/cam-releases --web
```

List release assets from the command line:

```bash
gh release view --repo jcyLite/cam-releases --json tagName,url,assets \
  --jq '{tagName,url,assets:[.assets[].name]}'
```

## Install Or Update CAM CLI

Use the remote installer from the latest release:

```bash
curl -fsSL https://github.com/jcyLite/cam-releases/releases/latest/download/install-remote.sh | bash
```

Install a specific version:

```bash
CAM_VERSION=0.1.10 \
curl -fsSL https://github.com/jcyLite/cam-releases/releases/latest/download/install-remote.sh | bash
```

The release includes local packages for:

- Linux x86_64: `cam-local-<version>-linux-x86_64.tar.gz`
- macOS Apple Silicon: `cam-local-<version>-darwin-arm64.tar.gz`

Each package has a matching `.sha256` file.

## Android APK

Download the Android dashboard APK from the latest release:

```bash
gh release download --repo jcyLite/cam-releases --pattern 'cam-dashboard-*.apk'
```

Or open the Releases page and download:

```text
cam-dashboard-<version>.apk
```

After installing the APK, configure the CAM service address, for example:

```text
http://192.168.1.20:3456
```

Use the dashboard token shown by the CAM bridge when logging in.

## Release Assets

A complete CAM release should contain:

- `cam-dashboard-<version>.apk`
- `cam-local-<version>-linux-x86_64.tar.gz`
- `cam-local-<version>-linux-x86_64.tar.gz.sha256`
- `cam-local-<version>-darwin-arm64.tar.gz`
- `cam-local-<version>-darwin-arm64.tar.gz.sha256`
- `install-remote.sh`

## Publishing

Publish releases from the source repository with:

```bash
scripts/github-cam-release.sh --version 0.1.10 --android-version-code 10 --push
```

That script dispatches `.github/workflows/release-all.yml`, waits for GitHub
Actions, and verifies the assets in this repository.
