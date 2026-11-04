# wook8170-plugins

English | [한국어](README.ko.md)

A Claude Code plugin marketplace with three plugins. Each plugin lives in its own GitHub repository. This repository holds only the catalog file, `.claude-plugin/marketplace.json`.

## Plugins

### king-wjang-harness

A hook and CLI harness for process discipline. It splits work into a design track, a build track and a ship track. Each track has phases, and each phase ends at a gate. While the design track is open, hooks block writes to source files. A session also cannot end while its turn log is unsettled. The harness does nothing in a project that has no `.harness/` directory.

Repository: <https://github.com/wook8170/king-wjang-harness>

### wb (Agent Workbench)

A workbench for running parallel Claude Code agents in lanes. It routes each task to a model, limits how many lanes and slots run at once, and guards git operations. It also handles handoff and review between sessions. The safety hooks turn on at install. The workflow hooks act only in a repository that has `.wb/config.json`.

Repository: <https://github.com/wook8170/wb-plugin>

### verifying-production-readiness (VPR)

A skill for go/no-go release decisions. A verdict must rest on observed evidence, not on passing tests alone. The skill keeps a defect ledger, and a lint script checks that ledger. The lint rejects a "ship" verdict that has no measured evidence. It also rejects a conditional verdict that names no conditions.

Repository: <https://github.com/wook8170/verifying-production-readiness>

## How they fit together

- The harness sets the phase discipline: what may happen in each phase, and when a phase may close.
- wb sets how agents run: lanes, safety guards and quality checks.
- VPR gives the ship verdict.

The harness and wb both call VPR when they need a release verdict. VPR is not bundled in either of them, so install it as well.

## Install

Add the marketplace once:

```bash
claude plugin marketplace add wook8170/claude-plugins
```

Then install the plugins you want. The marketplace name is `wook8170-plugins`, as set in `marketplace.json`.

```bash
claude plugin install king-wjang-harness@wook8170-plugins
claude plugin install wb@wook8170-plugins
claude plugin install verifying-production-readiness@wook8170-plugins
```

Inside a session, `/plugin marketplace add wook8170/claude-plugins` and `/plugin install <name>@wook8170-plugins` do the same.

The catalog does not pin versions. Each entry points at the default branch of its repository. The version you get comes from that repository's `plugin.json`, so a new version reaches you when the plugin's author raises it there. Run `claude plugin update <name>@wook8170-plugins` to fetch it.

## Requirements

- Node.js 18 or later (the harness and wb run Node scripts).
- Claude Code 2.1.295 or later. wb needs this version for its safety hooks. The other two plugins state no minimum.
- VPR also uses `bash` and `python3` (standard library only).

## Manuals

| Plugin | Where to read |
|---|---|
| king-wjang-harness | [README](https://github.com/wook8170/king-wjang-harness#readme), "Quick start" and "Command reference" sections |
| wb | [docs/en](https://github.com/wook8170/wb-plugin/tree/main/docs/en) (English), [docs/ko](https://github.com/wook8170/wb-plugin/tree/main/docs/ko) (Korean) |
| verifying-production-readiness | [README](https://github.com/wook8170/verifying-production-readiness#readme) and [skill README](https://github.com/wook8170/verifying-production-readiness/tree/main/skills/verifying-production-readiness) |

## License

The catalog in this repository is under the MIT License. See [LICENSE](LICENSE). Each plugin has its own license in its repository. All three are MIT.
