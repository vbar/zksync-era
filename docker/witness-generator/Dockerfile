FROM matterlabs/zksync-build-base:latest AS builder

ARG DEBIAN_FRONTEND=noninteractive
ARG RUST_FLAGS=""
ENV RUSTFLAGS=${RUST_FLAGS}

WORKDIR /usr/src/zksync
COPY . .

RUN cd prover && cargo build --release --bin zksync_witness_generator

FROM debian:bookworm-slim

RUN apt-get update && apt-get install -y curl libpq5 ca-certificates && rm -rf /var/lib/apt/lists/*

COPY prover/data/keys/ /prover/data/keys/

COPY --from=builder /usr/src/zksync/prover/target/release/zksync_witness_generator /usr/bin/

ENTRYPOINT ["zksync_witness_generator"]
