## tulsu_devops-case1

Кейс: автоматизация сборки и запуска Flask-приложения на GitHub + Docker + GitHub Actions

## Содержание
1. [Структура проекта](#структура-проекта)  
2. [Локальный запуск](#локальный-запуск)  
3. [Docker](#docker)  
4. [CI / GitHub Actions](#ci--github-actions)  

---

## Структура проекта
```text
.
├── app.py              # Flask-приложение
├── requirements.txt    # Зависимости Python
├── Dockerfile          # Сборка образа
├── .dockerignore       # Исключения для Docker
├── README.md           # Этот файл
└── .github/
    └── workflows/
        └── main.yml    # CI-пайплайн
```
---

## Локальный запуск

1. Клонировать репозиторий и перейти в папку:

   ```bash
   git clone https://github.com/LoqSore/tulsu_devops-case1.git
   cd tulsu_devops-case1
   ```
2. (Опционально) Создать и активировать виртуальное окружение:

   ```bash
   python -m venv venv
   # Windows PowerShell:
   venv\Scripts\activate
   # Git Bash / WSL:
   source venv/Scripts/activate
   ```
3. Установить зависимости:

   ```bash
   pip install -r requirements.txt
   ```
4. Запустить приложение:

   ```bash
   python app.py
   ```
5. Открыть браузер: [http://localhost:6996](http://localhost:6996) — должно отдать "Hello, world!"

---

## Docker

1. Собрать Docker-образ:

   ```bash
   docker build -t tulsu_devops-case1 .
   ```
2. Запустить контейнер:

   ```bash
   docker run -d -p 6996:6996 --name tulsu_devops-case1 tulsu_devops-case1
   ```
3. Проверить работу:

   ```bash
   curl http://localhost:6996
   # → Hello, world!
   ```
4. Остановить и удалить контейнер:

   ```bash
   docker stop tulsu_devops-case1
   docker rm tulsu_devops-case1
   ```

---

## CI / GitHub Actions

Workflow-файл: `.github/workflows/main.yml`. При каждом `push` в ветку `main` выполняет:

1. Checkout кода `actions/checkout@v4`
2. Установку Python 3.11 `actions/setup-python@v5`
3. Установку зависимостей и `flake8`
4. Линтинг `flake8 .`
5. Сборку Docker-образа
6. Смоук-тест: запускает контейнер и проверяет HTTP 200
7. Очистку тестового контейнера

---
