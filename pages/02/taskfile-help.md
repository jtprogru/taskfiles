
# Taskfile — help

```yaml
tasks:
  default:
    silent: true
    cmds:
      - task --list --color
```

Да! Это всё. И даже если это не добавить, task подскажет сам. Для ленивых:

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

<!--
Максимально простой пример того, как можно упростить себе жизнь даже не используя default-задачу.
-->
