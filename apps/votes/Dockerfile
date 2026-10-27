FROM dhi.io/golang:1.26-dev@sha256:c1e60bfcf708775a036d8777a4a29b6c26f618bbf639a9a6161559c5adda20af AS build

USER 1000:1000
WORKDIR /tmp/src
ENV GOCACHE=/tmp/go-build \
    GOMODCACHE=/tmp/go-mod
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -trimpath -ldflags="-s -w" -o /tmp/votes .

FROM scratch

COPY --from=build /tmp/votes /votes

USER 65532:65532
EXPOSE 8081
ENTRYPOINT ["/votes"]
