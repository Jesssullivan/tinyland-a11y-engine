# Changelog

## 1.0.0 (2026-10-14)

Major release for the estate stack uplift (RU1, RU10). The runtime API and the
export map are unchanged; the breaking changes are the toolchain, the peer
contract and the distribution channel.

### Changed

- Toolchain: TypeScript 7.0.2 (native compiler), vitest 5.0.3 and vite 8.3.3,
  all pinned exactly (RU5). `tsconfig.json` now uses `module`/`moduleResolution`
  `NodeNext` and lists `types: ["node"]` explicitly (TypeScript 6+ no longer
  includes every `@types/*` package by default).
- Peers: `svelte` is pinned exactly to 5.57.2 (was `^5.0.0`); both `svelte`
  and `axe-core` (`>=4.0.0`) stay optional.
- In-house dependency: `@tummycrypt/tinyland-color-utils` resolves through the
  Bazel module graph at 1.0.0 (`tummycrypt_tinyland_color_utils`), up from
  0.2.3.
- Bazel: aspect_rules_ts 3.10.1 with the TypeScript 7.0.2 extension, Node
  22.22.0. `//:pkg` ships only `dist/` plus docs and carries the color-utils
  link, but never this module's own copies of `svelte` or `axe-core`; the
  consuming app's lock supplies them.
- Distribution: Bazel-only (RU6, RU8). The npm publish workflow, `publishConfig`
  and `prepublishOnly` are removed and the package is `private`. Releases are a
  signed git tag, a GitHub release and an entry in xoxd-ai/bazel-registry.
- Engines: Node `>=22.12.0`.

### Fixed

- Relative imports name their `.js` file, so `dist/` resolves under plain Node
  ESM outside a bundler (ported from the tinyland.dev packages/ copy, #23).
- The Svelte preprocessor keeps its issue list per `validateAccessibility`
  call, so concurrent preprocess calls no longer clear or mix each other's
  findings (#23).

### Migration

1. Replace any npm dependency on `@tummycrypt/tinyland-a11y-engine` with
   `bazel_dep(name = "tummycrypt_tinyland_a11y_engine", version = "1.0.0")`
   from `https://raw.githubusercontent.com/xoxd-ai/bazel-registry/main`, and
   link it with
   `npm_link_package(name = "node_modules/@tummycrypt/tinyland-a11y-engine", src = "@tummycrypt_tinyland_a11y_engine//:pkg")`.
2. Your own lock must provide `svelte` 5.57.2 whenever you import the root
   entry (`.`), which re-exports the Svelte preprocessor (`svelte/compiler`)
   and action, and `axe-core` 4.x when you use `./browser`. Apps still on an
   older Svelte move to the RU1 stack in the same change, because the peer is
   exact.
3. Drop any `single_version_override` or registry overlay for this module and
   for `tummycrypt_tinyland_color_utils` older than 1.0.0, then refresh
   `MODULE.bazel.lock`.
4. No source changes are needed: every export path (`.`, `./browser`,
   `./browser-runtime`, `./validators`, `./plugins`, `./streaming`,
   `./reporting`, `./color`) and every exported symbol is the same as 0.3.0.

## 0.3.0

- `svelte` peer range requires Svelte 5 (#21).
