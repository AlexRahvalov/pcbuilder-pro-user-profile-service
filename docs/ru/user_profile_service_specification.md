# Техническое задание: Сервис профилей пользователей PCBuilder Pro

## 1. Цель:

* Разработка микросервиса управления профилями пользователей для PCBuilder Pro. Сервис должен обеспечивать хранение и управление данными пользователей, включая личную информацию, сохраненные сборки ПК и избранные компоненты.

## 2. Функциональные требования:
### Управление профилями:
* Сервис должен обеспечивать создание, чтение, обновление и удаление (CRUD) профилей пользователей.
* Хранение основной информации: имя пользователя, email, хешированный пароль.
* Управление дополнительными данными: сохраненные сборки, избранные компоненты.

### Интеграция с сервисом аутентификации:
* Проверка JWT токенов для авторизации запросов.
* Взаимодействие при регистрации новых пользователей.
* Обеспечение безопасного доступа к данным профиля.

### Управление сборками:
* Сохранение ссылок на сборки ПК пользователя.
* Получение списка сохраненных сборок.
* Удаление сборок из профиля.

### Управление избранным:
* Добавление компонентов в избранное.
* Получение списка избранных компонентов.
* Удаление компонентов из избранного.

## 3. Нефункциональные требования:
### Безопасность:
* Защита от SQL-инъекций через ORM.
* Валидация входных данных.
* Проверка прав доступа к данным.

### Производительность:
* Быстрый доступ к данным через индексы.
* Эффективная работа с большими списками (пагинация).

### Масштабируемость:
* Возможность горизонтального масштабирования.
* Оптимизация работы с базой данных.

### Надежность:
* Обработка ошибок и исключений.
* Логирование операций.
* Резервное копирование данных.

## 4. Технологический стек:
### Язык программирования:
* Node.js

### Фреймворк:
* Express.js

### ORM:
* Sequelize

### База данных:
* PostgreSQL

### Дополнительные библиотеки:
* uuid: генерация уникальных идентификаторов
* jsonwebtoken: проверка JWT токенов
* cors: настройка CORS
* winston: логирование

## 5. База данных:

### Основная таблица users:
```sql
CREATE TABLE users (
  user_id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  username VARCHAR(255) NOT NULL UNIQUE,
  email VARCHAR(255) NOT NULL UNIQUE,
  hashed_password VARCHAR(255) NOT NULL,
  saved_builds UUID[],
  favorite_components UUID[],
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_username ON users(username);
```

## 6. Структура проекта:
```
user-profile-service/
├── src/
│   ├── config/
│   │   ├── database.js
│   │   └── cors.js
│   ├── controllers/
│   │   └── userController.js
│   ├── models/
│   │   └── userModel.js
│   ├── routes/
│   │   └── userRoutes.js
│   ├── middleware/
│   │   ├── auth.js
│   │   └── errorHandler.js
│   ├── services/
│   │   └── userService.js
│   └── index.js
├── tests/
│   └── user.test.js
└── docs/
    └── ru/
        └── README.md
```

## 7. Зависимости:
* `express`: Веб-фреймворк
* `sequelize`: ORM для работы с базой данных
* `pg`: PostgreSQL клиент
* `jsonwebtoken`: Проверка JWT токенов
* `cors`: Настройка CORS
* `uuid`: Генерация UUID
* `winston`: Логирование

## 8. API:
Детальная спецификация API находится в отдельном документе (api.md).

## 9. Обработка ошибок:

### Ошибки валидации:
```json
{
  "status": "error",
  "code": "VALIDATION_ERROR",
  "message": "Ошибка валидации данных"
}
```

### Ошибки доступа:
```json
{
  "status": "error",
  "code": "FORBIDDEN",
  "message": "Доступ запрещен"
}
```

### Ошибки данных:
```json
{
  "status": "error",
  "code": "USER_NOT_FOUND",
  "message": "Пользователь не найден"
}
```

## 10. Требования к тестированию:

### Unit-тесты:
1. Тесты моделей:
   - Валидация данных пользователя
   - Работа с массивами сборок и компонентов
   - Хуки жизненного цикла

2. Тесты сервисов:
   - CRUD операции
   - Обработка ошибок
   - Валидация данных

### Интеграционные тесты:
1. Тесты API:
   - Все CRUD операции
   - Авторизация и доступ
   - Пагинация и фильтрация

2. Тесты базы данных:
   - Транзакции
   - Индексы
   - Ограничения

### End-to-end тесты:
1. Сценарии использования:
   - Создание и обновление профиля
   - Работа со сборками
   - Работа с избранным

### Тесты производительности:
1. Нагрузочное тестирование:
   - Множественные запросы
   - Большие наборы данных
   - Параллельные операции 