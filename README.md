# klein-plugin-artifacts

Public, content-addressed binary artifacts for Klein plugins. Nothing here is trusted by name: Klein's
provisioning fetches an asset by URL and accepts it only if its SHA-256 equals the pin recorded in the
Klein repository. Every artifact is built reproducibly from pinned sources, so anyone can rebuild it and
arrive at the same digest instead of downloading it.

## Harper (reference plugin)

| | |
|---|---|
| Release | [`harper-2.11.0`](../../releases/tag/harper-2.11.0) |
| Asset | `harper-f4dcb3862584721b631de4d74e685279d86056f981acea4c45552a67e0031bfb.component.wasm` |
| SHA-256 | `f4dcb3862584721b631de4d74e685279d86056f981acea4c45552a67e0031bfb` |
| Size | 138,123,017 bytes |
| What | WebAssembly component (wasm32-wasip2) of the Klein Harper plugin, pre-initialised with Wizer |
| Built from | [`harper-core` 2.11.0](https://crates.io/crates/harper-core/2.11.0) ([automattic/harper](https://github.com/automattic/harper)) and the Klein plugin glue (`plugins/harper/` in the Klein repository), Rust 1.96 |
| Licence | Harper is Apache-2.0: see [`LICENSE-harper-Apache-2.0`](LICENSE-harper-Apache-2.0) |

Verify a download:

```bash
sha256sum harper-f4dcb3862584721b631de4d74e685279d86056f981acea4c45552a67e0031bfb.component.wasm
# f4dcb3862584721b631de4d74e685279d86056f981acea4c45552a67e0031bfb
```

Harper is an optional plugin: Klein's default build and gates do not need it. It exists to exercise the
plugin architecture end to end; plugins are to be distributed through a marketplace later, and this
repository is the interim public store.

## OSV advisory snapshots (`osv-snapshot-<date>`)

klein-rs's supply-chain gate scans every lockfile offline with osv-scanner against one pinned snapshot of the OSV
vulnerability databases, so the gate's verdict depends on the commit alone. The scheduled workflow
`.github/workflows/osv-snapshot.yml` publishes one release per week (and on `workflow_dispatch`): tag
`osv-snapshot-<date>`, asset `osv-snapshot-<sha256>.tar.gz` (the tree `osv-scalibr/<ecosystem>/all.zip` plus
`SNAPSHOT_DATE`) and its `.sha256`. The release notes record each upstream object's `x-goog-generation`.

Unlike the Harper component, this artefact **cannot be rebuilt from pinned sources**: the upstream bucket is rewritten
continuously, so a snapshot is only ever *verified* by its digest, never reproduced. Releases are immutable (repository
setting), so a pinned digest holds for the pin's lifetime. klein-rs pins `OSV_SNAPSHOT_DATE` and `OSV_SNAPSHOT_SHA256`
in `.github/scripts/workflow-lint-tools.sh`; its checksum-verified installer refuses any other bytes.

Data licences and attribution: `LICENSE-osv-data` (RustSec CC0-1.0; PyPA, Go vulnerability database and GitHub
advisory database CC-BY-4.0).
