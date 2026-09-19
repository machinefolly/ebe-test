# Scripts

## Entry points

| Script | Command | What it does |
| --- | --- | --- |
| `build.py` | `python3 scripts/build.py` | Build all feasible targets, no flags |
| `build.py --verify` | `python3 scripts/build.py --verify` | Build + verify all artifacts |
| `run_artifact.py` | `python3 scripts/run_artifact.py` | Run the built artifact for your platform |
| `build_orchestrator.py` | (used by build.py internally) | BuildPlan-based engine, one plan per target |

## Output layout

```
releases/
  linux/amd64/app/latest
  linux/arm64/app/latest
  darwin/arm64/app/latest
  windows/amd64/app/latest.exe
  js/wasm/app/latest.wasm
```

Build reports go to `reports/build/`.

## Verification

```bash
# Build everything possible:
python3 scripts/build.py

# Build and verify:
python3 scripts/build.py --verify

# Run the built artifact:
python3 scripts/run_artifact.py

# Or use make:
make build
make verify
```

## Go caches

Build caches are isolated to `.tools/cache/` (not the global Go cache). This keeps CI and dev environments in sync.
