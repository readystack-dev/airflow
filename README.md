# Apache Airflow: ReadyStack build kit

The offline build kit for the Docker image `readystack/airflow:3.4.0-CE-ubuntu24.04-r3`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [3.4.0-CE-ubuntu24.04-r3](https://github.com/readystack-dev/airflow/releases/tag/3.4.0-CE-ubuntu24.04-r3)
- Image: [`docker.io/readystack/airflow:3.4.0-CE-ubuntu24.04-r3`](https://hub.docker.com/r/readystack/airflow/tags), manifest digest `sha256:02ad7df0171546c6f114434643ae9369bb74ca66943eadeb81c07a2eee313eba`
- Product page: https://readystack.dev/products/airflow
- Source and build recipe: https://readystack.dev/products/airflow/recipe
- Build trail: https://readystack.dev/products/airflow/trail/3.4.0-CE-ubuntu24.04-r3
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=airflow

## The image

Single container running `airflow standalone`, which supervises the API server, scheduler, DAG processor and triggerer in one foreground process against a self-contained SQLite metadata database. Workflows are Python files placed in /opt/airflow/dags.

## Who uses Apache Airflow and why

Data engineers and platform teams who run scheduled data pipelines, batch jobs and model training.

Airflow lets them write workflows as Python code, then schedules each task in dependency order and shows runs, progress and failures in a web interface.

A typical use: Every night a team pulls sales data from several systems, cleans it and loads it into a reporting database. In Airflow each step is a task in one workflow file kept in version control; the scheduler runs the steps in order, and when one fails the team sees which step broke and why.

From the upstream project's own description: https://github.com/apache/airflow, https://airflow.apache.org/docs/apache-airflow/stable/index.html.

## This release

- Docker Scout policy grade at release: **A** (94%), graded 2026-11-20 with `docker scout policy` (CLI 1.26.0, docker-default configuration).
  - Docker Scout counts 33 packages with copyleft licences (GPL, LGPL, MPL), 30 of them operating-system packages (it counts only packages whose licence it can read; the SBOM lists every component).
- Signed with ReadyStack's release key (fingerprint `04daa92344a52dbc`): https://readystack.dev/keys/cosign.pub.
- Runs as a non-root user (`airflow`).
- Attestations on the image: an SPDX SBOM and SLSA provenance v1.
- Rebuilt from this kit with network egress blocked on 2026-11-20; the rebuilt image digest is the published one.
- Base: Ubuntu 24.04.

The current grade, measured again every day, is on https://readystack.dev/products/airflow.

Apache Airflow is distributed under its upstream licence (Apache-2.0); the kit contains upstream's source under that licence, and each vendored component under its own.

## Download

From the [3.4.0-CE-ubuntu24.04-r3 release](https://github.com/readystack-dev/airflow/releases/tag/3.4.0-CE-ubuntu24.04-r3): `ReadyStack-airflow-3.4.0-CE-ubuntu24.04-r3.tar.gz` (803.4 MB), SHA-256 `62577cd17965bbec34eac48b787707a5849bdc5cc36d7ff96f85fd0cb221df36`.

```sh
sha256sum --check --ignore-missing SHA256SUMS
```

## Verify and rebuild

The rebuild statement is signed with the ReadyStack release key. Check the signature before you rely on it:

```sh
cosign verify-blob --key https://readystack.dev/keys/cosign.pub \
  --bundle ReadyStack-airflow-3.4.0-CE-ubuntu24.04-r3.rebuild.json.sigstore ReadyStack-airflow-3.4.0-CE-ubuntu24.04-r3.rebuild.json
```

Unpack with `-p` so file modes are kept, then follow the kit's own README.md and docs/REBUILD.md:

```sh
tar -xpzf ReadyStack-airflow-3.4.0-CE-ubuntu24.04-r3.tar.gz
cd 3.4.0-CE-ubuntu24.04-r3
less README.md docs/REBUILD.md
```

The build trail (`ReadyStack-airflow-3.4.0-CE-ubuntu24.04-r3.trail.json`, signed the same way, and its Markdown copy) records how this release was built and checked.

## This repository

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/airflow/recipe. The complete kit is the release download. Each release is a tag here.

— ReadyStack · https://readystack.dev
