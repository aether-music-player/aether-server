# Developing Aether

The Go server is the repo root and the Vue SPA lives in `webui/`. Internal docs,
including the contributor notes on each subsystem, are in `docs/agents/`. The
desktop player (Tauri 2, Rust) lives in its own repo and opens the SPA built
here.

## Make targets

Tasks are driven by make, from the repo root:

```
make help     # list all targets
make test     # run the go tests
make lint     # run golangci-lint
make verify   # full suite (test, lint, license, benchmark, coverage, ui tests, spec lint)
make build    # snapshot binary via goreleaser
```

`make go-verify` runs the Go checks alone.

## Backend and frontend separately

```
make run                  # backend on :8075
cd webui && npm install
cd webui && npm run dev   # Vite dev server, hot reload
```

Open the URL Vite prints; it proxies `/api` and `/rest` to the backend.
`make run-ui` serves the SPA embedded from `:8075`.

`make run` uses the dev config `zarf/localdata/config.yaml`, the full
annotated example of every setting. It scans `zarf/locallibrary`
(git-ignored); `make sample-library` fills it with ~1 GB of freely licensed
albums from the Internet Archive.

## Release

GitHub Actions publishes a release when a version tag is pushed:

```
make tag version="v0.1.0"
```

Needs a clean `main`; run `make verify` first. CI builds the server binaries,
packages and container image and attaches them to the release. See
[`docs/agents/releasing.md`](docs/agents/releasing.md) for what CI builds.
