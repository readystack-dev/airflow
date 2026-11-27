# Apache Airflow 3.4.0 — ReadyStack release `3.4.0-CE-ubuntu24.04-r4`

Generated 2026-11-27 from this release's build records and strategy. Statements that come from a single record name it in brackets; the others come from this release's strategy and build log.

## Release

- **Image:** `readystack/airflow:3.4.0-CE-ubuntu24.04-r4` (linux/amd64). Index digest `sha256:d106ae5c556c4f4fe54b3a3714f395143699b433000ba1c4609e6b4c4088f59f` [`docs/provenance/LOCKED_DIGEST.txt`].
- **Image manifest digest:** `sha256:322cab41ac2503ec78026c829f78662cd5c690367bda4a5c7d1b3ddc71d7371c` [`TARGET_DIGEST.txt`].
- **Upstream:** Apache Airflow 3.4.0 built from upstream's published release artifact https://pypi.org/project/apache-airflow/3.4.0/; its source is `3.4.0` of https://github.com/apache/airflow — upstream notes: https://github.com/apache/airflow/releases/tag/3.4.0 [`docs/provenance/BUILD_RECIPE.json`].
- **Base image:** none (`FROM scratch`): a chiselled Ubuntu 24.04 runtime, the files Canonical's chisel cut from Ubuntu's own packages (`vendored/chisel/slices.txt`, `vendored/chisel/packages.tsv`); runtime user `airflow`.
- **Build:** `source-rebuild-python`, compiled offline with `--network=none`.
- **Supersedes:** ReadyStack release `3.4.0-CE-ubuntu24.04-r3`.

## What changed in this release

- Upstream version unchanged (3.4.0); ReadyStack rebuild.
- Minimum dependency versions ReadyStack enforces on top of upstream (raised only where upstream's own version was lower); shipped versions from the SBOM [`docs/sbom/SBOM.json`]:
  - `dnspython` at least `2.9.2` (shipped: `2.9.2`)
- Apache Airflow 3.4.0 is unchanged from the release this supersedes (3.4.0-CE-ubuntu24.04-r3); this release rebuilds it. Changes: libexpat1 moves from 2.6.1-2ubuntu0.6 to 2.6.1-2ubuntu0.7 (USN-9196-1). Settings, ports and data volumes are those of 3.4.0-CE-ubuntu24.04-r3: replace the container with this image on the same volumes.

## Security status at release (measured 2026-11-25)

- **Docker Scout:** 0 fixable critical/high vulnerabilities; runs as `airflow` (non-root); SBOM + provenance attestations present (SPDX SBOM + SLSA v1) [`docs/provenance/SCOUT_VERDICT.json`].
- **OSV cross-check:** no fixable critical/high vulnerability beyond Docker Scout's findings.
- **Signature:** cosign signature verified by ReadyStack's release gate before publishing; check it yourself with `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/airflow:3.4.0-CE-ubuntu24.04-r4` (signatures are recorded in the public Sigstore transparency log).
- **ReadyStack release check at release:** passed (ReadyStack's own test, not a Docker Scout grade). Vulnerabilities are disclosed every day and the image never changes, so it is re-checked daily, beside Docker Scout's own dated grade, at https://readystack.dev/queue.
- **Before exposing this service:** The admin password is stored in plain text in /opt/airflow/simple_auth_manager_passwords.json.generated and printed to the container log. Treat both as secrets, and use a full auth manager for anything beyond a small trusted deployment.
- **Before exposing this service:** Port 8080 is plain HTTP; put a TLS proxy in front of it before exposing it.

## Reproducibility

Rebuilds offline from this archive to the identical image digest `sha256:322cab41ac2503ec78026c829f78662cd5c690367bda4a5c7d1b3ddc71d7371c`: run `./BUILD_OFFLINE.sh`, then `./VERIFY_OFFLINE.sh` (add `--online` to confirm the Docker Hub tag holds the same digest). Before publishing, ReadyStack rebuilt the kit with `./BUILD_OFFLINE.sh` and compared the result with the published digest [`docs/provenance/REPRO_VERIFICATION.json`], then ran `./BUILD_OFFLINE.sh` and `./VERIFY_OFFLINE.sh` from this archive (without `--online`).

## Validation

- 2 of 2 use cases passed (22 authored, 2 kept in the suite; 20 not exercised, see below) against the running image [`docs/validation/test-results.json`].
- The cases are `GET /api/v2/version` (200), `GET /api/v2/monitor/health` (200); the suite checks nothing else.
- 20 further authored checks were not exercised and are not counted above: 19 needed credentials the build's check does not have; 1 (`GET /`) were answered with the same page the build's test container served for a made-up path, so they could not show the path exists (listed under Not exercised in `docs/validation/FAILED_TESTS.md`).

## Known issues

- **medium** (upstream): Secret written in plain text or to the log by default [`RSD-airflow-1af001779a`]

## Verify this archive

```
sha256sum -c CHECKSUMS.sha256
./BUILD_OFFLINE.sh && ./VERIFY_OFFLINE.sh --online
cosign verify --key https://readystack.dev/keys/cosign.pub readystack/airflow:3.4.0-CE-ubuntu24.04-r4
```

ReadyStack records every signature in the public Sigstore transparency log (Rekor), so the command needs no extra flags.

