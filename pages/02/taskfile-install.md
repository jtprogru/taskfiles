
# Taskfile — установка

Документация доступна на официальном сайте утилиты [taskfile.dev](https://taskfile.dev).

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

<Callout type="tip">

Один статический бинарь на Go: не тянет за собой рантайм и одинаково ставится на macOS, Linux и Windows.

</Callout>
