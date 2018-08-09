FROM golang:1.10-alpine AS lsp-adapter
WORKDIR /go/src/github.com/sourcegraph/lsp-adapter
COPY . .
RUN CGO_ENABLED=0 GOBIN=/usr/local/bin go install github.com/sourcegraph/lsp-adapter

FROM perl:5.26.2
RUN \
  set -xe; \
  cpan Clone URI; \
  curl -sL -o /usr/local/bin/monastery-fatpack https://raw.githubusercontent.com/ccakes/p5-app-monastery/master/author/monastery-fatpack; \
  chmod +x /usr/local/bin/monastery-fatpack

COPY --from=lsp-adapter /usr/local/bin/lsp-adapter /usr/local/bin/
EXPOSE 8080
CMD ["lsp-adapter", "-proxyAddress=0.0.0.0:8080", " monastery-fatpack"]
