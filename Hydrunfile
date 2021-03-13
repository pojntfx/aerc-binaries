#!/bin/bash

# Install dependencies
apt update
apt install -y curl make git build-essential scdoc

# Install Go
GO_VERSION=1.16
if [ "$(uname -m)" = 'x86_64' ]; then
    curl -L -o /tmp/go.tar.gz https://golang.org/dl/go${GO_VERSION}.linux-amd64.tar.gz
else
    curl -L -o /tmp/go.tar.gz https://golang.org/dl/go${GO_VERSION}.linux-arm64.tar.gz
fi
tar -C /usr/local -xzf /tmp/go.tar.gz
export PATH=$PATH:/usr/local/go/bin

# Build scdocs
git clone https://git.sr.ht/~sircmpwn/scdoc /tmp/scdoc
cd /tmp/scdoc
make
make install

# Clone upstream
rm -rf /data/upstream
mkdir -p /data/upstream
git clone https://git.sr.ht/~sircmpwn/aerc /data/upstream

# Checkout latest release
cd /data/upstream
git checkout $(git describe --tags $(git rev-list --tags --max-count=1))

# Build upstream
make
DESTDIR=out make install

# Create tar archive in staging directory
mkdir -p /data/staging
tar -zcvf /data/staging/aerc-linux.$(uname -m).tar.gz -C out .
