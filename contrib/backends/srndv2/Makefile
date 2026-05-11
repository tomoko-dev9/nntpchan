GOROOT ?= $(shell go env GOROOT)
GO ?= $(GOROOT)/bin/go
REPO=$(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))
VERSION=$(shell $(GO) version | cut -d' ' -f3)
GOARCH ?= $(shell go env GOARCH)
GOOS ?= $(shell go env GOOS)
GOARM ?= $(shell go env GOARM)
all: build

build: srndv2

srndv2:
	GOARM=$(GOARM) GOOS=$(GOOS) GOARCH=$(GOARCH) GO111MODULE=off GOPATH=/tmp/srnd-gopath GOROOT=$(GOROOT) $(GO) build -ldflags "-X srnd.GitVersion=-$(shell git rev-parse --short HEAD)" -v

srndv2-lua:
	GO111MODULE=off GOPATH=/tmp/srnd-gopath GOROOT=$(GOROOT) $(GO) build -ldflags "-X srnd.GitVersion=-$(shell git rev-parse --short HEAD)" -tags lua -v

clean:
	GO111MODULE=off GOPATH=/tmp/srnd-gopath GOROOT=$(GOROOT) $(GO) clean -v

test:
	GO111MODULE=off GOPATH=/tmp/srnd-gopath GOROOT=$(GOROOT) $(GO) test srnd
