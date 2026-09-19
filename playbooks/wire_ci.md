# Playbook: Wire Up CI

Set up GitHub Actions to build your app for every platform automatically.

## Prerequisites

- Your code is on GitHub
- The build pipeline works locally (`python3 scripts/build.py --verify`)

## Steps

### 1. Create the workflow directory

```bash
mkdir -p .github/workflows
```

### 2. Create the workflow file

Create `.github/workflows/build.yml`:

```yaml
name: Build

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    strategy:
      matrix:
        include:
          - os: ubuntu-latest
            goos: linux
            goarch: amd64
          - os: ubuntu-latest
            goos: js
            goarch: wasm
          - os: macos-latest
            goos: darwin
            goarch: arm64
          - os: windows-latest
            goos: windows
            goarch: amd64

    runs-on: ${{ matrix.os }}

    steps:
      - uses: actions/checkout@v4

      - name: Set up Go
        uses: actions/setup-go@v5
        with:
          go-version: '1.26.4'

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Build
        env:
          GOOS: ${{ matrix.goos }}
          GOARCH: ${{ matrix.goarch }}
        run: python scripts/build.py --verify

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: app-${{ matrix.goos }}-${{ matrix.goarch }}
          path: releases/
```

### 3. Commit and push

```bash
git add .github/workflows/build.yml
git commit -m "Add CI build workflow"
git push
```

### 4. Check the workflow

Go to your GitHub repo → Actions tab. The build should start automatically.

## Troubleshooting

**Workflow fails on Windows**
The Python script may have path issues. Check that `scripts/build.py` uses `Path` objects (it does by default).

**"Go version not found"**
The `setup-go` action needs the exact version string. Check `go.mod` for the pinned version.

**macOS build fails with CGO errors**
The `macos-latest` runner includes Xcode and CGO toolchains. If it still fails, check that `CGO_ENABLED=1` is set.
