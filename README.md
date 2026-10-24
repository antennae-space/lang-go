# Go for Web IDE

The Go language pack of [Web IDE](https://github.com/antennae-space/web-ide): the highlighting of Go, the language server gopls (diagnostics, completion, navigation, rename, formatting, the structure of a file) and the test pattern `{name}_test.go`. An official plugin: the IDE installs it by default.

## What it needs

- **gopls** on your PATH (`go install golang.org/x/tools/gopls@latest`).
- The IDE runs it **confined**: it reads your project (not write), your Go toolchain (`GOROOT`: `$GOROOT`, the folder of the `go` on your PATH, `/usr/local/go`…) and your module cache (`GOMODCACHE`), keeps its caches in its own folder, and has no network (`GOTOOLCHAIN=local`, `GOPROXY=off`: download the modules of a project with `go mod download` in a terminal).
- On a project over SSH, gopls runs on its host, as you installed it there.

## Permissions

| | |
|---|---|
| `program:gopls` | run gopls, confined |
| `path:GOROOT` | read your Go toolchain |
| `path:GOMODCACHE` | read your module cache |

## Develop

A pack is data: `plugin.json` (checked by `node scripts/check.mjs` against the [plugin SDK](https://github.com/antennae-space/plugin-sdk)'s schema) and `grammars/go.json` (a grammar of the IDE's editor). `./scripts/pack.sh` builds the archives; a tag `v*` publishes a release.

## License

MIT.
