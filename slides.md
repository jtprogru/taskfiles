---
theme: seriph
background: /carbon.png
class: text-center
highlighter: shiki
lineNumbers: true
info: |
  ## Альтернатива Makefile, которая читается легче

drawings:
  persist: false
transition: slide-left
title: Taskfile
mdc: true
---

# Taskfile

## Альтернатива Makefile, которая читается легче

### Михаил Савин | [@jtprogru_channel](https://t.me/jtprogru_channel)

### [Мишка на сервере](https://jtprog.ru) | 2023

---

## Makfile

Makefile – общепринятый стандарт.

Утилита `make` есть почти на каждой системе.

Используется чтобы меньше писать букв – например:

```shell
docker compose -f docker-compose.local.yml up -d
# vs
make rundev
```

---

## Makefile – Example

<Transform :scale="0.85">

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

---

## Makefile – Usage

Теперь можно спокойно писать в консоли:

```shell
# Запуск контейнеров с зависимостями
make run.dc
# Запуск через go run
make run.cmd
# Сборка бинарного исполняемого файла и последующий запуск
make run.bin
```

---
layout: two-cols
---

## Makefile – Help

Первый запуск проекта обычно – ознакомление.

*Вопрос*: Какие команды тебе доступны в Makefile?

*Ответ*: "Открой в редакторе и почитай! Тычо?"

::right::

![](/history.png)

---

## Makefile – Help

<Transform :scale="0.63">

```make
.PHONY: help
## Show this help message
help:
 @echo "$$(tput bold)Available rules:$$(tput sgr0)"
 @echo
 @sed -n -e "/^## / { \
  h; \
  s/.*//; \
  :doc" \
  -e "H; \
  n; \
  s/^## //; \
  t doc" \
  -e "s/:.*//; \
  G; \
  s/\\n## /---/; \
  s/\\n/ /g; \
  p; \
 }" ${MAKEFILE_LIST} \
 | LC_ALL='C' sort --ignore-case \
 | awk -F '---' \
  -v ncol=$$(tput cols) \
  -v indent=19 \
  -v col_on="$$(tput setaf 6)" \
  -v col_off="$$(tput sgr0)" \
 '{ \
  printf "%s%*s%s ", col_on, -indent, $$1, col_off; \
  n = split($$2, words, " "); \
  line_length = ncol - indent; \
  for (i = 1; i <= n; i++) { \
   line_length -= length(words[i]) + 1; \
   if (line_length <= 0) { \
    line_length = ncol - indent - length(words[i]) - 1; \
    printf "\n%*s ", -indent, " "; \
   } \
   printf "%s ", words[i]; \
  } \
  printf "\n"; \
 }' \
 | more $(shell test $(shell uname) == Darwin && echo '--no-init --raw-control-chars')
```

</Transform>


---

## Taskfile

Документация доступна тут на официальном сайте утилиты [taskfile.dev](https://taskfile.dev).

Установка максимально простая.

#### Для пользователей Homebrew

```shell
brew install go-task/tap/go-task
```

#### Ну или с помощью скриптов

```bash
sh -c "$(curl --location https://taskfile.dev/install.sh)" -- -d
```

#### Ну в крайнем случае go install

```bash
go install github.com/go-task/task/v3/cmd/task@latest
```

---

## Taskfile

```yaml
# yaml-language-server: $schema=https://taskfile.dev/schema.json
---
version: "3"

set:
  - pipefail

silent: false

tasks:
  prec:
    desc: Preconditions for project
    preconditions:
      - test -f $(which go)
      - test -f go.mod
      - test -f go.sum
      - test -d dist || mkdir dist

  runcmd:
    desc: Run like go run main.go
    deps:
      - prec
    cmds:
      - go run ./cmd/gonewszer/main.go
```

---

## Taskfile – Help

```yaml
tasks:
  default:
    silent: true
    cmds:
      - task --list --color
```

ДА! Это все! И даже если это не добавить, тебе task подскажет сам! Для ленивых:

```bash {lines:false}
~/work/github/jtf $ task
task: Available tasks for this project:
* apply:                         Apply terraform plan with confirmation
* checkov:                       Run checkov for all files
* decrypt:                       Decrypt all secrets
* encrypt:                       Encrypt all secrets
* fmt:                           Fix formatting all terraform files
task: Task "default" does not exist
```

task сам подскажет тебе

<!--
Максимально простой пример того, как можно упростить себе жизнь даже не используя default-задачу.
-->

---

## Taskfile vs Makefile

Отличия в использовании:

- (**!**) Придется писать task, а не make;
- Удобно читать YAML;
- Ревью YAML гораздо приятнее и удобнее, чем Makefile;

<!--
1. Действительно, самое сложное – приучиться писать task, а не make.
2. В большинстве случаев мы работаем с YAML-кодом в виде HCL, K8s манифесты, Helm-чарты;
3. Ревьюить YAML портянку гораздо приятнее.
-->

---

## Taskfile – Demo

На примере рабочего Terraform-проекта можно посмотреть использование Taskfile.

---
layout: two-cols
---

## СПАСИБО

Мелкое и полезное

- [taskfile](https://taskfile.dev) – документация по Taskfile API;
- [justfile](https://just.systems/man/en/) – довольно мощная альтернатива;
- [Gist Taskfile.yml](https://gist.github.com/jtprogru/59e4a634725f68bda83c29ce677b73e0) – мой пример Taskfile из реального проекта;
- [Gist Makefile](https://gist.github.com/jtprogru/541f00f70ca0bd866e4f1164f6f77e5b) - мой пример Makefile из реального проекта;
- [Мишка на сервере](https://t.me/jtprogru_channel) – велкам в мою телегу;

::right::

![](/qr-code.gif)
