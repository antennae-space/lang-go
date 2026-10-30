# Go for Web IDE

The Go language pack of [Web IDE](https://github.com/antennae-space/web-ide): the highlighting of Go, the language server gopls (diagnostics, completion, navigation, rename, formatting, the structure of a file), the debug adapter Delve and the test pattern `{name}_test.go`. An official plugin: the IDE installs it by default.

## What it needs

- **Install from the IDE**: a Go file without gopls shows a banner, *Install gopls* (also in *Settings → Plugins*): the IDE runs `go install golang.org/x/tools/gopls@latest` in a console, into its own folder (`~/.local/share/web-ide/tools/lang-go`). You need Go.
- **gopls** on your PATH (`go install golang.org/x/tools/gopls@latest`).
- The IDE runs it **confined**: it reads your project (not write), your Go toolchain (`GOROOT`: `$GOROOT`, the folder of the `go` on your PATH, `/usr/local/go`…) and your module cache (`GOMODCACHE`), keeps its caches in its own folder, and has no network (`GOTOOLCHAIN=local`, `GOPROXY=off`: download the modules of a project with `go mod download` in a terminal).
- On a project over SSH, gopls runs on its host, as you installed it there.

## Debug

- **dlv** (`dlv dap`, [Delve](https://github.com/go-delve/delve)) is the adapter: `go install github.com/go-delve/delve/cmd/dlv@latest`, or the IDE installs it the same way (*Settings → Plugins*, Debug) into its own folder.
- The IDE starts it as `dlv dap --listen 127.0.0.1:<port>` and speaks DAP to it. **Not confined**: it traces your program (ptrace) and runs it, as a command you saved would. On an SSH project it runs on the host of the project.
- Configurations of `.ide/debug.json`, from the templates of the pack: *Launch the package* (`program: ${workspaceFolder}`), *Test the package* (`mode: test`), *Attach to a process* (`processId: ${env:PID}`).

## Permissions

| | |
|---|---|
| `program:gopls` | run gopls, confined |
| `path:GOROOT` | read your Go toolchain |
| `path:GOMODCACHE` | read your module cache |
| `debugger:go` | run dlv for it, not confined |

## Develop

A pack is data: `plugin.json` (checked by `node scripts/check.mjs` against the [plugin SDK](https://github.com/antennae-space/plugin-sdk)'s schema) and `grammars/go.json` (a grammar of the IDE's editor). `./scripts/pack.sh` builds the archives; a tag `v*` publishes a release.

## License

MIT.
