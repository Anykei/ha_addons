FROM rust:slim-bookworm AS builder

RUN apt-get update && apt-get install -y \
    build-essential \
    pkg-config \
    libssl-dev \
    libsqlite3-dev \
    libfreetype6-dev \
    libfontconfig1-dev \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY . .

RUN cargo build --release

# --- Этап выполнения ---
FROM debian:bookworm-slim
RUN apt-get update && apt-get install -y \
    ca-certificates libssl3 libsqlite3-0 libfreetype6 libfontconfig1 \
    fontconfig fonts-dejavu \
    && rm -rf /var/lib/apt/lists/*

COPY --from=builder app/target/release/rust_test .

CMD ["./test_addon"]