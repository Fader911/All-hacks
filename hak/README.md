# AI Test Assistant (Webant Hackathon 2025)

AI-помощник тестировщика: генерирует тест-кейсы из текстового описания, преобразует их в автотесты на Playwright, позволяет редактировать и сохранять в GitHub, а также делает AI-ревью тестов.

## Быстрый старт

1) Установите зависимости backend:

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

2) Настройте окружение:
- Скопируйте `.env.example` в `.env` и заполните переменные.

3) Запустите backend:

```bash
uvicorn backend.main:app --reload
```

4) Установите Playwright окружение (опционально для локального запуска тестов):

```bash
cd tests
npm install
npx playwright install
```

5) Откройте минимальный фронтенд:
- Откройте файл `frontend/index.html` в браузере (или подайте его через любой HTTP сервер).

6) Для Playwright на Python:

```bash
pip install -r requirements.txt
playwright install
pytest -q -k example -s
```

## Структура

- `backend/` — FastAPI API
- `backend/services/` — интеграция с LLM, GitHub и генерация кода тестов
- `tests/` — проект Playwright (TS)
- `frontend/` — простой HTML интерфейс
- `python_tests/` — директория для автотестов на Python (pytest + playwright)

## API (основные эндпоинты)

- POST `/generate/test-cases` — генерация тест-кейсов из описания.
- POST `/generate/test-code` — генерация кода автотеста из тест-кейса (ts/js/python).
- POST `/review/test` — AI ревью кода автотеста.
- POST `/save/local` — сохранение файла локально в репо.
- POST `/save/github` — сохранение/обновление файла через GitHub Contents API.
- POST `/generate/demo-app` — сгенерировать демо-приложение (index.html, script.js, styles.css).
- POST `/git/push` — локальный git add/commit/push (нужен настроенный origin).
- POST `/tests/run` — запуск тестов (body.kind: ts/js/python).

Примеры тел запросов смотрите в `backend/services/schemas.py`.

## Переменные окружения

- `OPENAI_API_KEY` — API ключ для OpenAI (или совместимых провайдеров).
- `OPENAI_BASE_URL` — базовый URL провайдера (необязательно).
- `OPENAI_MODEL` — модель (по умолчанию gpt-4o-mini).
- `GITHUB_TOKEN` — PAT с доступом к репозиторию.
- `DEFAULT_REPO_ROOT` — локальный путь, куда сохранять файлы.
- `AI_PROVIDER` — провайдер ИИ: `openai` (по умолчанию) или `ollama` (бесплатно, локально).
- Для `ollama`: установите Ollama и модель (`OLLAMA_BASE_URL`=http://localhost:11434, `OLLAMA_MODEL`=qwen2.5:7b-instruct или любая установленная).

## Запуск Playwright тестов

```bash
cd tests
npm run test
```

Сгенерированные тесты сохраняйте в `tests/e2e/*.spec.ts`. Playwright конфигурация хранится в `tests/playwright.config.ts`.

Для Python:
```bash
pytest -q
```

Сгенерированные Python тесты сохраняйте в `python_tests/*.py`.

## CLI

Пример end-to-end:
```bash
python -m backend.cli --requirements "Логин с email и паролем" --format markdown --target python --out output --push
```

- Сохранит test_cases.md/json, сгенерирует автотест (Python или TS/JS), создаст демо-приложение и опционально сделает git push в настроенный origin.

## Лицензия

MIT


