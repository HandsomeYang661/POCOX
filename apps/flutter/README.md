# POCOX Flutter app

The POCOX app for iOS, Android, macOS, Windows and Linux. It reaches the Rust
workspace through [`flutter_rust_bridge`][frb] (the `pocox_bridge` crate in
[`crates/pocox-bridge`](../../crates/pocox-bridge)); bindings under
`lib/src/rust/` are generated and never hand-edited.

## Toolchain

Flutter **3.44.0** is pinned in `.fvmrc`, `pubspec.yaml`
(`environment.flutter`) and CI; bump all three together.

```bash
brew tap leoafarias/fvm && brew install fvm
fvm install 3.44.0 --setup

cd apps/flutter   # from the repository root
fvm flutter pub get
fvm flutter analyze
fvm flutter test
fvm flutter run
```

After editing `crates/pocox-bridge/src/api/`, regenerate the bindings from the
repository root with `flutter_rust_bridge_codegen generate`
([config](flutter_rust_bridge.yaml)). Desktop release builds swap in
`pubspec-desktop.yaml`, which adds the bundled CJK font
([fonts](../../docs/app-ui.md#fonts)).

## See also

- [App UI](../../docs/app-ui.md) and [composer and turns](../../docs/composer-turns.md)
  — design, layout and interaction rules
- [AGENTS.md](../../AGENTS.md) — code rules and verification commands

[frb]: https://cjycode.com/flutter_rust_bridge/
