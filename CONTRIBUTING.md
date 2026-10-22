# Contributing to POCOX

Thanks for helping shape POCOX. This page covers setup, commits and pull
requests. The rules for code and design (product contracts, the Rust and Dart
style, verification commands) live in [AGENTS.md](AGENTS.md); read it before
any non-trivial change.

> [!WARNING]
> **Pre-1.0.** CLI flags, on-disk layout, protocols and crate boundaries can
> still change. This guide is about *how* to contribute, not a stability
> promise.

## Before you start

- Open an issue first for non-trivial work
  ([templates](.github/ISSUE_TEMPLATE)); a PR is fine for small, obvious fixes.
- Read the subsystem doc in [docs/](docs/README.md) for the area you change.
- Don't break userspace: documented CLI flags, on-disk layout and wire fields
  are contracts ([AGENTS.md › Product rules](AGENTS.md#product-rules)).

## Prerequisites

| Tool | Version / source |
| --- | --- |
| Rust | pinned by [`rust-toolchain.toml`](rust-toolchain.toml); `rustup` picks it up |
| Flutter | **3.44.0**, pinned in [`apps/flutter/.fvmrc`](apps/flutter/.fvmrc), `pubspec.yaml` (`environment.flutter`) and CI; bump all three together |
| FVM | `brew tap leoafarias/fvm && brew install fvm`, then `fvm install 3.44.0 --setup` |
| `codex` | on `PATH` for flows that host a real app-server; POCOX never bundles a model runtime |
| `flutter_rust_bridge_codegen` | only when editing `crates/pocox-bridge/src/api/` |

## Setup

```bash
git clone --recurse-submodules git@github.com:acking-you/POCOX.git
cd POCOX
git submodule update --init --recursive   # if cloned without submodules
cargo build --workspace
cargo run -p pocox-cli -- --help
(cd apps/flutter && fvm flutter pub get && fvm flutter run)
```

`deps/codex` and `deps/grok-build` are pinned submodules and pb-mapper is a
pinned Git dependency; see
[AGENTS.md › Dependencies and submodules](AGENTS.md#dependencies-and-submodules).
Don't edit code inside `deps/` from this repository.

## Branches and commits

- Branch as `<type>/<short-slug>` (for example `fix/state-toml-permissions`);
  never push directly to `main`.
- Commits follow [Conventional Commits](https://www.conventionalcommits.org/):
  `feat`, `fix`, `docs`, `refactor`, `perf`, `deps`, `ci`, with an optional
  scope. Keep the subject under ~70 characters and put the *why* in the body.
- Code, comments, commits and PR descriptions are in English.

## Verification

Run the commands in [AGENTS.md › Verification](AGENTS.md#verification) before
requesting review; CI runs the same ones scoped to what your change touches.
When you edit `crates/pocox-bridge/src/api/`, regenerate the bindings with
`flutter_rust_bridge_codegen generate`.

## Bumping dependencies

- pb-mapper: push the SDK fix to its `pocox` branch, run
  `cargo update -p pb-mapper`, and commit the lockfile.
- `deps/codex`: follow
  [protocol sync › Bumping deps/codex](docs/app-server-protocol-sync.md).
- Any submodule: bump only on purpose, as its own `deps(<name>): …` commit,
  and update the wrapper crate in the same PR when the upstream API changed.

## Pull requests

- Open against `main` from a topic branch and fill in the
  [PR template](.github/PULL_REQUEST_TEMPLATE.md), especially **Userspace
  impact**.
- Keep PRs focused. When behaviour changes, update its doc under `docs/` and
  note user-visible changes in [CHANGELOG.md](CHANGELOG.md) in the same PR.

## Bugs and features

Use the [issue forms](.github/ISSUE_TEMPLATE). For bugs include
`pocox version`, `codex --version` when relevant, the OS and any `tracing`
output; for features lead with the problem.

## License

POCOX is licensed under the [Apache License 2.0](LICENSE); by contributing you
agree your contributions are licensed under the same terms. Submodules under
`deps/` keep their own licenses.
