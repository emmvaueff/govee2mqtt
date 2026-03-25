#ARG BUILD_FROM
#FROM ghcr.io/emmvaueff/govee2mqtt:latest AS govee2mqtt
#FROM $BUILD_FROM
#COPY run.sh /run.sh
#COPY --from=govee2mqtt /app/govee /app/
#COPY --from=govee2mqtt /app/assets /app/assets/
#COPY --from=govee2mqtt /app/AmazonRootCA1.pem /app/
#CMD [ "/run.sh" ]

ARG BUILD_FROM
FROM $BUILD_FROM AS build

# Install build dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        curl \
        build-essential \
        pkg-config \
        libssl-dev \
        ca-certificates \
        git \
        rustc \
        cargo && \
    rm -rf /var/lib/apt/lists/*

# Copy source
COPY . /src
WORKDIR /src

# Build govee2mqtt
RUN cargo build --release

# Runtime image
FROM $BUILD_FROM

COPY run.sh /run.sh
COPY --from=build /src/target/release/govee2mqtt /app/govee2mqtt

CMD [ "/run.sh" ]
