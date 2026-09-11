<!-- Исходный файл: Holkas/docs/releases-readme.md. Публикуется в BelousovSL/Holkas-Releases. -->

# Holkas

> Публичные дистрибутивы и инструкции по установке Holkas.

[![Последний релиз](https://img.shields.io/github/v/release/BelousovSL/Holkas-Releases?display_name=tag&sort=semver&label=release)](https://github.com/BelousovSL/Holkas-Releases/releases/latest)
[![Docker-образ](https://img.shields.io/docker/v/belousov2501/holkas?sort=semver&label=Docker)](https://hub.docker.com/r/belousov2501/holkas)
[![Загрузки Docker](https://img.shields.io/docker/pulls/belousov2501/holkas)](https://hub.docker.com/r/belousov2501/holkas)

> [!IMPORTANT]
> Holkas активно разрабатывается. Перед применением плана к важной базе данных внимательно проверьте его и убедитесь, что у вас есть рабочая резервная копия.

## О проекте

Holkas — инструмент для переноса доработок приложений Парус между разными средами и СУБД. В Парусе прикладная логика хранится не только в исходном коде, но и в виде связанных метаданных и объектов базы данных. Holkas собирает их в единую модель, сравнивает источник с назначением и формирует понятный план изменений.

## Что можно переносить

Holkas работает с двумя группами объектов:

- **объекты Парус:** классы, приложения, действия, методы, вызовы методов, домены, ссылки, ограничения, сообщения, стереотипы, пиктограммы, пользовательские процедуры, отчёты, скрипты и представления;
- **объекты базы данных:** таблицы, колонки, индексы, ограничения, триггеры, функции, процедуры, представления, последовательности и составные типы.

Можно переносить как отдельные выбранные объекты, так и подготовленный ранее пакет.

## Источники и назначения

Источником данных может быть:

- база данных;
- Git-репозиторий;
- пакет в S3-совместимом хранилище;
- локальный ZIP-пакет Holkas.

Результат можно применить к базе данных, сохранить в Git или S3, скачать в виде ZIP-пакета либо сформировать SQL-файл. Это позволяет строить разные маршруты: например, `БД → БД`, `БД → Git`, `Git → БД`, `S3 → БД` или `пакет → SQL`.

## План переноса

Перед выполнением Holkas сравнивает источник с назначением и создаёт упорядоченный план команд:

- `Create` — создать отсутствующий объект;
- `Update` — обновить существующий объект;
- `Delete` — удалить лишнюю часть объекта, если это допускается правилами данного типа.

Holkas учитывает зависимости между командами и определяет безопасный порядок их выполнения. План можно изучить заранее, а отдельные команды — исключить из операции перед применением.

## Способы управления

Основной способ работы — веб-интерфейс Holkas Web. В нём можно настраивать подключения, выбирать объекты, сравнивать источники, просматривать планы переноса и историю операций.

Для автоматизации существует Holkas CLI с командами `status`, `connections list`, `connections test`, `migrate` и `compare`. Также Holkas предоставляет REST API и MCP-интерфейс для интеграции со сценариями и AI-агентами.

## Дистрибутивы

| Дистрибутив | Назначение | Загрузка |
| --- | --- | --- |
| Holkas Web | Веб-интерфейс и API для Windows x64 | [`holkas-web-win-x64-<версия>.zip`](https://github.com/BelousovSL/Holkas-Releases/releases/latest) |
| Holkas CLI | Консольный клиент для Windows x64 | [`holkas-cli-win-x64-<версия>.zip`](https://github.com/BelousovSL/Holkas-Releases/releases/latest) |
| Docker-образ | Сервер Linux или Docker Desktop | [`belousov2501/holkas`](https://hub.docker.com/r/belousov2501/holkas) |

Архивы Windows содержат автономные сборки и не требуют отдельной установки .NET. Сейчас они не имеют цифровой подписи, поэтому при появлении предупреждения SmartScreen проверьте опубликованную контрольную сумму SHA-256.

## Быстрый запуск в Docker

```bash
docker volume create holkas-data

docker run -d \
  --name holkas \
  --restart unless-stopped \
  -p 8080:8080 \
  -v holkas-data:/data \
  -e HolkasWeb__BootstrapAdminPassword='задайте-сложный-пароль' \
  belousov2501/holkas:latest
```

Откройте <http://localhost:8080> и войдите под именем `admin`, используя указанный выше пароль.

В томе `holkas-data` находятся база состояния, артефакты, журналы и криптографические ключи. Сохраняйте его при обновлении и повторном создании контейнера. Для воспроизводимого развёртывания используйте конкретный тег, например `0.1.0`, вместо `latest`.

### Docker Compose

```yaml
services:
  holkas:
    image: belousov2501/holkas:latest
    container_name: holkas
    restart: unless-stopped
    ports:
      - "8080:8080"
    environment:
      HolkasWeb__BootstrapAdminPassword: ${HOLKAS_ADMIN_PASSWORD}
    volumes:
      - holkas-data:/data

volumes:
  holkas-data:
```

Добавьте `HOLKAS_ADMIN_PASSWORD=задайте-сложный-пароль` в локальный файл `.env`, ограничьте доступ к нему и выполните `docker compose up -d`.

## Установка Holkas Web в Windows

1. Скачайте `holkas-web-win-x64-<версия>.zip` и `SHA256SUMS` со страницы [последнего релиза](https://github.com/BelousovSL/Holkas-Releases/releases/latest).
2. При необходимости сравните SHA-256 архива со значением в `SHA256SUMS`.
3. Распакуйте архив и откройте PowerShell во вложенном каталоге `holkas-web-win-x64`.
4. Запустите Holkas:

```powershell
$env:HolkasWeb__Mode = "Standalone"
$env:HolkasWeb__BootstrapAdminPassword = "задайте-сложный-пароль"
$env:HolkasWeb__DataPath = "$PWD\data"
$env:HolkasWeb__StatePath = "$PWD\data\holkasweb.db"
$env:ASPNETCORE_URLS = "http://localhost:5000"
.\Holkas.Web.exe
```

Откройте <http://localhost:5000> и войдите под именем `admin`. Пока Holkas работает, окно PowerShell должно оставаться открытым. Для остановки нажмите `Ctrl+C`.

Начальный пароль применяется только при создании первого администратора. После входа смените его в веб-интерфейсе.

## Установка CLI в Windows

Скачайте и распакуйте `holkas-cli-win-x64-<версия>.zip`, затем проверьте сервер:

```powershell
.\holkas.exe status --server http://localhost:5000
```

Для команд, требующих авторизации, создайте токен для скрипта в разделе **Полномочия → Мой агент** и настройте текущий сеанс PowerShell:

```powershell
$env:HOLKAS_SERVER = "http://localhost:5000"
$env:HOLKAS_API_KEY = "hpat_..."
.\holkas.exe connections list
```

Полный список команд и параметров выводится командой `holkas --help`.

## Обновление и резервное копирование

- **Docker:** загрузите новый образ и пересоздайте контейнер с тем же томом `holkas-data`.
- **Windows:** остановите Holkas, сохраните копию каталога `data`, распакуйте новый релиз отдельно и укажите ему существующий каталог данных.
- Создавайте резервную копию постоянных данных перед каждым обновлением.
- Не публикуйте Holkas напрямую в интернете. Для удалённого доступа используйте обратный прокси с TLS и сетевые ограничения.

## Проверка архива Windows

Замените имя файла номером скачанной версии:

```powershell
(Get-FileHash .\holkas-web-win-x64-0.1.0.zip -Algorithm SHA256).Hash.ToLower()
Get-Content .\SHA256SUMS
```

Полученное значение должно совпасть с соответствующей строкой в `SHA256SUMS`.

## Поддержка и обратная связь

Если Holkas работает не так, как ожидалось, или у вас появился вопрос, создайте обращение в
[разделе Issues](https://github.com/BelousovSL/Holkas-Releases/issues/new/choose). Выберите форму
«Сообщить об ошибке» либо «Задать вопрос или предложить улучшение». Уже созданные обращения и
ответы доступны в [общем списке](https://github.com/BelousovSL/Holkas-Releases/issues).

Обращения и вложения в этом репозитории видны всем. Не публикуйте пароли, токены, строки
подключения и конфиденциальные данные из баз. Перед отправкой журнала операции проверьте его
содержимое.

## Состав релиза

Каждый успешный выпуск содержит:

- архивы Holkas Web и Holkas CLI для Windows с номером версии;
- файл контрольных сумм `SHA256SUMS`;
- Docker-теги `<версия>` и `latest`.

Все версии доступны на странице [Releases](https://github.com/BelousovSL/Holkas-Releases/releases) и в [Docker Hub](https://hub.docker.com/r/belousov2501/holkas/tags).
