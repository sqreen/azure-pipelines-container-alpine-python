FROM python:2.7-alpine3.11 AS python-2.7-alpine3.11
FROM python:3.5-alpine3.12 AS python-3.5-alpine3.12
FROM python:3.6-alpine3.12 AS python-3.6-alpine3.12
FROM python:3.7-alpine3.12 AS python-3.7-alpine3.12
FROM python:3.8-alpine3.12 AS python-3.8-alpine3.12
FROM python:3.9-rc-alpine3.12 AS python-3.9-alpine3.12

# Following doc https://github.com/microsoft/azure-pipelines-agent/blob/master/docs/design/non-glibc-containers.md
FROM node:12-alpine3.12

RUN apk add --no-cache --virtual .pipeline-deps readline linux-pam \
  && apk add bash sudo shadow \
  && apk del .pipeline-deps

LABEL "com.azure.dev.pipelines.agent.handler.node.path"="/usr/local/bin/node"

RUN apk -U add binutils patchelf curl python3 alpine-sdk clang ninja python2 tar xz

# Build GN
RUN git clone https://gn.googlesource.com/gn /tmp/gn \
    && git -C /tmp/gn checkout ad9e442d92dcd9ee73a557428cfc336b55cbd533 \
    && python /tmp/gn/build/gen.py && ninja -C /tmp/gn/out \
    && cp -f /tmp/gn/out/gn /usr/local/bin/gn && rm -rf /tmp/gn

CMD [ "node" ]
