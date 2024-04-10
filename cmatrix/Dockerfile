# Build container image
FROM alpine as cmatrixbuilder

WORKDIR cmatrix

RUN apk update --no-cache && \
    apk add git autoconf automake alpine-sdk ncurses-dev ncurses-static && \
    git clone https://github.com/ricbha/cmatrix.git . && \
    autoreconf -i && \
    mkdir -p /usr/lib/kbd/consolefonts /usr/share/consolefonts && \
    ./configure LDFLAGS="-static" && \
    make 

# cmatrix container image
FROM alpine

LABEL org.opencontainers.image.authors="Richa Bharti" \
      org.opencontainers.image.description="Container image for cmatrix for coursera project"

RUN apk update --no-cache && \
    apk add ncurses-terminfo-base && \
    adduser -g "Thomas A" -s /usr/sbin/nologin -D -H thomas

COPY --from=cmatrixbuilder /cmatrix/cmatrix /cmatrix

USER thomas

ENTRYPOINT ["./cmatrix"]
CMD ["-b"]
