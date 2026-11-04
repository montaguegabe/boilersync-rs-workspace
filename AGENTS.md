# boilersync-rs-workspace

Private remake workspace for **boilersync** in Rust + Tauri. Do **not** push to or modify the public `boilersync`, `boilersync-desktop`, `boilersync-react`, `boilersync-skills` or `boilersync-workspace` remotes from here.

## Gabe-stated target (2026-11-04)

- Implement **boilersync in Rust**, the same way `multi` was remade in `multi-rs-workspace` (sibling workspace under the multi project): new private workspace, new private repos, old Python trees as **reference only**.
- Keep the command surface, `template.json` and `.boilersync` formats, template syntax (`$${...}` Jinja delimiters, `NAME_*` path placeholders, `.starter` / `.boilersync` extensions) and the `~/.boilersync/templates` cache layout compatible, so existing templates and projects keep working and the `boilersync` skill stays accurate.
- The `boilersync` command on Gabe's Mac is a shim that runs `cargo run --release` against this checkout's `boilersync-rs` crate, so `main` here must always build and pass `cargo test`.
- Prefer Fable coding judgment for crate/layout details Gabe did not specify. Do not invent product scope beyond replacing the Python CLI.

## Status (2026-11-04)

- `boilersync-rs`: CLI and `boilersync-core` implemented with parity to the Python CLI; `cargo test` passes (unit, integration and end-to-end CLI tests). Every `openbase-community/templates` template renders byte-identically to the Python implementation (`init` with the same `--var` values), including `.boilersync` metadata.
- Known deliberate differences: `.boilersync` always ends with a newline (Python dropped it when registering a child), and `--version` prints `boilersync` (Python printed `cursor-multi`).
- `boilersync-rs-desktop`: Tauri 2 + React remake of the Electron `boilersync-desktop` implemented over `boilersync-core` (template generator, project list with `.boilersync` children as sub-repos, diff/commit/history/sync screens, `boilersync://` deep links); builds and runs on macOS, installed to `/Applications` with `scripts/install-local-app.sh`. Not yet done: release signing/notarization, updater feed, Windows/Linux verification, hands-on testing of the project list and settings screens (the scaffold and diff screens were verified on screen), and the `BOILERSYNC_RS_READ_TOKEN` CI secret in `boilersync-rs-desktop`.

## Layout

| Path | Repo | Purpose |
| --- | --- | --- |
| `boilersync-rs/` | montaguegabe/boilersync-rs (private) | Rust boilersync (CLI + `boilersync-core`) |
| `boilersync-rs-desktop/` | montaguegabe/boilersync-rs-desktop (private) | Tauri desktop |

Reference (read-only): the original `boilersync` workspace checkout on the current machine (`boilersync-cli` is the Python source, `boilersync-skills` documents the command surface).

## Conventions

- Workspace git hooks live in `.githooks` (gitleaks on commit and push); `multi hooks status` should report every repo installed.
- Never check in secrets or Gabe personal absolute paths.
- Commit and push each affected private sub-repo, then this workspace root.
