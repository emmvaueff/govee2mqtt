#ARG BUILD_FROM
#FROM ghcr.io/emmvaueff/govee2mqtt:latest AS govee2mqtt
#FROM $BUILD_FROM
#COPY run.sh /run.sh
#COPY --from=govee2mqtt /app/govee /app/
#COPY --from=govee2mqtt /app/assets /app/assets/
#COPY --from=govee2mqtt /app/AmazonRootCA1.pem /app/
#CMD [ "/run.sh" ]

ARG GOVEE_REPO=https://github.com/emmvaueff/govee2mqtt.git
ARG GOVEE_REF=main

FROM rust:bookworm AS build

ARG GOVEE_REPO
ARG GOVEE_REF

RUN apt-get update && \
	apt-get install -y --no-install-recommends \
		build-essential \
		pkg-config \
		libssl-dev \
		ca-certificates \
		git && \
	rm -rf /var/lib/apt/lists/*

WORKDIR /src
RUN git clone --depth 1 --branch ${GOVEE_REF} ${GOVEE_REPO} /src
RUN cargo build --release

ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base-debian:bookworm
FROM ${BUILD_FROM}

COPY run.sh /run.sh
COPY --from=build /src/target/release/govee /app/govee
COPY --from=build /src/assets /app/assets/
COPY --from=build /src/AmazonRootCA1.pem /app/

CMD [ "/run.sh" ]
