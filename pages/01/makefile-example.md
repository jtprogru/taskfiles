
# Makefile — пример

<Transform :scale="0.72">

```make
SHELL := /bin/bash
.SILENT:
.DEFAULT_GOAL := help

# Global vars
export SYS_GO=$(shell which go)
export SYS_GOFMT=$(shell which gofmt)
export SYS_DOCKER=$(shell which docker)

export BINARY_DIR=dist
export BINARY_NAME=app

include .env
export $(shell sed 's/=.*//' .env)

.PHONY: run.cmd
## Run as go run cmd/app/main.go
run.cmd: cmd/app/main.go
	$(SYS_GO) run cmd/app/main.go

.PHONY: run.bin
## Run as binary
run.bin: build.bin
	source .env && ./$(BINARY_DIR)/$(BINARY_NAME)

.PHONY: run.dc
## Run in Docker
run.dc:
	$(SYS_DOCKER) compose up -d --build
```

</Transform>
