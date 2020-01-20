FROM python:2.7-alpine3.11 AS python-2.7-alpine3.11
FROM python:3.5-alpine3.11 AS python-3.5-alpine3.11
FROM python:3.6-alpine3.11 AS python-3.6-alpine3.11
FROM python:3.7-alpine3.11 AS python-3.7-alpine3.11
FROM python:3.8-alpine3.11 AS python-3.8-alpine3.11

# Following doc https://github.com/microsoft/azure-pipelines-agent/blob/master/docs/design/non-glibc-containers.md
FROM node:10-alpine3.11

RUN apk add --no-cache --virtual .pipeline-deps readline linux-pam \
  && apk add bash sudo shadow \
  && apk del .pipeline-deps

LABEL "com.azure.dev.pipelines.agent.handler.node.path"="/usr/local/bin/node"

# Python, git, musl-dev (libc headers), gcc and libc6-compat to have the libc6.so
# symlink to muslc
RUN apk -U --repository=http://dl-cdn.alpinelinux.org/alpine/edge/community \
  add binutils patchelf git musl-dev python3

# Python 2.7 mu
RUN mkdir -p /opt/python/cp27-cp27mu
COPY --from=python-2.7-alpine3.11 /usr/local/ /opt/python/cp27-cp27mu/
RUN for bin in /opt/python/cp27-cp27mu/bin/*; do \
  readelf -h $bin >/dev/null 2&>1 && patchelf --set-rpath /opt/python/cp27-cp27mu/lib $bin || sed -i'' -e '1s"^#!.\+$"#! /opt/python/cp27-cp27mu/bin/python"' $bin; \
  done
RUN /opt/python/cp27-cp27mu/bin/pip debug

# Python 3.5
RUN mkdir -p /opt/python/cp35-cp35m
COPY --from=python-3.5-alpine3.11 /usr/local/ /opt/python/cp35-cp35m/
RUN for bin in /opt/python/cp35-cp35m/bin/*; do \
  readelf -h $bin >/dev/null 2&>1 && patchelf --set-rpath /opt/python/cp35-cp35m/lib $bin || sed -i'' -e '1s"^#!.\+$"#! /opt/python/cp35-cp35m/bin/python"' $bin; \
  done
RUN /opt/python/cp35-cp35m/bin/pip debug

# Python 3.6
RUN mkdir -p /opt/python/cp36-cp36m
COPY --from=python-3.6-alpine3.11 /usr/local/ /opt/python/cp36-cp36m/
RUN for bin in /opt/python/cp36-cp36m/bin/*; do \
  readelf -h $bin >/dev/null 2&>1 && patchelf --set-rpath /opt/python/cp36-cp36m/lib $bin || sed -i'' -e '1s"^#!.\+$"#! /opt/python/cp36-cp36m/bin/python"' $bin; \
  done
RUN /opt/python/cp36-cp36m/bin/pip debug

# Python 3.7
RUN mkdir -p /opt/python/cp37-cp37m
COPY --from=python-3.7-alpine3.11 /usr/local/ /opt/python/cp37-cp37m/
RUN for bin in /opt/python/cp37-cp37m/bin/*; do \
  readelf -h $bin >/dev/null 2&>1 && patchelf --set-rpath /opt/python/cp37-cp37m/lib $bin || sed -i'' -e '1s"^#!.\+$"#! /opt/python/cp37-cp37m/bin/python"' $bin; \
  done
RUN /opt/python/cp37-cp37m/bin/pip debug

# Python 3.8
RUN mkdir -p /opt/python/cp38-cp38
COPY --from=python-3.8-alpine3.11 /usr/local/ /opt/python/cp38-cp38/
RUN for bin in /opt/python/cp38-cp38/bin/*; do \
  readelf -h $bin >/dev/null 2&>1 && patchelf --set-rpath /opt/python/cp38-cp38/lib $bin || sed -i'' -e '1s"^#!.\+$"#! /opt/python/cp38-cp38/bin/python"' $bin; \
  done
RUN /opt/python/cp38-cp38/bin/pip debug

CMD [ "node" ]
