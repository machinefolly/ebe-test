# Playbook: Add a Platform Target

Add a new OS/architecture to the build matrix.

## Prerequisites

- You understand the build pipeline (`scripts/build.py`)
- The target has CGO support or is pure Go (like WASM)

## Steps

### 1. Open the build script

Open `scripts/build.py`. Find the `build_target_matrix()` function (around line 204).

### 2. Add your target

Add a new `Target(...)` entry to the matrix list. Example for adding `linux/arm64`:

```python
Target(goos="linux", goarch="arm64", name="linux-arm64",
       output_ext="", output_name="latest"),
```

Each target needs:
- `goos` — the GOOS value (linux, darwin, windows, js)
- `goarch` — the GOARCH value (amd64, arm64, arm, wasm)
- `name` — a unique identifier for this target
- `output_ext` — `.exe` for Windows, `.wasm` for WASM, empty string otherwise
- `output_name` — `latest` plus the extension

### 3. Update feasibility logic (if needed)

In `_evaluate_feasibility()`, the default is: host platform = feasible, JS/WASM = feasible, everything else = not feasible (needs cross-compiler).

If your target needs special handling, add a condition before the default.

### 4. Test the build

```bash
python3 scripts/build.py --verify
```

Your new target should appear in the feasibility report.

### 5. Verify the artifact

If the build succeeds, check that the artifact appears:

```bash
ls releases/{goos}/{goarch}/app/
```

## Troubleshooting

**Target shows "NOT POSSIBLE"**
You need a cross-compiler toolchain for that platform. For example, building for darwin from Linux requires `osxcross`.

**Build fails with "exec: pkg-config: not found"**
Install pkg-config: `sudo apt-get install -y pkg-config`

**Build fails with missing headers**
Install the X11 dev packages for your target platform.
