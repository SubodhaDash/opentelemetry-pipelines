# Create a stage for building the application.
ARG GO_VERSION=1.22
FROM --platform=$BUILDPLATFORM golang:${GO_VERSION}-alpine AS builder
WORKDIR /usr/src

RUN --mount=type=cache,target=/go/pkg/mod/ \
    --mount=type=cache,target=/root/.cache/go-build \
    mkdir -p /root/.cache/go-build

COPY go.mod go.sum ./

RUN go mod download

COPY . . 

RUN go build -o product-catalog .

################################################################################

FROM alpine:latest AS final

WORKDIR /usr/src

COPY ./products ./products
COPY --from=builder /usr/src/product-catalog ./

# Expose the port that the application listens on.
ENV PRODUCT_CATALOG_PORT=8088

# What the container should run when it is started.
ENTRYPOINT [ "./product-catalog" ]
