FROM ubuntu:16.04

RUN apt-get update && apt-get install -y --no-install-recommends \
  g++ \
  make \
  file \
  curl \
  ca-certificates \
  python \
  git \
  cmake \
  sudo \
  gdb \
  xz-utils \
  lib32stdc++6

RUN curl -o /usr/local/bin/sccache \
      https://s3.amazonaws.com/rust-lang-ci/rust-ci-mirror/2017-04-29-sccache-x86_64-unknown-linux-musl && \
      chmod +x /usr/local/bin/sccache

RUN curl -OL https://github.com/Yelp/dumb-init/releases/download/v1.2.0/dumb-init_1.2.0_amd64.deb && \
    dpkg -i dumb-init_*.deb && \
    rm dumb-init_*.deb
ENTRYPOINT ["/usr/bin/dumb-init", "--"]

WORKDIR /tmp
COPY build-emscripten.sh /tmp/
RUN ./build-emscripten.sh
ENV PATH=$PATH:/tmp/emsdk_portable
ENV PATH=$PATH:/tmp/emsdk_portable/clang/tag-e1.37.10/build_tag-e1.37.10_32/bin
ENV PATH=$PATH:/tmp/emsdk_portable/node/4.1.1_32bit/bin
ENV PATH=$PATH:/tmp/emsdk_portable/emscripten/tag-1.37.10
ENV EMSCRIPTEN=/tmp/emsdk_portable/emscripten/tag-1.37.10

ENV RUST_CONFIGURE_ARGS --target=asmjs-unknown-emscripten

# Run `emcc` first as it's got a prompt and doesn't actually do anything, after
# that's done with do the real build.
ENV SCRIPT emcc && \
      python2.7 ../x.py test --target asmjs-unknown-emscripten

