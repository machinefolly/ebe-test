# Playbook: Run and Debug

Build your app and run it. Debug when things go wrong.

## Prerequisites

- You have completed [fork_and_rename.md](fork_and_rename.md)
- Go and Python are on PATH

## Steps

### 1. Build your app

```bash
python3 scripts/build.py --verify
```

This builds all targets your host can support and verifies the artifacts.

### 2. Run the built artifact

```bash
python3 scripts/run_artifact.py
```

This finds the correct artifact for your platform and runs it.

### 3. If you need to rebuild after changes

Just run the build again:

```bash
python3 scripts/build.py --verify
```

No flags needed. It always builds everything possible.

## Debugging

### Check what the build produced

```bash
ls releases/
```

You should see directories like `linux/`, `windows/`, `darwin/`, `js/`.

### Run with Go directly (faster iteration)

```bash
go run ./cmd/app/
```

This skips the build pipeline and runs your code directly. Good for quick testing.

### Check build logs

Build reports are saved to `reports/build/`. Each run creates a timestamped JSON file:

```bash
ls reports/build/
cat reports/build/build-report-*.json | python3 -m json.tool
```

### Common errors

**"cannot find module"**
```bash
go mod tidy
```

**"GLFW library not initialized" (Linux)**
You need X11 development headers:
```bash
sudo apt-get install -y libx11-dev libxcursor-dev libxrandr-dev libxinerama-dev libxi-dev libgl-dev libxxf86vm-dev
```

**"DISPLAY environment variable is missing"**
You're in a headless environment (SSH, WSL without GUI). The binary works — it just can't open a window here.

**Build succeeds but window is blank**
Check that your font files are in `cmd/app/_assets/` if you're using custom fonts.

## Verification

After building and running, confirm:
- [ ] The window opens
- [ ] The window title matches what you set in `cmd/app/main.go`
- [ ] The app renders something (not a blank window)
