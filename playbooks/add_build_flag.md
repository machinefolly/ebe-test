# Playbook: Add a Build Flag

Add a Go build tag that changes what gets compiled.

## Prerequisites

- You understand Go build tags
- The build pipeline works (`python3 scripts/build.py --verify`)

## Steps

### 1. Create your build tag files

Go build tags control which files are compiled. Create files with build constraints:

```go
//go:build debug

package main

const DebugMode = true
```

```go
//go:build !debug

package main

const DebugMode = false
```

Put these in `cmd/app/` alongside `main.go`.

### 2. Update the build pipeline

Open `scripts/build.py`. In the `_run_build()` function, find the build command:

```python
cmd = [go, "build", "-buildvcs=false", "-o", str(target.output_path), "./cmd/app"]
```

Add your tag when the environment variable is set:

```python
tags = os.environ.get("BUILD_TAGS", "")
if tags:
    cmd.extend(["-tags", tags])
```

### 3. Build with your tag

```bash
BUILD_TAGS=debug python3 scripts/build.py --verify
```

### 4. Verify

Check that the built artifact behaves differently based on your tag.

## Troubleshooting

**"undefined: DebugMode"**
Your build tag files aren't being picked up. Check that the `//go:build` line is at the very top of the file (before the package declaration).

**Tag doesn't change behavior**
Make sure you have both the tagged and untagged versions. The `!debug` tag means "when debug is NOT set."
