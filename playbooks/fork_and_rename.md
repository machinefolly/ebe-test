# Playbook: Fork and Rename

Make this boilerplate your own project.

## Prerequisites

- Git installed
- Go 1.26.4 installed (`go version` shows 1.26.4)
- Python 3.10+ installed (`python3 --version`)

## Steps

### 1. Fork the repository

```bash
# On GitHub: click Fork, or use gh CLI:
gh repo fork cjtrowbridge/ebe-boilerplate --clone
```

### 2. Rename the directory

```bash
mv ebe-boilerplate your-project-name
cd your-project-name
```

### 3. Update go.mod module name

```bash
# Replace 'ebe-boilerplate' with your module name:
go mod edit -module your-module-name
go mod tidy
```

### 4. Update the window title

Open `cmd/app/main.go`. Find this line:

```go
ebiten.SetWindowTitle("Hello, Boilerplate")
```

Change it to your app name:

```go
ebiten.SetWindowTitle("Your App Name")
```

### 5. Verify the build works

```bash
python3 scripts/build.py --verify
```

Expected: at least one target builds and is verified.

### 6. Run it

```bash
python3 scripts/run_artifact.py
```

Expected: a window opens showing your app.

## Troubleshooting

**"Go toolchain is not available on PATH"**
Go is installed but not on PATH. Add it:
```bash
export PATH="/usr/local/go/bin:$PATH"
```

**"python3: command not found"**
Install Python 3.10+ or try `python scripts/build.py` instead.

**Build fails with module errors**
Run `go mod tidy` and try again.
