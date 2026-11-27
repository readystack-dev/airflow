[![ReadyStack](https://readystack.dev/logo.svg)](https://readystack.dev)

**Stable, Open-source Releases, Automated** · [readystack.dev](https://readystack.dev)

[![Docker Hub](https://img.shields.io/badge/Docker%20Hub-readystack%2Fairflow-2496ED?logo=docker&logoColor=white)](https://hub.docker.com/r/readystack/airflow) [![Docker pulls](https://img.shields.io/docker/pulls/readystack/airflow)](https://hub.docker.com/r/readystack/airflow) [![Grade at release](https://img.shields.io/badge/grade%20at%20release-A%20%2894%25%29-2ea44f)](https://readystack.dev/products/airflow) [![Signed: cosign](https://img.shields.io/badge/signed-cosign-2ea44f)](https://readystack.dev/keys/cosign.pub) ![SBOM: SPDX](https://img.shields.io/badge/SBOM-SPDX-2ea44f) ![Provenance: SLSA v1](https://img.shields.io/badge/provenance-SLSA%20v1-2ea44f) ![Offline rebuild: verified](https://img.shields.io/badge/offline%20rebuild-verified-2ea44f)

# Apache Airflow: ReadyStack build kit

The offline build kit for the Docker image `readystack/airflow:3.4.0-CE-ubuntu24.04-r4`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [3.4.0-CE-ubuntu24.04-r4](https://github.com/readystack-dev/airflow/releases/tag/3.4.0-CE-ubuntu24.04-r4)
- Image: [`docker.io/readystack/airflow:3.4.0-CE-ubuntu24.04-r4`](https://hub.docker.com/r/readystack/airflow/tags), manifest digest `sha256:322cab41ac2503ec78026c829f78662cd5c690367bda4a5c7d1b3ddc71d7371c`
- Product page: https://readystack.dev/products/airflow
- Source and build recipe: https://readystack.dev/products/airflow/recipe
- Build trail: https://readystack.dev/products/airflow/trail/3.4.0-CE-ubuntu24.04-r4
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=airflow

## The image

Single container running `airflow standalone`, which supervises the API server, scheduler, DAG processor and triggerer in one foreground process against a self-contained SQLite metadata database. Workflows are Python files placed in /opt/airflow/dags.

## Who uses Apache Airflow and why

Data engineers and platform teams who run scheduled data pipelines, batch jobs and model training.

Airflow lets them write workflows as Python code, then schedules each task in dependency order and shows runs, progress and failures in a web interface.

A typical use: Every night a team pulls sales data from several systems, cleans it and loads it into a reporting database. In Airflow each step is a task in one workflow file kept in version control; the scheduler runs the steps in order, and when one fails the team sees which step broke and why.

From the upstream project's own description: https://github.com/apache/airflow, https://airflow.apache.org/docs/apache-airflow/stable/index.html.

## This release

- Docker Scout policy grade at release: **A** (94%), graded 2026-11-27 with `docker scout policy` (CLI 1.26.0, docker-default configuration).
  - Docker Scout counts 33 packages with copyleft licences (GPL, LGPL, MPL), 30 of them operating-system packages (it counts only packages whose licence it can read; the SBOM lists every component).
- Signed with ReadyStack's release key (fingerprint `04daa92344a52dbc`): https://readystack.dev/keys/cosign.pub.
- Runs as a non-root user (`airflow`).
- Attestations on the image: an SPDX SBOM and SLSA provenance v1.
- Rebuilt from this kit with network egress blocked on 2026-11-25; the rebuilt image digest is the published one.
- Base: Ubuntu 24.04.

The current grade, measured again every day, is on https://readystack.dev/products/airflow.

Apache Airflow is distributed under its upstream licence (Apache-2.0); the kit contains upstream's source under that licence, and each vendored component under its own.

## Download

From the [3.4.0-CE-ubuntu24.04-r4 release](https://github.com/readystack-dev/airflow/releases/tag/3.4.0-CE-ubuntu24.04-r4): `ReadyStack-airflow-3.4.0-CE-ubuntu24.04-r4.tar.gz` (803.4 MB), SHA-256 `436657cf62a39e88d0c57b6998202778979081a3aed9eee1c4c3578626f260ed`.

```sh
sha256sum --check --ignore-missing SHA256SUMS
```

## Verify and rebuild

The rebuild statement is signed with the ReadyStack release key. Check the signature before you rely on it:

```sh
cosign verify-blob --key https://readystack.dev/keys/cosign.pub \
  --bundle ReadyStack-airflow-3.4.0-CE-ubuntu24.04-r4.rebuild.json.sigstore ReadyStack-airflow-3.4.0-CE-ubuntu24.04-r4.rebuild.json
```

Unpack with `-p` so file modes are kept, then follow the kit's own README.md and docs/REBUILD.md:

```sh
tar -xpzf ReadyStack-airflow-3.4.0-CE-ubuntu24.04-r4.tar.gz
cd 3.4.0-CE-ubuntu24.04-r4
less README.md docs/REBUILD.md
```

The build trail (`ReadyStack-airflow-3.4.0-CE-ubuntu24.04-r4.trail.json`, signed the same way, and its Markdown copy) records how this release was built and checked.

## This repository

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/airflow/recipe. The complete kit is the release download. Each release is a tag here. How to report a vulnerability: [SECURITY.md](SECURITY.md).

## About ReadyStack

**Stable, Open-source Releases, Automated.** ReadyStack releases popular open-source apps every week with every available critical and high security fix applied, plus full documentation, reference data, tests and a build recipe that recreates each release bit-for-bit. Each release is rebuilt from its pinned inputs with the network off, checked against its published digest, signed, and scanned and graded again every day. The images and build kits are free, and ReadyStack is soon to be a nonprofit.

[readystack.dev](https://readystack.dev) · [All products](https://readystack.dev/products.html) · [Build kits on GitHub](https://github.com/readystack-dev) · [Defect log](https://readystack.dev/defects.html) · contact@readystack.dev

— ReadyStack · https://readystack.dev
