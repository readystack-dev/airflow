# GENERATED L-009 hermetic Python build (--network=none, SBOM+SLSA-attestation-producing).
# Pinned base from BASE_IMAGE_DIGESTS.lock; vendored wheelhouse + source + debs from Dockerfile.vendor.
# pydeps sub-stage installs the wheelhouse offline; COPY /deps /usr/local keeps pip OUT of the runtime SBOM.
# CPython 3.13.18 COMPILED HERE, offline, from python.org's tarball (sha256 re-checked) and the vendored
# build debs, by the same command the vendor stage ran; linked against Ubuntu's libraries (debs the scan
# sees, apt fixes). The runtime takes /usr/local from this stage and none of the toolchain.
FROM ubuntu:24.04@sha256:9c7f200c07e3d4a03e7249d2c16bf66a51120fb876aa2242cf31451412543289 AS cpython
ARG SOURCE_DATE_EPOCH=1795160985
ENV SOURCE_DATE_EPOCH=${SOURCE_DATE_EPOCH}
COPY vendored/python/debs/ /tmp/cpython-debs/
COPY vendored/python/Python-3.13.18.tar.xz /tmp/Python-3.13.18.tar.xz
RUN set --; for f in /tmp/cpython-debs/*.deb; do [ -e "$f" ] && set -- "$@" "$f"; done; \
    for pass in 1 2 3 4 5; do [ $# -eq 0 ] || dpkg -i --skip-same-version "$@" >/dev/null 2>&1 && break; done; \
    dpkg --configure -a && bad= && for d in "$@"; do p=$(dpkg-deb -f "$d" Package) v=$(dpkg-deb -f "$d" Version); \
      case "$(dpkg-query -W -f='${Version} ${db:Status-Abbrev}' "$p" 2>/dev/null)" in "$v ii"*) ;; *) bad="$bad $p=$v" ;; esac; done && \
    { [ -z "$bad" ] || { echo "vendored debs not installed at their vendored version:$bad" >&2; false; }; } && \
    : "${SOURCE_DATE_EPOCH:?the CPython build needs SOURCE_DATE_EPOCH}" && \
    echo "abb0102e6d4ae027509bcb82327d9dd85515b44a3ba8ad538cfc5e9847029f68  /tmp/Python-3.13.18.tar.xz" | sha256sum -c - && \
    mkdir -p /usr/src/python && tar -xJf /tmp/Python-3.13.18.tar.xz -C /usr/src/python --strip-components=1 --no-same-owner && \
    rm -rf /tmp/cpython-debs /tmp/Python-3.13.18.tar.xz && cd /usr/src/python && \
    export DEB_BUILD_MAINT_OPTIONS=optimize=-lto && \
    PYCF="$(dpkg-buildflags --get CFLAGS) $(dpkg-buildflags --get CPPFLAGS)" && PYLD="$(dpkg-buildflags --get LDFLAGS)" && \
    ./configure --build=x86_64-linux-gnu --prefix=/usr/local --enable-shared --enable-loadable-sqlite-extensions --enable-option-checking=fatal --disable-test-modules --without-static-libpython --with-ensurepip=install --with-system-expat --without-system-libmpdec --with-dbmliborder= py_cv_module__dbm=n/a py_cv_module__gdbm=n/a py_cv_module__tkinter=n/a && \
    make -j"$(nproc)" "EXTRA_CFLAGS=$PYCF" "LDFLAGS=$PYLD,--strip-all" && \
    rm python && make -j"$(nproc)" "EXTRA_CFLAGS=$PYCF" "LDFLAGS=$PYLD,--strip-all,-rpath='\$\$ORIGIN/../lib'" python && \
    make install COMPILEALL_OPTS=--invalidation-mode=checked-hash && \
    cd / && rm -rf /usr/src/python /usr/local/lib/python3.13/idlelib /usr/local/lib/python3.13/tkinter /usr/local/lib/python3.13/turtledemo /usr/local/bin/idle3* && \
    for s in pip3 pydoc3 python3 python3-config; do ln -sfT "$s" "/usr/local/bin/${s%3*}${s#*3}"; done && \
    ldconfig && /usr/local/bin/python3 -I -B -c "import ssl, sqlite3, zlib, bz2, lzma, ctypes, uuid, pyexpat, decimal, readline, curses, hashlib, sys, importlib.util as u; bad = [m for m in ('_dbm', '_gdbm', '_tkinter') if u.find_spec(m)]; assert not bad, ('built but must not be', bad); print(sys.version.replace(chr(10), ' '), '|', ssl.OPENSSL_VERSION, '| sqlite', sqlite3.sqlite_version, '| expat', pyexpat.EXPAT_VERSION)" && \
    ldd /usr/local/lib/python3.13/lib-dynload/_ssl.cpython-*.so | grep -qE 'libssl.so.3 => /(usr/)?lib/x86_64-linux-gnu/' && ldd /usr/local/lib/python3.13/lib-dynload/_sqlite3.cpython-*.so | grep -qE 'libsqlite3.so.0 => /(usr/)?lib/x86_64-linux-gnu/' && ldd /usr/local/lib/python3.13/lib-dynload/pyexpat.cpython-*.so | grep -qE 'libexpat.so.1 => /(usr/)?lib/x86_64-linux-gnu/' && ldd /usr/local/lib/python3.13/lib-dynload/_ctypes.cpython-*.so | grep -qE 'libffi.so.8 => /(usr/)?lib/x86_64-linux-gnu/' && ldd /usr/local/lib/python3.13/lib-dynload/_lzma.cpython-*.so | grep -qE 'liblzma.so.5 => /(usr/)?lib/x86_64-linux-gnu/' && ldd /usr/local/lib/python3.13/lib-dynload/_bz2.cpython-*.so | grep -qE 'libbz2.so.1.0 => /(usr/)?lib/x86_64-linux-gnu/' && ldd /usr/local/lib/python3.13/lib-dynload/_uuid.cpython-*.so | grep -qE 'libuuid.so.1 => /(usr/)?lib/x86_64-linux-gnu/' && ldd /usr/local/lib/python3.13/lib-dynload/zlib.cpython-*.so | grep -qE 'libz.so.1 => /(usr/)?lib/x86_64-linux-gnu/'
FROM cpython AS pydeps
ENV PIP_DISABLE_PIP_VERSION_CHECK=1
COPY vendored/wheelhouse /wheelhouse
RUN pip install --no-index --no-cache-dir --no-compile --find-links=/wheelhouse -c /wheelhouse/constraints.txt -r /wheelhouse/requirements.lock --target=/deps
# the Python runtime tree for the chiselled rootfs: the compiled CPython with the wheelhouse installed into it
FROM cpython AS pyrt
COPY --from=pydeps /deps /usr/local
RUN set -eu; rm -rf /usr/local/lib/python3.13/site-packages/pip /usr/local/lib/python3.13/site-packages/pip-*.dist-info /usr/local/bin/pip /usr/local/bin/pip3 /usr/local/bin/pip3.13
RUN set -eu; find /usr/local -type f | LC_ALL=C sort | while read -r f; do \
      [ "$(od -An -c -N4 "$f" | tr -d ' ')" = 177ELF ] || continue; \
      ldd "$f" 2>/dev/null | sed -n 's/^[[:space:]]*\([^[:space:]]*\) => \/.*/\1/p; s/^[[:space:]]*\(\/[^[:space:]]*\) (0x.*/\1/p; s/^[[:space:]]*\([^[:space:]]*\) => not found.*/absent \1/p'; \
    done | LC_ALL=C sort -u > /tmp/ldd.all; \
    grep '^absent ' /tmp/ldd.all | cut -d' ' -f2 | tr '\n' ' ' | sed 's/^./not in the release image either (not required): &/'; echo; \
    grep -v '^absent ' /tmp/ldd.all | while read -r lib; do case "$lib" in \
      /*) echo "$lib" ;; *) [ -n "$(find /usr/local -name "$lib" -print -quit)" ] || echo "$lib" ;; esac; \
    done > /kitforge.needs && rm -f /tmp/ldd.all && \
    echo "CPython and the installed packages load: $(tr '\n' ' ' < /kitforge.needs)"
# rs-probe: the HEALTHCHECK helper (standard library only, from the kit's rs-probe.go)
FROM golang:1.27.3-bookworm@sha256:972b4110ba0f47539a975c31e52c64765e1ff19f2d74469ae4a595c6676e80a5 AS build_rs_probe
WORKDIR /probe
COPY rs-probe.go main.go
COPY rs-probe.mod go.mod
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOFLAGS="-buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1795160985
RUN go build -trimpath -ldflags='-s -w' -o /rs-probe .
# the chiselled rootfs (vendored/chisel, cut by Dockerfile.vendor), the service user, the binaries
FROM ubuntu:24.04@sha256:9c7f200c07e3d4a03e7249d2c16bf66a51120fb876aa2242cf31451412543289 AS rootfs
ARG SOURCE_DATE_EPOCH=1795160985
ADD vendored/chisel/rootfs.tar /rootfs/
RUN printf 'airflow:x:50000:50000::/opt/airflow:/bin/sh\n' >> /rootfs/etc/passwd && printf 'airflow:x:50000:\n' >> /rootfs/etc/group && \
    mkdir -p /rootfs/opt/airflow /rootfs/opt/airflow/dags /rootfs/opt/airflow/logs /rootfs/opt/airflow/plugins && chown -R 50000:50000 /rootfs/opt/airflow /rootfs/opt/airflow/dags /rootfs/opt/airflow/logs /rootfs/opt/airflow/plugins
COPY --from=build_rs_probe /rs-probe /rootfs/usr/local/bin/rs-probe
COPY --from=pyrt /usr/local/ /rootfs/usr/local/
COPY --from=pyrt /kitforge.needs /tmp/needs/airflow
RUN chmod 755 /rootfs/usr/local/bin/rs-probe && \
    chown -R 50000:50000 /rootfs/usr/local
RUN --mount=type=bind,source=vendored/chisel,target=/chisel set -eu; \
    for n in /tmp/needs/*; do while read -r lib; do [ -n "$lib" ] || continue; \
      case "$lib" in /*) [ -e "/rootfs$lib" ] ;; *) [ -n "$(find /rootfs/usr/lib /rootfs/usr/lib64 -name "$lib" 2>/dev/null | head -1)" ] ;; esac || \
      { echo "$(basename "$n") needs $lib, which the chiselled rootfs does not have: add its slice to runtime_base.slices" >&2; exit 1; }; \
    done < "$n"; done; rm -rf /tmp/needs; \
    [ -e /rootfs/etc/os-release ] || { echo "no /etc/os-release: base-files_release-info is missing" >&2; exit 1; }; \
    [ "$(grep -c '^Package: ' /rootfs/var/lib/dpkg/status)" = "$(wc -l < /chisel/packages.tsv)" ] || \
      { echo "/var/lib/dpkg/status does not list every package of vendored/chisel/packages.tsv" >&2; exit 1; }; \
    rm -rf /rootfs/var/log/dpkg.log /rootfs/var/log/alternatives.log /rootfs/var/log/apt /rootfs/var/cache/ldconfig/aux-cache /rootfs/var/cache/fontconfig
FROM scratch
ARG SOURCE_DATE_EPOCH=1795160985
COPY --from=rootfs /rootfs/ /
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin PYTHONUNBUFFERED=1 PYTHONDONTWRITEBYTECODE=1 PYTHONPATH=/usr/local LANG=C.UTF-8 AIRFLOW_HOME=/opt/airflow AIRFLOW__CORE__LOAD_EXAMPLES=False AIRFLOW__DATABASE__SQL_ALCHEMY_CONN=sqlite:////opt/airflow/airflow.db AIRFLOW__API__HOST=0.0.0.0 AIRFLOW__API__PORT=8080
LABEL org.opencontainers.image.title="Apache Airflow"
LABEL org.opencontainers.image.version="3.4.0-CE-ubuntu24.04-r3"
LABEL org.opencontainers.image.licenses="Apache-2.0"
LABEL org.opencontainers.image.source="https://github.com/apache/airflow"
LABEL org.opencontainers.image.description="Platform to programmatically author, schedule and monitor workflows"
LABEL org.opencontainers.image.vendor="ReadyStack.dev"
LABEL readystack.product="airflow"
LABEL readystack.edition="CE"
LABEL readystack.build_method="source-rebuild-python"
LABEL org.opencontainers.image.revision="619a787ac81e8b7c8a9102fc78e3f0b00625d04e"
HEALTHCHECK --interval=5s --timeout=30s --start-period=60s \
  CMD /usr/local/bin/rs-probe http://127.0.0.1:8080/api/v2/monitor/health || exit 1
WORKDIR /opt/airflow
USER airflow
EXPOSE 8080
ENTRYPOINT ["airflow", "standalone"]
