# API Спецификация User Profile Service

## Общие положения

### Формат ответа
Все ответы возвращаются в формате JSON и имеют следующую структуру:

Успешный ответ:
```json
{
  "status": "success",
  "data": {
    // Данные ответа
  }
}
```

Ответ с ошибкой:
```json
{
  "status": "error",
  "code": "ERROR_CODE",
  "message": "Описание ошибки"
}
```

### Аутентификация
Все защищенные endpoints требуют JWT токен в заголовке:
```
Authorization: Bearer <token>
```

## Endpoints

### Профиль пользователя

#### POST /users
Создание нового профиля пользователя.

**Тело запроса:**
```json
{
  "username": "string",
  "email": "string",
  "password": "string"
}
```

**Успешный ответ:** (201 Created)
```json
{
  "status": "success",
  "data": {
    "userId": "uuid",
    "username": "string",
    "email": "string",
    "createdAt": "date"
  }
}
```

#### GET /users/{userId}
Получение информации о пользователе.

**Параметры пути:**
- userId: UUID пользователя

**Заголовки:**
- Authorization: Bearer <token>

**Успешный ответ:** (200 OK)
```json
{
  "status": "success",
  "data": {
    "userId": "uuid",
    "username": "string",
    "email": "string",
    "savedBuilds": ["uuid"],
    "favoriteComponents": ["uuid"],
    "createdAt": "date",
    "updatedAt": "date"
  }
}
```

#### PUT /users/{userId}
Обновление информации пользователя.

**Параметры пути:**
- userId: UUID пользователя

**Заголовки:**
- Authorization: Bearer <token>

**Тело запроса:**
```json
{
  "username": "string", // опционально
  "email": "string",    // опционально
  "password": "string"  // опционально
}
```

**Успешный ответ:** (200 OK)
```json
{
  "status": "success",
  "data": {
    "userId": "uuid",
    "username": "string",
    "email": "string",
    "updatedAt": "date"
  }
}
```

#### DELETE /users/{userId}
Удаление профиля пользователя.

**Параметры пути:**
- userId: UUID пользователя

**Заголовки:**
- Authorization: Bearer <token>

**Успешный ответ:** (204 No Content)

### Сборки ПК

#### POST /users/{userId}/builds
Сохранение новой сборки ПК.

**Параметры пути:**
- userId: UUID пользователя

**Заголовки:**
- Authorization: Bearer <token>

**Тело запроса:**
```json
{
  "buildId": "uuid",
  "name": "string",
  "description": "string" // опционально
}
```

**Успешный ответ:** (201 Created)
```json
{
  "status": "success",
  "data": {
    "userId": "uuid",
    "buildId": "uuid",
    "name": "string",
    "description": "string",
    "createdAt": "date"
  }
}
```

#### GET /users/{userId}/builds
Получение списка сохраненных сборок.

**Параметры пути:**
- userId: UUID пользователя

**Параметры запроса:**
- page: number (по умолчанию: 1)
- limit: number (по умолчанию: 10)

**Заголовки:**
- Authorization: Bearer <token>

**Успешный ответ:** (200 OK)
```json
{
  "status": "success",
  "data": {
    "builds": [
      {
        "buildId": "uuid",
        "name": "string",
        "description": "string",
        "createdAt": "date"
      }
    ],
    "pagination": {
      "total": "number",
      "page": "number",
      "limit": "number",
      "pages": "number"
    }
  }
}
```

#### DELETE /users/{userId}/builds/{buildId}
Удаление сохраненной сборки.

**Параметры пути:**
- userId: UUID пользователя
- buildId: UUID сборки

**Заголовки:**
- Authorization: Bearer <token>

**Успешный ответ:** (204 No Content)

### Избранные компоненты

#### POST /users/{userId}/favorites
Добавление компонента в избранное.

**Параметры пути:**
- userId: UUID пользователя

**Заголовки:**
- Authorization: Bearer <token>

**Тело запроса:**
```json
{
  "componentId": "uuid"
}
```

**Успешный ответ:** (201 Created)
```json
{
  "status": "success",
  "data": {
    "userId": "uuid",
    "componentId": "uuid",
    "addedAt": "date"
  }
}
```

#### GET /users/{userId}/favorites
Получение списка избранных компонентов.

**Параметры пути:**
- userId: UUID пользователя

**Параметры запроса:**
- page: number (по умолчанию: 1)
- limit: number (по умолчанию: 10)

**Заголовки:**
- Authorization: Bearer <token>

**Успешный ответ:** (200 OK)
```json
{
  "status": "success",
  "data": {
    "components": [
      {
        "componentId": "uuid",
        "addedAt": "date"
      }
    ],
    "pagination": {
      "total": "number",
      "page": "number",
      "limit": "number",
      "pages": "number"
    }
  }
}
```

#### DELETE /users/{userId}/favorites/{componentId}
Удаление компонента из избранного.

**Параметры пути:**
- userId: UUID пользователя
- componentId: UUID компонента

**Заголовки:**
- Authorization: Bearer <token>

**Успешный ответ:** (204 No Content)

## Коды ошибок

| Код ошибки | HTTP код | Описание |
|------------|----------|-----------|
| USER_NOT_FOUND | 404 | Пользователь не найден |
| BUILD_NOT_FOUND | 404 | Сборка не найдена |
| COMPONENT_NOT_FOUND | 404 | Компонент не найден |
| INVALID_TOKEN | 401 | Недействительный токен |
| TOKEN_EXPIRED | 401 | Срок действия токена истек |
| FORBIDDEN | 403 | Доступ запрещен |
| VALIDATION_ERROR | 400 | Ошибка валидации данных |
| EMAIL_EXISTS | 409 | Email уже существует |
| USERNAME_EXISTS | 409 | Username уже существует |
| INTERNAL_ERROR | 500 | Внутренняя ошибка сервера |

## Ограничения и лимиты

- Максимальное количество избранных компонентов: 100
- Максимальное количество сохраненных сборок: 50
- Максимальная длина username: 50 символов
- Максимальная длина описания сборки: 500 символов
- Лимит запросов: 100 запросов в минуту на IP 