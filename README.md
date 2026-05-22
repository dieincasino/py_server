# Python Сервер

## Описание

Простое веб-приложение на Python, работающее за Nginx reverse proxy в Docker.

Backend не доступен напрямую и используется только внутри Docker сети.

---
## Архитектура

```bash
Client → Nginx (host:80)
        → Docker network → Backend:8080
```

Nginx принимает HTTP запросы и проксирует их в Python HTTP сервер.

---
## Структура проекта

```bash
├── backend/
│ ├── Dockerfile
│ └── app.py
├── nginx/
│ └── nginx.conf
├── docker-compose.yml
└── README.md
```

---

## Требования

- Docker
- Docker Compose

---
## Запуск

```bash
docker compose up -d --build
```

---
## Проверка

```bash
curl http://localhost
```

---
## Остановка

```bash
docker compose down
```

---
## Как работает система

1. Docker Compose поднимает два сервиса:
    - backend слушает 8080 внутри контейнера (не публикуется наружу)
    - nginx (reverse proxy на 80)
2. Оба сервиса находятся в одной Docker сети
3. Nginx обращается к backend по имени сервиса `backend`
4. Backend возвращает ответ, который проксируется клиенту

---
## Компоненты

### Backend
- Python HTTPServer
- слушает порт 8080 внутри контейнера
- не доступен извне
- запускается от непривилегированного пользователя `user` (не root)

### Nginx
- reverse proxy
- принимает запросы на порт 80
- проксирует в backend

### Network
- изолированная docker bridge сеть
- взаимодействие сервисов через DNS Docker
