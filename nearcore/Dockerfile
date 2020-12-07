FROM ubuntu:18.04 as builder


RUN apt-get update -qq && apt-get install -y git cmake g++ pkg-config libssl-dev curl llvm clang \
    && rm -rf /var/lib/apt/lists/*
    && git clone https://github.com/near/nearcore.git
    && NEAR_RELEASE_VERSION=$(curl -s https://github.com/near/nearcore/releases/latest | tr '/" ' '\n' | grep "[0-9]\.[0-9]*\.[0-9]" | head -n 1)
    && cd nearcore
    && git checkout $NEAR_RELEASE_VERSION
    && cargo build -p neard --release

FROM lts-slim

RUN mkdir -p /home/near/nearcore

COPY --from=builder /nearcore/target/release/near /home/near/nearcore/
COPY --from=builder /nearcore/target/release/neard /home/near/nearcore/


