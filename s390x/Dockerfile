ARG DOCKER_VERSION=18.06.3-ce
ARG PYTHON_VERSION=3.9
ARG RUNTIME_ALPINE_VERSION=3.12

FROM quay.io/ibmz/docker:${DOCKER_VERSION} AS docker-cli

FROM quay.io/ibmz/python:${PYTHON_VERSION} AS build
RUN apk add --no-cache \
    bash \
    build-base \
    ca-certificates \
    curl \
    gcc \
    git \
    libc-dev \
    libffi-dev \
    libgcc \
    make \
    musl-dev \
    openssl \
    openssl-dev \
    zlib-dev
ENV BUILD_BOOTLOADER=1

COPY docker-compose-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["sh", "/usr/local/bin/docker-compose-entrypoint.sh"]
COPY --from=docker-cli /usr/local/bin/docker /usr/local/bin/docker
WORKDIR /code/
RUN pip install \
    virtualenv==20.0.30 \
    tox==3.19.0

COPY requirements-dev.txt .
COPY requirements-indirect.txt .
COPY requirements.txt .
RUN pip install -r requirements.txt -r requirements-indirect.txt -r requirements-dev.txt
COPY .pre-commit-config.yaml .
COPY tox.ini .
COPY setup.py .
COPY README.md .
COPY compose compose/
RUN tox --notest
COPY . .
ARG GIT_COMMIT=unknown
ENV DOCKER_COMPOSE_GITSHA=$GIT_COMMIT
RUN script/build/linux-entrypoint

FROM quay.io/ibmz/alpine:${RUNTIME_ALPINE_VERSION} AS runtime
COPY docker-compose-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["sh", "/usr/local/bin/docker-compose-entrypoint.sh"]
COPY --from=docker-cli  /usr/local/bin/docker           /usr/local/bin/docker
COPY --from=build       /usr/local/bin/docker-compose   /usr/local/bin/docker-compose
