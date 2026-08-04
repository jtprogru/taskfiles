# taskfiles

Презентация "Taskfile – Альтернатива Makefile, которая читается легче".

Собрана на [Slidev](https://sli.dev) с темой [slidev-theme-bear](https://github.com/jtprogru/slidev-theme-bear).

## Структура

`slides.md` — точка входа: headmatter, титульный слайд, разделители `section`, финальный `questions` и порядок подключения остальных слайдов через `src:`.

Контентные слайды лежат по одному в файле в `pages/NN/`, где `NN` — блок доклада:

- `pages/01/` — как мы жили с Makefile;
- `pages/02/` — Taskfile;
- `pages/03/` — сравнение и демо;
- `pages/04/` — итоги.

Frontmatter слайда (лейаут, картинка) живёт в самом файле страницы, в `slides.md` остаётся только `src:` — при конфликте выигрывает `slides.md`, так что дублировать ключи не нужно.

## Useful

Собрать готовый PDF:

```shell
task export
```

Запустить development-сервер:

```shell
task dev
```

## License

Распространение только с письменного согласия автора и владельца – Savin Michael.

