# Web_Server

Простой веб-сервер на Go: отдаёт статические страницы, обрабатывает форму и отвечает на `/hello`.

## Стек

- **Go** (стандартная библиотека: `net/http`, `log`, `fmt`)
- **HTML** — статические страницы в папке `static/`

## Структура проекта

```
Web_Server/
├── main.go          # точка входа, роутинг и хендлеры
└── static/
    ├── index.html   # главная страница
    └── form.html    # форма (POST /form)
```

## Требования

- Go 1.20+ (или новее)

## Запуск

```bash
git clone https://github.com/assdffw/Web_Server.git
cd Web_Server
go run main.go
```

Сервер стартует на **http://localhost:8080**.

## Эндпоинты

| Метод | Путь      | Описание |
|-------|-----------|----------|
| GET   | `/`       | Отдаёт статику из `./static` (index.html) |
| GET   | `/form`   | Отдаёт `form.html` |
| POST  | `/form`   | Обрабатывает форму: принимает `name` и `address`, выводит их в ответ |
| GET   | `/hello`  | Возвращает `hello!` |

> `/hello` принимает **только GET**. Любой другой метод вернёт `404 not found` — так задумано в коде.

## Примеры

Проверка `/hello`:

```bash
curl http://localhost:8080/hello
# hello!
```

Отправка формы:

```bash
curl -X POST http://localhost:8080/form \
  -d "name=Ivan" -d "address=Moscow"
# POST request successful
# Name = Ivan
# Address = Moscow
```

## Как это работает

- `http.FileServer(http.Dir("./static"))` раздаёт статику по корню `/`.
- `formHandler` парсит форму через `r.ParseForm()` и читает поля `name` и `address`.
- `helloHandler` проверяет путь и метод, иначе возвращает 404.

## Возможные улучшения

- Валидация полей формы и защита от пустых значений.
- Возврат HTML-страницы вместо plain-text ответа после POST.
- Вынести порт в переменную окружения (`PORT`).
- Логирование запросов (middleware).
- Тесты для хендлеров (`httptest`).
- Dockerfile для запуска в контейнере.
