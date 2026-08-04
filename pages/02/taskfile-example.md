---
layout: code
---

# Taskfile — пример

<Transform :scale="0.85">

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

</Transform>
