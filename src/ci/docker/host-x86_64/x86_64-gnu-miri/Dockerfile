FROM ghcr.io/rust-lang/ubuntu:22.04

ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y --no-install-recommends \
  g++ \
  make \
  ninja-build \
  file \
  curl \
  ca-certificates \
  python3 \
  git \
  cmake \
  libssl-dev \
  sudo \
  xz-utils \
  tidy \
  \
  libc6 \
  wget \
  # libgccjit dependencies
  flex \
  libmpfr-dev \
  libgmp-dev \
  libmpc3 \
  libmpc-dev \
  && rm -rf /var/lib/apt/lists/*

COPY scripts/sccache.sh /scripts/
RUN sh /scripts/sccache.sh

# Fix rustc_codegen_gcc lto issues.
ENV GCC_EXEC_PREFIX="/usr/lib/gcc/"

COPY host-x86_64/x86_64-gnu-miri/check-miri.sh /tmp/

ENV RUST_CONFIGURE_ARGS \
  --build=x86_64-unknown-linux-gnu \
  --enable-new-symbol-mangling

ENV HOST_TARGET x86_64-unknown-linux-gnu

# FIXME(#133381): currently rustc alt builds do *not* have rustc debug
# assertions enabled! Therefore, we cannot force download CI rustc.
#ENV FORCE_CI_RUSTC 1

COPY scripts/shared.sh /scripts/

ENV SCRIPT /tmp/check-miri.sh ../x.py
