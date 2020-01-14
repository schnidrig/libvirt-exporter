# Stage 1: Build libvirt exporter
FROM golang:alpine

ENV libxml2_version 2.9.8
ENV libvirt_version 3.8.0

# Install dependencies
RUN set -ex
RUN apk add --update git gcc g++ make libc-dev portablexdr-dev linux-headers libnl-dev perl libtirpc-dev pkgconfig wget python python-dev libxslt upx
RUN wget ftp://xmlsoft.org/libxml2/libxml2-${libxml2_version}.tar.gz -P /tmp && \
    tar -xf /tmp/libxml2-${libxml2_version}.tar.gz -C /tmp
WORKDIR /tmp/libxml2-${libxml2_version}
RUN ./configure --disable-shared --enable-static && \
    make -j$(nproc) && \
    make install
RUN wget https://libvirt.org/sources/libvirt-${libvirt_version}.tar.xz -P /tmp && \
    tar -xf /tmp/libvirt-${libvirt_version}.tar.xz -C /tmp
WORKDIR /tmp/libvirt-${libvirt_version}
RUN ./configure --disable-shared --enable-static --localstatedir=/var --without-storage-mpath && \
    make -j$(nproc) && \
    make install && \
    sed -i 's/^Libs:.*/& -lnl -ltirpc -lxml2/' /usr/local/lib/pkgconfig/libvirt.pc

# The stuff below is needed for Visual Studio Code

RUN \
    go get -x -d github.com/stamblerre/gocode 2>&1 \
    && go build -o gocode-gomod github.com/stamblerre/gocode \
    && mv gocode-gomod $GOPATH/bin/ \
    #
    # Install Go tools
    && go get -u -v \
        github.com/mdempsky/gocode \
        github.com/uudashr/gopkgs/cmd/gopkgs \
        github.com/ramya-rao-a/go-outline \
        github.com/acroca/go-symbols \
        github.com/godoctor/godoctor \
        golang.org/x/tools/cmd/guru \
        golang.org/x/tools/cmd/gorename \
        github.com/rogpeppe/godef \
        github.com/zmb3/gogetdoc \
        github.com/haya14busa/goplay/cmd/goplay \
        github.com/sqs/goreturns \
        github.com/josharian/impl \
        github.com/davidrjenni/reftools/cmd/fillstruct \
        github.com/fatih/gomodifytags \
        github.com/cweill/gotests/... \
        golang.org/x/tools/cmd/goimports \
        golang.org/x/lint/golint \
        golang.org/x/tools/gopls \
        github.com/alecthomas/gometalinter \
        honnef.co/go/tools/... \
        github.com/golangci/golangci-lint/cmd/golangci-lint \
        github.com/mgechev/revive \
        github.com/derekparker/delve/cmd/dlv 2>&1

RUN apk add bash
