---
plan_id: 2026-09-18-160000_phase1-verify-build
title: Phase 1 Verification — confirm make build on Linux host
summary: Run the canonical no-flag build pipeline on this Linux/WSL host and verify all feasible targets produce artifacts. Complete the last open item in Phase 1.
status: current
created_at: 2026-09-18-16:00:00
---

# Phase 1 Verification — confirm make build on Linux host

Key: `[ ]` pending task, `[x]` completed task, `[?]` needs validation, `[-]` closed task

## 1. Run the build pipeline

- [ ] Run `make build` from the repo root
- [ ] Confirm the feasibility report prints all 8 targets with correct feasibility states
- [ ] Confirm `linux/amd64` builds successfully and artifact appears at `releases/linux/amd64/app/latest`
- [ ] Confirm `js/wasm` builds successfully and artifact appears at `releases/js/wasm/app/latest.wasm`
- [ ] Confirm non-host CGO targets (darwin, windows, linux-arm64/arm) are reported as NOT POSSIBLE with reasons

## 2. Verify artifacts

- [ ] Run `make verify` (or `python3 scripts/build.py --verify`)
- [ ] Confirm all built artifacts are non-empty
- [ ] Confirm the build report is persisted to `reports/build/`

## 3. Run the built artifact

- [ ] Run `python3 scripts/run_artifact.py` to launch the host artifact
- [ ] Confirm the Ebitengine window opens and renders

## 4. Record results

- [ ] Append verification results to `journal/` with date
- [ ] Update `plans/current/index.md` with this plan
- [ ] Mark Phase 1 as complete in `ROADMAP.md` if all items pass

## Definition of done

`make build` produces at least the native host artifact and JS/WASM on this Linux host. The feasibility report is accurate. The built artifact runs.
