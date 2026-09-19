# Playbook: Pin or Bump Ebitengine

Update the Ebitengine version your project depends on.

## Prerequisites

- The build pipeline works locally

## Steps

### 1. Check current version

```bash
grep ebitengine go.mod
```

This shows the current pinned version.

### 2. Find available versions

```bash
go list -m -versions github.com/hajimehoshi/ebiten/v2
```

### 3. Update to a new version

```bash
go get github.com/hajimehoshi/ebiten/v2@v2.9.9
go mod tidy
```

Replace `v2.9.9` with your target version.

### 4. Build and test

```bash
python3 scripts/build.py --verify
```

If the build succeeds, the new version works with your code.

### 5. Commit

```bash
git add go.mod go.sum
git commit -m "Bump Ebitengine to vX.Y.Z"
```

## Troubleshooting

**Build fails after bumping**
Check the [Ebitengine changelog](https://github.com/hajimehoshi/ebiten/blob/main/CHANGES.md) for breaking changes. You may need to update your code.

**"go get" fails**
Run `go mod tidy` first, then try again.

**Version not found**
Check the exact version tag on the [Ebitengine releases page](https://github.com/hajimehoshi/ebiten/releases).
