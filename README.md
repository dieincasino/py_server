# Python HTTP Server + Nginx

## Описание

Простое веб-приложение на Python, работающее за Nginx reverse proxy в Docker.
Backend недоступен напрямую — только через Nginx внутри Docker сети.

---

## Архитектура

```
Client → Nginx (host:80) → Docker network → Backend:8080
```

Nginx принимает HTTP запросы и проксирует их в Python HTTP сервер.

---

## Структура проекта

```
├── backend/
│   ├── Dockerfile
│   ├── .dockerignore
│   └── app.py
├── nginx/
│   └── nginx.conf
├── docker-compose.yml
├── .env.example
└── README.md
```

---

## Требования

- Docker
- Docker Compose

---

## Запуск

Скопируй `.env.example` в `.env`:

```bash
cp .env.example .env
```

Запусти проект:

```bash
docker compose up -d --build
```

---

## Проверка

```bash
curl http://localhost
```

Ожидаемый ответ:

```
Hello from Effective Mobile!
```

---

## Остановка

```bash
docker compose down
```

---

## Как работает система

1. Docker Compose поднимает два сервиса:
   - **backend** — слушает порт 8080 внутри контейнера, не публикуется наружу
   - **nginx** — reverse proxy, слушает порт 80
2. Оба сервиса находятся в одной Docker сети `app_network`
3. Nginx принимает запрос от клиента и проксирует его на backend по имени сервиса
4. Backend возвращает ответ, Nginx передаёт его клиенту

---

## Компоненты

### Backend

- Python `http.server`
- Слушает порт 8080 внутри контейнера
- Недоступен снаружи (только `expose`)
- Запускается от непривилегированного пользователя `user` (не root)
- Healthcheck каждые 30 секунд

### Nginx

- Официальный образ `nginx:1.27-alpine`
- Принимает запросы на порт 80
- Проксирует на backend с передачей заголовков Host, X-Real-IP, X-Forwarded-For
- Healthcheck каждые 30 секунд

---

## Используемые технологии

- Python 3.12
- Nginx 1.27
- Docker
- Docker Compose
