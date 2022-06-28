FROM docker.io/alpine:3.15

# Include dist
ADD ./tanner/docker/tanner/dist/ /root/dist/

# Setup apt
RUN apk -U --no-cache add \
    build-base \
    git \
    libcap \
    libffi-dev \
    libressl-dev \
    linux-headers \
    py3-yarl \
    python3 \
    python3-dev \
    py3-pip

COPY ./tanner /opt/tanner
WORKDIR /opt/tanner/
RUN cp /root/dist/config.yaml /opt/tanner/tanner/data/
RUN python3 -m pip install -r requirements.txt \
    && python3 setup.py install \
    && chown -R nobody:nobody /opt/tanner

# Clean up
RUN apk del --purge \
    build-base \
    linux-headers \
    python3-dev \
    && rm -rf /root/* \
    && rm -rf /tmp/* /var/tmp/* \
    && rm -rf /var/cache/apk/*

USER nobody:nobody
WORKDIR /opt/tanner
