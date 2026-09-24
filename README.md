<!-- Исходный файл: Holkas/docs/releases-readme.md. Публикуется в BelousovSL/Holkas-Releases workflow «Public texts» при изменении в master; правки прямо в Holkas-Releases будут перезаписаны. -->

# Holkas — перенос доработок КОР Парус 8

[![Последний релиз](https://img.shields.io/github/v/release/BelousovSL/Holkas-Releases?display_name=tag&sort=semver&label=release)](https://github.com/BelousovSL/Holkas-Releases/releases/latest)
[![Docker-образ](https://img.shields.io/docker/v/belousov2501/holkas?sort=semver&label=Docker)](https://hub.docker.com/r/belousov2501/holkas)

**Сайт и документация: [holkas.belousov.net.ru](https://holkas.belousov.net.ru/)**

Holkas переносит доработки КОР Парус 8 (Конструктор отраслевых расширений системы Парус 8) из тестовой базы в рабочую. Он сравнивает выбранные объекты — классы с их методами и вызовами, отчёты, функции и таблицы базы — и показывает план команд до применения, чтобы вы разобрали изменения перед запуском.

Базы могут быть на PostgreSQL или Oracle. Если рабочая база в закрытом контуре, доработку можно передать ZIP-пакетом или через Git.

Holkas — независимый инструмент, не модуль корпорации «Парус». Он переносит структуру и метаданные, а не строки документов.

## С чего начать

| | |
| --- | --- |
| [Запуск в Docker](https://holkas.belousov.net.ru/install/docker/) | один образ: веб-интерфейс, REST и MCP |
| [Windows x64](https://holkas.belousov.net.ru/install/windows/) | архивы Holkas Web и CLI без установки .NET |
| [Перенести доработку между базами](https://holkas.belousov.net.ru/tasks/test-to-prod/) | первый перенос по шагам |
| [Как это работает](https://holkas.belousov.net.ru/concepts/model/) | модель, план, порядок команд и контроль применения |
| [Что нового](https://holkas.belousov.net.ru/releases/) | заметки ко всем версиям |

> [!IMPORTANT]
> Holkas активно разрабатывается. Первый перенос делайте на копии. Перед применением плана к важной базе сделайте резервную копию приёмника.

## Загрузка

- **Windows:** архивы `holkas-web-win-x64-<версия>.zip`, `holkas-cli-win-x64-<версия>.zip` и контрольные суммы `SHA256SUMS` — на странице [последнего релиза](https://github.com/BelousovSL/Holkas-Releases/releases/latest).
- **Linux:** CLI `holkas-cli-linux-x64-<версия>.tar.gz` для скриптов и конвейеров CI/CD — там же.
- **Docker:** образ [`belousov2501/holkas`](https://hub.docker.com/r/belousov2501/holkas), теги `<версия>` и `latest`.

## Вопросы и ошибки

Создайте обращение в [Issues](https://github.com/BelousovSL/Holkas-Releases/issues/new/choose). Обращения и вложения видны всем: не публикуйте пароли, токены, строки подключения и данные из баз.
