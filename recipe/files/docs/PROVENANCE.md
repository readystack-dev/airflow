# Provenance — Apache Airflow (`3.4.0-CE-ubuntu24.04-r4`)

Every item below is copied from a record in this archive; the record is named in brackets.

## Source

- Upstream repository: https://github.com/apache/airflow
- Upstream release: `3.4.0` (commit `619a787ac81e8b7c8a9102fc78e3f0b00625d04e`), the source revision of the release artifact this image is built from
- Release artifact: https://pypi.org/project/apache-airflow/3.4.0/; the build installs it
- Source tree: `af6efccef2378cd88978f5570674d7358af2b92421fea4d9ee348e1658d128a2` over 14958 files [`docs/provenance/SOURCE_TREE_HASH.json`] — upstream's tree at that revision, cloned for the licence and Community Edition audit; the image is not built from it
- Community Edition only: enterprise code is removed or refused before the build (CE audit at acquire).

## Build

- Method: `source-rebuild-python`; base image `scratch + a chiselled Ubuntu 24.04 runtime (vendored/chisel/slices.txt, packages.tsv)`
- Compiled with `--network=none` from the vendored upstream release artifact, dependencies and base images; BuildKit pinned to `moby/buildkit:v0.31.2` (shipped in `toolchain/`).
- SOURCE_DATE_EPOCH: `1795644287` [`SOURCE_DATE_EPOCH.txt`]
- Frozen inputs: [`docs/provenance/FREEZE_MANIFEST.json`], build recipe: [`docs/provenance/BUILD_RECIPE.json`].

## Output

- Published image: `readystack/airflow:3.4.0-CE-ubuntu24.04-r4` — index digest `sha256:d106ae5c556c4f4fe54b3a3714f395143699b433000ba1c4609e6b4c4088f59f` [`docs/provenance/LOCKED_DIGEST.txt`]
- linux/amd64 image digest `sha256:322cab41ac2503ec78026c829f78662cd5c690367bda4a5c7d1b3ddc71d7371c` — reproduced from this archive before publishing [`docs/provenance/REPRO_VERIFICATION.json`].

## Attestations and signature

- SBOM + SLSA provenance attached to the image: attached: an SPDX SBOM and SLSA v1 provenance [`docs/provenance/SCOUT_VERDICT.json`]
- cosign signature by the ReadyStack release key: verified before publishing. Check it: `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/airflow:3.4.0-CE-ubuntu24.04-r4`

## Pinned inputs, and why

- 27 pinned inputs, each with where it came from, why this one, how it was checked and when it was decided: `docs/PINS.md` [`docs/provenance/PINS.json`] (upstream source 3, base and build images 2, image digests 2, language runtimes and toolchains 1, dependency versions readystack sets 2, files the vendor stage downloads 1, pins in the kit's dockerfiles 7, readystack's build engine 7, dependencies upstream chose 2).

## Build trail

- How this release passed through the ReadyStack harness, record by record (every chain, drive, stage, review round, defect and check, each naming the record it is copied from): `ReadyStack-airflow-3.4.0-CE-ubuntu24.04-r4.trail.json`, published beside this archive and signed with the release key (`ReadyStack-airflow-3.4.0-CE-ubuntu24.04-r4.trail.json.sigstore`), and readable at https://readystack.dev/products/airflow/trail/3.4.0-CE-ubuntu24.04-r4.

