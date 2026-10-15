# @tummycrypt/tinyland-a11y-engine

Accessibility evaluation engine and validators for Tinyland applications:
WCAG contrast analysis, ARIA validation, keyboard navigation checks,
reporting, streaming and orchestration.

## Consume (Bazel only)

This package is distributed through the Bazel module graph, not npm (RU6).

```starlark
# MODULE.bazel
bazel_dep(name = "tummycrypt_tinyland_a11y_engine", version = "1.0.0")
```

```starlark
# BUILD.bazel
npm_link_package(
    name = "node_modules/@tummycrypt/tinyland-a11y-engine",
    src = "@tummycrypt_tinyland_a11y_engine//:pkg",
)
```

with `--registry=https://raw.githubusercontent.com/xoxd-ai/bazel-registry/main`
ahead of BCR. `svelte` (5.57.2) and `axe-core` (4.x) are optional peers that
your own lock provides: the root entry re-exports the Svelte preprocessor and
action, and `./browser` loads `axe-core`.

## Develop

```bash
pnpm install --frozen-lockfile   # lock for npm_translate_lock
bazel build //:pkg
bazel test //...                 # vitest suite and the tsc 7 typecheck
```

`@tummycrypt/tinyland-color-utils` resolves only through Bazel, so the
type check and tests run under Bazel rather than plain `pnpm`.

See [CHANGELOG.md](CHANGELOG.md) for the 1.0.0 migration notes.
