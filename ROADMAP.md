# Roadmap

A phased plan for turning this repo into a genuinely useful forkable boilerplate for cross-platform Ebitengine apps and games.

**Status keys:** `[x]` done · `[ ]` planned/in progress · `[?]` open question · `[-]` deferred (with reason)

The guiding rule: **every phase should leave the repo in a state a forker can clone and build.** A half-finished feature that breaks the one-command build is worse than no feature.

---

## Phase 0 — Bootstrap *(foundation)*

The agentic pipeline framework is mounted and the repo has a clear split between agent-facing and human-facing docs.

- [x] Mount the `agentic-pipelines` framework submodule at `./agentic-pipelines`
- [x] Keep `AGENTS.md` (agent-facing) separate from `README.md` (human-facing)
- [x] Keep `third_party/apparat/` ignored and untracked (reference-only)
- [x] Write human-facing `README.md` with goals, approach, and quick start
- [x] Write this `ROADMAP.md`

---

## Phase 1 — Build pipeline + placeholder app *(the core promise)*

Deliver the one-command, no-flag cross-platform build. This is the whole point of the boilerplate.

Current scope: the no-flag driver builds only targets it can establish as locally supported. Non-host CGO desktop targets remain deferred until their cross-toolchains can be detected without per-target configuration.

- [x] Add Ebitengine as a public Go module dependency (pinned to v2.9.9 in `go.mod`)
- [x] Create `cmd/app/main.go` — a minimal Ebitengine window showing "Hello, Boilerplate"
- [x] `scripts/build.py` — canonical no-flag entry point: detect host, report feasible/impossible targets, build all feasible ones
- [x] `scripts/build_orchestrator.py` — `BuildPlan`-based engine, one plan per `(goos, goarch, target)`
- [x] `Makefile` — pin Go 1.26.4; invoke the Python build script with `.tools/cache/` for Go caches
- [x] `scripts/run_artifact.py` — run a freshly built artifact with forwarded args
- [x] Output to `releases/{goos}/{goarch}/{target}/latest[.exe]` (`.apk` for Android)
- [x] Verify: `make build` on a Linux host produces linux/amd64 + js/wasm artifacts; non-host CGO targets correctly reported as infeasible (verified 2026-09-18)

**Definition of done:** `make build` works with zero flags on at least one host OS, and the feasibility report is accurate and readable.

### Open questions
- [ ] Should we pin Ebitengine by tag or by commit hash? *(Tentative: tag, with a note on how to bump.)*
- [ ] Is `Ebitenui` worth including as a second module dependency for a placeholder UI, or does it add noise? *(Tentative: skip for now; the placeholder is a single window.)*

---

## Phase 2 — Playbooks + editor integration *(making it forkable)*

Turn the working pipeline into something a human (or an agent) can *extend* without reverse-engineering the repo.

- [x] Write 5–8 playbooks using the canonical playbook template:
  - [x] How to fork and rename the boilerplate
  - [x] How to add a new platform target
  - [x] How to add a new build flag / Go tag
  - [x] How to run and debug a built artifact
  - [x] How to wire the build into CI
  - [x] How to pin or bump Ebitengine
- [x] Add VS Code tasks and launch config for the build (in addition to the existing pipeline tasks)
- [x] Add a `scripts/README.md` inventory (entry points, output layout, verification commands)

**Definition of done:** a forker can complete each playbook task without asking the maintainer.

---

## Phase 3 — Advanced targets *(breadth)*

Extend the target matrix once the core pipeline is solid.

- [?] Android target via gomobile (`android/arm64`, `android/armeabi.v7a`) — deferred: needs Android NDK
- [?] iOS target via gomobile (`ios/arm64`) — deferred: needs macOS + Xcode
- [-] Headless / smoke-test target — not feasible: Ebitengine requires CGO/GLFW, no headless mode
- [x] Optional: WebAssembly target — builds successfully, HTML wrapper in `releases/js/wasm/app/`

### Deferred
- [ ] Patched-gomobile helper (apparat uses a local patch) — revisit only if the stock gomobile path proves flaky.

---

## Phase 4 — CI/CD *(shipping)*

Take the local pipeline to a host.

- [x] GitHub Actions workflow: matrix build across `linux/amd64`, `windows/amd64`, `darwin/arm64`
- [x] Upload `releases/` artifacts to the workflow run
- [ ] Tag-triggered release with versioned artifact names (drop `latest` for tagged builds)
- [x] golangci-lint + govulncheck as required CI gates

### Deferred
- [ ] Automated semver tagging and changelog generation. *(Nice to have; not needed for the core promise.)*

---

## For your own fork

When you fork this, the roadmap is yours to edit. A useful pattern:

1. Copy Phase 1–2 into a `ROADMAP.md` in your fork and mark what you actually need.
2. Treat Phase 3–4 as an *optional menu* — take the targets and CI that fit your project.
3. If you add a target, add one row to the build matrix and one playbook. That's the whole contract.

The invariant to never break: **one command builds everything the host can build, and the report tells you the rest.**
