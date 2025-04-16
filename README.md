# 🚀 Kubercode Compose

<div align="center">
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker">
  <img src="https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white" alt="MongoDB">
  <img src="https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white" alt="Redis">
  <img src="https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white" alt="Nginx">
</div>

## 📋 Содержание

-   [Архитектура](#архитектура)
-   [Запуск проекта](#запуск-проекта)
-   [Доступ к сервисам](#доступ-к-сервисам)
-   [Добавление новых микросервисов](#добавление-новых-микросервисов)
-   [Мониторинг](#мониторинг)
-   [Безопасность](#безопасность)
-   [Разработка](#разработка)
-   [Поддержка](#поддержка)

## 🏗️ Архитектура

Проект представляет собой микросервисную архитектуру, состоящую из нескольких сервисов, объединенных с помощью Docker Compose.

### 🔐 SSO Service (kubercode-sso)

<div align="center">
  <img src="https://img.shields.io/badge/Порт-1488-blue" alt="Port">
  <img src="https://img.shields.io/badge/Статус-Активный-green" alt="Status">
</div>

-   **Описание**: Сервис аутентификации и авторизации
-   **Зависимости**: MongoDB, Redis
-   **Функционал**:
    -   ✅ Регистрация пользователей
    -   ✅ Аутентификация (JWT)
    -   ✅ Управление сессиями
    -   ✅ Выход из системы
    -   ✅ Обновление токенов

### 🌐 Nginx (kubercode-nginx)

<div align="center">
  <img src="https://img.shields.io/badge/Порт-8080-blue" alt="Port">
  <img src="https://img.shields.io/badge/Статус-Активный-green" alt="Status">
</div>

-   **Описание**: Прокси-сервер для маршрутизации запросов
-   **Конфигурация**: `nginx/nginx.conf`
-   **Функционал**:
    -   🔄 Балансировка нагрузки
    -   🛣️ Маршрутизация запросов
    -   🔒 SSL/TLS терминация

### 🗄️ MongoDB (kubercode-mongodb)

<div align="center">
  <img src="https://img.shields.io/badge/Порт-27017-blue" alt="Port">
  <img src="https://img.shields.io/badge/Статус-Активный-green" alt="Status">
</div>

-   **Описание**: Основная база данных
-   **Данные для подключения**:
    ```yaml
    username: user
    password: password
    uri: mongodb://user:password@mongodb:27017
    ```
-   **Функционал**:
    -   💾 Хранение пользовательских данных
    -   🔐 Хранение сессий
    -   📊 Хранение метаданных

### 🔥 Redis (kubercode-redis)

<div align="center">
  <img src="https://img.shields.io/badge/Порт-6379-blue" alt="Port">
  <img src="https://img.shields.io/badge/Статус-Активный-green" alt="Status">
</div>

-   **Описание**: Кэш-сервер
-   **Данные для подключения**:
    ```yaml
    password: my-password
    port: 6379
    databases: 16
    ```
-   **Функционал**:
    -   ⚡ Кэширование данных
    -   🔄 Хранение сессий
    -   🔒 Управление блокировками

## 🚀 Запуск проекта

### 📋 Предварительные требования

-   🐳 Docker
-   🎵 Docker Compose

### 📝 Шаги для запуска

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

## 🔗 Доступ к сервисам

| Сервис      | URL                       |
| ----------- | ------------------------- |
| SSO Service | http://localhost:1488     |
| Nginx       | http://localhost:8080     |
| MongoDB     | mongodb://localhost:27017 |
| Redis       | redis://localhost:6379    |

## ➕ Добавление новых микросервисов

Для добавления нового микросервиса:

1. Создайте директорию для сервиса
2. Создайте Dockerfile
3. Добавьте конфигурацию в `docker-compose.yml`:
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
4. Обновите конфигурацию Nginx

## 📊 Мониторинг

```bash
# Просмотр логов конкретного сервиса
docker-compose logs -f [service-name]

# Просмотр всех логов
docker-compose logs -f
```

## 🛡️ Безопасность

<div align="center">
  <img src="https://img.shields.io/badge/Сеть-Изолированная-blue" alt="Network">
  <img src="https://img.shields.io/badge/Аутентификация-JWT-green" alt="Auth">
</div>

-   🔒 Все сервисы работают в изолированной сети `app_network`
-   🔑 MongoDB и Redis защищены паролями
-   🎫 JWT токены подписываются секретным ключом
-   🛡️ Nginx обеспечивает дополнительный уровень безопасности

## 💻 Разработка

### 🛠️ Локальная разработка

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

### 📝 Логирование

```bash
# Просмотр логов в реальном времени
docker-compose logs -f [service-name]

# Просмотр последних 100 строк логов
docker-compose logs --tail=100 [service-name]
```

## ❓ Поддержка

При возникновении проблем:

1. 🔍 Проверьте логи сервисов
2. 🔌 Убедитесь, что все зависимости запущены
3. 🌐 Проверьте конфигурацию сети
4. 🔌 Проверьте доступность портов

<div align="center">
  <sub>Built with ❤️ by Kubercode Team</sub>
</div>
