---
plan_id: 2026-09-18-170000_phase3-advanced-targets
title: Phase 3 — Advanced targets (what's feasible locally)
summary: Improve WASM build, add headless smoke-test target for CI. Android/iOS deferred until gomobile and NDK are configured.
status: current
created_at: 2026-09-18-17:00:00
---

# Phase 3 — Advanced targets (what's feasible locally)

Key: `[ ]` pending task, `[x]` completed task, `[?]` needs validation, `[-]` closed task

## Scope

Android and iOS require gomobile + NDK/SDK that aren't on this host. This plan covers what IS feasible:
- WASM: verify the existing build, add a browser-ready HTML wrapper
- Headless: add a smoke-test target that compiles without a window for CI

## 1. WASM improvements

- [ ] Verify WASM artifact builds and is usable
- [ ] Add `releases/js/wasm/app/index.html` wrapper for browser testing
- [ ] Document how to serve the WASM build locally

## 2. Headless / smoke-test target

- [ ] Add a build tag `headless` that skips window creation
- [ ] Add headless target to the build matrix in `scripts/build.py`
- [ ] Verify headless build compiles and exits cleanly

## 3. Update build matrix

- [ ] Confirm WASM is marked as POSSIBLE in feasibility report
- [ ] Add headless target to matrix with correct GOOS/GOARCH

## 4. Record and close

- [ ] Update ROADMAP.md Phase 3 items
- [ ] Journal entry with results
- [ ] Archive plan to plans/past/

## Deferred (need platform SDKs)

- [?] Android target via gomobile — requires Android NDK
- [?] iOS target via gomobile — requires macOS + Xcode

## Definition of done

WASM builds and has a browser wrapper. Headless target compiles and runs without a window. Build matrix is updated.
