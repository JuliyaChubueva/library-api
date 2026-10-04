# Library API

Учебный проект по проектированию API-контрактов.

Проект представляет OpenAPI 3.1 контракт для сервиса «Библиотека».

## Возможности

API содержит три основных ресурса:

- Books — книги;
- Readers — читатели;
- Loans — выдачи книг.

Для Books и Readers реализован CRUD.

Для Loans реализованы:

- создание выдачи;
- получение списка;
- получение выдачи по ID;
- возврат книги через `POST /loans/{id}/return`.

## Основные бизнес-правила

- книга может быть выдана только при `available_copies > 0`;
- у читателя может быть не более 5 активных выдач;
- `due_date` должна быть позже текущей даты;
- книгу нельзя удалить при наличии активных выдач;
- читателя нельзя удалить при наличии активных выдач;
- `available_copies` вычисляется сервером;
- возврат книги не удаляет Loan, а заполняет `returned_at`.

## API version

Базовый путь API:

`/v1`

## Списки

Коллекции поддерживают:

- фильтрацию;
- сортировку;
- пагинацию через `limit` и `offset`.

Пример:

`GET /loans?status=overdue&limit=20&offset=0`

## Ошибки

API использует единый `ErrorResponse`.

Основные коды ошибок:

- `INVALID_REQUEST`
- `NOT_FOUND`
- `VALIDATION_ERROR`
- `CONFLICT`
- `BUSINESS_RULE_VIOLATION`
- `IDEMPOTENCY_MISMATCH`

## Idempotency

`POST /loans` требует заголовок `Idempotency-Key`.

Повтор запроса с тем же ключом и тем же телом не создаёт новую выдачу,
а возвращает сохранённый результат предыдущего запроса.

Ключ хранится 24 часа.

## Authorization

В дополнительной части задания добавлена Bearer JWT авторизация
через OpenAPI `securitySchemes`.

Формат заголовка:

`Authorization: Bearer <token>`

## Структура проекта

```text
library-api/
├── openapi.yaml
├── STYLE.md
├── REVIEW.md
└── README.md