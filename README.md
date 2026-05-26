# itmo-handlebars

Учебный проект для рендеринга шаблонов с поддержкой Handlebars, Pug, Nunjucks и Jinja2. Включает CLI, HTTP API и веб-песочницу на React.

## Возможности

- **4 шаблонизатора** — Handlebars, Pug, Nunjucks, Jinja2
- **⚙️ CLI-инструмент** — пакетный рендеринг для CI/CD
- **🌐 HTTP-сервер** — REST API для интеграции
- **🎨 Веб-песочница** — интерактивный редактор с выбором движка
- **⏱️ Метрики** — измерение времени рендеринга
- **📋 Копирование** — результат одним кликом в буфер обмена
- **🧪 Тесты** — 5 тестов Vitest, все проходят

## 🖼️ Интерфейс веб-песочницы
![Веб-песочница](./screenshots/sandbox.png)

## Установка

```bash
npm install
```

## Быстрый старт

Запуск примера CLI:

```bash
npm run render:example
```

Запуск веб-песочницы:

```bash
npm run sandbox
```

После запуска откройте:

`http://127.0.0.1:3000`

## Скрипты

- `npm test` — запуск тестов Vitest.
- `npm run render:example` — рендер примера из `templates/example.hbs` и `data/context.json`.
- `npm run sandbox` — запуск HTTP-сервера с веб-интерфейсом.

## CLI

Точка входа: `bin/hbs-runner.js`

Аргументы:

| Аргумент | Обязательный | Описание | Описание |
| --- | --- | --- | --- |
| `--template <path>` | да | Путь к `.hbs` шаблону. | --- |
| `--context <path>` | да | Путь к `.json` файлу с контекстом. | --- |
| `--out <path>` | нет | Путь для записи результата | stdout |
| `--engine <path>` | нет | Шаблонизатор | handlebars |

<b>Доступные движки</b>: handlebars, pug, nunjucks, jinja2

<b>Примеры:</b>

```bash
# Handlebars (по умолчанию)
node bin/hbs-runner.js --template templates/example.hbs --context data/context.json --out dist/example.html

# Pug
node bin/hbs-runner.js --template template.pug --context data.json --engine pug
```

## Веб-песочница

Бэкенд:
- `GET /` — страница песочницы;
- `POST /api/render` — рендер шаблона.

<b>Тело запроса `POST /api/render`:</b>

```json
{
  "template": "Hello, {{name}}!",
  "context": "{\"name\":\"ITMO\"}"
}
```

Ответ:

```json
{
  "output": "Hello, ITMO!"
}
```

<b>Пример cURL:</b>
```bash
curl -X POST http://localhost:3000/api/render \
  -H "Content-Type: application/json" \
  -d '{
    "template": "h1= greeting",
    "context": "{\"greeting\":\"Привет, Pug!\"}",
    "engine": "pug"
  }'
```

<b>Ошибки:</b><br>
`400` — невалидный JSON, отсутствует шаблон, ошибка рендеринга<br>
`500` — внутренняя ошибка сервера

## Структура проекта
```text
itmo-handlebars/
├── bin/
│   └── hbs-runner.js          # CLI точка входа
├── src/
│   ├── renderer.js            # Основная логика (рендер, CLI utils)
│   ├── server.js              # HTTP сервер + API
│   └── engines
│       ├── handlebars.js
│       ├── index.js
│       ├── nunjucks.js
│       └── pug.js
├── public/
│   ├── index.html             # HTML обёртка песочницы
│   ├── app.jsx                # React компонент (с выбором движков)
│   └── styles.css             # Стили с анимациями
├── templates/
│   └── example.hbs            # Пример шаблона Handlebars
├── data/
│   ├── context.json           # Пример контекста
│   └── broken-context.json    # Для тестирования ошибок
├── test/
│   └── renderer.test.mjs      # 5 тестов Vitest
├── package.json
├── REPORT.md
└── README.md
```

## Скрипты npm
|  Команда  |  Описание  |
| --------  | --------  |
|`npm test`|Запуск тестов (5 тестов)|
|`npm run render:example`|CLI пример с Handlebars|
|`npm run sandbox`|Запуск веб-песочницы|

## Тестирование
Покрытие:<br>
✅ Простой рендер<br>
✅ Вложенные поля<br>
✅ Блоки `{{#each}}`<br>
✅ Ошибка JSON (невалидный контекст)<br>
✅ Запись результата в файл

## Веб-песочница: технический стек
- React 18 (UMD + CDN)
- Babel Standalone (транспиляция JSX в браузере)
- Чистый CSS с кастомными анимациями
- Нет сборки — подходит для учебных целей

## Лицензия
MIT License — подробности в файле <a href='https://license/'>LICENSE</a>
