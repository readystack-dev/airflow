# Apache Airflow: ReadyStack build kit

The offline build kit for the Docker image `readystack/airflow:3.4.0-CE-ubuntu24.04-r3`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [3.4.0-CE-ubuntu24.04-r3](https://github.com/readystack-dev/airflow/releases/tag/3.4.0-CE-ubuntu24.04-r3)
- Image: [`docker.io/readystack/airflow:3.4.0-CE-ubuntu24.04-r3`](https://hub.docker.com/r/readystack/airflow/tags), manifest digest `sha256:02ad7df0171546c6f114434643ae9369bb74ca66943eadeb81c07a2eee313eba`
- Product page: https://readystack.dev/products/airflow
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=airflow

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
