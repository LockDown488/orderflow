# OrderFlow

Backend одного интернет-магазина. Java 21, Spring Boot 4, PostgreSQL.

## Локальная PostgreSQL (OF-002)

Нужен запущенный Docker Desktop с Linux containers и Docker Compose.
Из корня проекта в PowerShell:

```powershell
Copy-Item .env.example .env
```

Заполните `POSTGRES_PASSWORD` в `.env` локальным паролем. Файл исключён из Git.
Остальные значения можно оставить из шаблона. Если порт занят, измените
`POSTGRES_PORT`, например на `5433`.

```powershell
docker compose config --quiet
docker compose up -d --wait postgres
docker compose ps
'SELECT 1;' | docker compose exec -T postgres sh -c 'psql -U "$POSTGRES_USER" -d "$POSTGRES_DB" -v ON_ERROR_STOP=1'
```

В `ps` ожидается `healthy`, SQL должен вернуть `1`. Healthcheck проверяет
готовность сервера; выполнение SQL дополнительно проверяет доступ к БД.

Подключение с компьютера (например, из IntelliJ IDEA):

- Host: `127.0.0.1`
- Port: `5432` (либо `POSTGRES_PORT` из `.env`)
- Database / User: `orderflow` (либо значения из `.env`)
- Password: значение `POSTGRES_PASSWORD` из `.env`
- JDBC URL по умолчанию: `jdbc:postgresql://127.0.0.1:5432/orderflow`

`.env` автоматически читает Compose; Spring Boot сам этот файл не загружает.
Datasource и миграции Liquibase настраиваются отдельно в OF-003.
Пользователь образа `POSTGRES_USER` имеет права суперпользователя;
эта конфигурация предназначена для локальной разработки.

```powershell
docker compose logs --tail 50 postgres
docker compose stop postgres
docker compose start postgres
docker compose down
```

`down` удаляет контейнер и сеть, но сохраняет named volume `postgres_data`.
Не добавляйте `-v`, если хотите сохранить данные. Для PostgreSQL 18 volume
монтируется в `/var/lib/postgresql`, внутри используется каталог версии.
Переменные `POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD` инициализируют
только пустой volume: изменение `.env` не меняет уже созданные роли и пароли.

Проверка сохранности данных: создайте тестовую запись в отдельной схеме,
выполните `docker compose up -d --wait --force-recreate postgres`, проверьте
запись и удалите тестовую схему. Прикладную схему создаёт только Liquibase.

Справка: [официальный образ PostgreSQL](https://hub.docker.com/_/postgres).
