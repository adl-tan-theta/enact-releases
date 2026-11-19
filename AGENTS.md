# enact-releases

This repository is a **release-distribution repo**, not an application source tree. It contains only this file, `README.md`, and Git release tags. The actual product (`enact`, a Codex-style CLI agent) is built in the separate `enact-agent` repo and published here as prebuilt binaries via **GitHub Releases**.

There is no source code, package manifest, build system, lint config, or test suite in this repo. Consequently there are no dependencies to install, nothing to build, and no automated tests to run here.

## Cursor Cloud specific instructions

- Scope: This repo only hosts release artifacts. Do not expect `package.json`, `Makefile`, `Dockerfile`, or any service to run. The update script is intentionally a no-op.
- The distributed product is the `enact` CLI. To exercise it, download a published binary from Releases instead of building from source.
- The `gh` CLI is pre-installed and authenticated in Cloud Agent VMs; use it to inspect/download releases:
  - List releases: `gh release list`
  - Latest stable at time of writing: tag `v0.1.2`.
  - Each release publishes per-platform tarballs, e.g. `enact-agent-<ver>-linux-x86_64.tar.gz` and `enact-agent-<ver>-macos-arm64.tar.gz`. Pick the asset matching `uname -m` / `uname -s`.
- Download + run (Linux x86_64) example:
  ```
  gh release download v0.1.2 --repo adl-tan-theta/enact-releases \
    --pattern 'enact-agent-0.1.2-linux-x86_64.tar.gz' --clobber
  tar -xzf enact-agent-0.1.2-linux-x86_64.tar.gz
  ./enact --version   # -> "enact 0.1.2"
  ./enact doctor      # runtime/config/auth/network diagnostics
  ```
- The tarball contains four binaries: `enact` (CLI entrypoint), `enact-app-server`, `enact-exec`, `enact-tui`.
- Do NOT commit downloaded binaries into this repo (each tarball is ~300 MB). Download them to a scratch dir such as `/tmp`.
- Expected sandbox `doctor` warnings that are NOT binary failures: missing `ENACT_API_KEY` (no auth configured), `TERM=dumb` (non-interactive shell), and provider `reachability` failures (restricted network). These are environmental, not defects.
