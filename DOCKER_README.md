## Docker Compose Setup

This docker-compose.yml provides a production-like environment with hot-reloading for both PHP and Vue.

```yaml
services:
  # Laravel Backend (PHP-FPM + Nginx)
  backend:
    image: bitnami/laravel:11
    ports:
      - "8000:8000"
    volumes:
      - ./backend:/app
    environment:
      - DB_HOST=database
      - DB_DATABASE=laravel
      - DB_USERNAME=sail
      - DB_PASSWORD=password
    depends_on:
      - database

  # Vue Frontend
  frontend:
    image: node:20-alpine
    working_dir: /app
    ports:
      - "5173:5173"
    volumes:
      - ./frontend:/app
    command: sh -c "npm install && npm run dev -- --host"

  # Database (PostgreSQL or MySQL)
  database:
    image: postgres:15-alpine
    environment:
      - POSTGRES_DB=laravel
      - POSTGRES_USER=sail
      - POSTGRES_PASSWORD=password
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:
```
