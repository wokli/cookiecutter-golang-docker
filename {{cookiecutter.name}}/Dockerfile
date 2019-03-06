FROM golang:alpine AS builder

# Add all the source code (except what's ignored
# under `.dockerignore`) to the build context.
ADD ./ /go/src/github.com/some/app

RUN set -ex && \
  cd /go/src/github.com/some/app && \       
  CGO_ENABLED=0 go build \
        -tags netgo \
        -v -a \
        -ldflags '-extldflags "-static"' && \
  mv ./app /usr/bin/app

FROM alpine

# Retrieve the binary from the previous stage
COPY --from=builder /usr/bin/app /usr/local/bin/app

# Set the binary as the entrypoint of the container
ENTRYPOINT [ "app" ]
