# Kubercode Compose

Проект представляет собой микросервисную архитектуру, состоящую из нескольких сервисов, объединенных с помощью Docker Compose.

## Архитектура

Проект состоит из следующих сервисов:

### 1. SSO Service (kubercode-sso)

-   **Порт**: 1488
-   **Описание**: Сервис аутентификации и авторизации
-   **Зависимости**: MongoDB, Redis
-   **Функционал**:
    -   Регистрация пользователей
    -   Аутентификация (JWT)
    -   Управление сессиями
    -   Выход из системы
    -   Обновление токенов

### 2. Nginx (kubercode-nginx)

-   **Порт**: 8080
-   **Описание**: Прокси-сервер для маршрутизации запросов
-   **Конфигурация**: `nginx/nginx.conf`
-   **Функционал**:
    -   Балансировка нагрузки
    -   Маршрутизация запросов
    -   SSL/TLS терминация

### 3. MongoDB (kubercode-mongodb)

-   **Порт**: 27017
-   **Описание**: Основная база данных
-   **Данные для подключения**:
    -   Username: user
    -   Password: password
    -   URI: mongodb://user:password@mongodb:27017
-   **Функционал**:
    -   Хранение пользовательских данных
    -   Хранение сессий
    -   Хранение метаданных

### 4. Redis (kubercode-redis)

-   **Порт**: 6379
-   **Описание**: Кэш-сервер
-   **Данные для подключения**:
    -   Password: my-password
    -   Port: 6379
    -   Databases: 16
-   **Функционал**:
    -   Кэширование данных
    -   Хранение сессий
    -   Управление блокировками

## Запуск проекта

### Предварительные требования

-   Docker
-   Docker Compose

### Шаги для запуска

1. Клонируйте репозиторий:

```bash
git clone <repository-url>
cd kubercode-compose
```

2. Запустите проект:

```bash
docker-compose up --build
```

3. Проверьте статус контейнеров:

```bash
docker-compose ps
```

## Доступ к сервисам

-   SSO Service: http://localhost:1488
-   Nginx: http://localhost:8080
-   MongoDB: mongodb://localhost:27017
-   Redis: redis://localhost:6379

## Добавление новых микросервисов

Для добавления нового микросервиса:

1. Создайте директорию для сервиса
2. Создайте Dockerfile
3. Добавьте конфигурацию в docker-compose.yml:

```yaml
new-service:
    container_name: kubercode-new-service
    networks:
        - app_network
    build:
        context: ./new-service
        dockerfile: Dockerfile
    ports:
        - "PORT:PORT"
    environment:
        - ENV_VAR1=value1
        - ENV_VAR2=value2
    depends_on:
        - kubercode-sso
        - mongodb
        - redis
```

4. Обновите конфигурацию Nginx для маршрутизации запросов к новому сервису

## Мониторинг

Для мониторинга состояния сервисов используйте:

```bash
docker-compose logs -f [service-name]
```

## Остановка проекта

```bash
docker-compose down
```

## Безопасность

-   Все сервисы работают в изолированной сети `app_network`
-   MongoDB и Redis защищены паролями
-   JWT токены подписываются секретным ключом
-   Nginx обеспечивает дополнительный уровень безопасности

## Разработка

### Локальная разработка

1. Для разработки конкретного сервиса:

```bash
cd sso  # или другой сервис
# внесите изменения
docker-compose up --build kubercode-sso
```

2. Для перезапуска конкретного сервиса:

```bash
docker-compose restart kubercode-sso
```

### Логирование

Логи доступны через:

```bash
docker-compose logs -f [service-name]
```

## Поддержка

При возникновении проблем:

1. Проверьте логи сервисов
2. Убедитесь, что все зависимости запущены
3. Проверьте конфигурацию сети
4. Проверьте доступность портов
