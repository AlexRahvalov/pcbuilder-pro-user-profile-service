# User Profile Service - Документация

## Содержание
1. [Общее описание](#1-общее-описание)
2. [Архитектура](#2-архитектура)
3. [API](#3-api)
4. [База данных](#4-база-данных)
5. [Безопасность](#5-безопасность)
6. [Развертывание](#6-развертывание)
7. [Разработка](#7-разработка)
8. [Тестирование](#8-тестирование)

## 1. Общее описание

User Profile Service - микросервис, отвечающий за управление профилями пользователей в системе PCBuilder Pro. Сервис обеспечивает хранение и управление пользовательскими данными, включая личную информацию, сохраненные сборки ПК и избранные компоненты.

### 1.1 Основные функции:
- Управление профилями пользователей (CRUD операции)
- Хранение сохраненных сборок ПК
- Управление списком избранных компонентов
- Интеграция с сервисом аутентификации

### 1.2 Технологический стек:
- Node.js
- Express.js
- PostgreSQL
- Sequelize ORM
- Jest (тестирование)

## 2. Архитектура

### 2.1 Структура проекта:
```
user-profile-service/
├── src/
│   ├── config/          # Конфигурационные файлы
│   ├── controllers/     # Контроллеры
│   ├── models/          # Модели данных
│   ├── routes/          # Маршруты API
│   ├── middleware/      # Промежуточные обработчики
│   ├── services/        # Бизнес-логика
│   └── index.js         # Точка входа
├── tests/               # Тесты
└── docs/                # Документация
```

### 2.2 Взаимодействие с другими сервисами:
- Authentication Service: Проверка JWT токенов
- Catalog Service: Получение информации о компонентах
- Build Service: Управление сборками ПК

## 3. API

### 3.1 Endpoints

#### Управление профилем
| Метод | Endpoint | Описание | Права доступа |
|-------|----------|----------|---------------|
| POST | `/users` | Создание профиля | Public |
| GET | `/users/{userId}` | Получение профиля | Auth |
| PUT | `/users/{userId}` | Обновление профиля | Auth |
| DELETE | `/users/{userId}` | Удаление профиля | Auth |

#### Управление сборками
| Метод | Endpoint | Описание | Права доступа |
|-------|----------|----------|---------------|
| POST | `/users/{userId}/builds` | Сохранение сборки | Auth |
| GET | `/users/{userId}/builds` | Получение всех сборок | Auth |
| DELETE | `/users/{userId}/builds/{buildId}` | Удаление сборки | Auth |

#### Управление избранным
| Метод | Endpoint | Описание | Права доступа |
|-------|----------|----------|---------------|
| POST | `/users/{userId}/favorites` | Добавление в избранное | Auth |
| GET | `/users/{userId}/favorites` | Получение избранного | Auth |
| DELETE | `/users/{userId}/favorites/{componentId}` | Удаление из избранного | Auth |

### 3.2 Форматы данных

#### Профиль пользователя
```json
{
  "userId": "uuid",
  "username": "string",
  "email": "string",
  "hashedPassword": "string",
  "savedBuilds": ["uuid"],
  "favoriteComponents": ["uuid"],
  "createdAt": "date",
  "updatedAt": "date"
}
```

### 3.3 Коды ответов
| Код | Описание |
|-----|-----------|
| 200 | Успешное выполнение |
| 201 | Ресурс создан |
| 400 | Некорректный запрос |
| 401 | Не авторизован |
| 403 | Доступ запрещен |
| 404 | Ресурс не найден |
| 409 | Конфликт |
| 500 | Внутренняя ошибка сервера |

## 4. База данных

### 4.1 Схема базы данных
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

### 4.2 Миграции
Миграции управляются через Sequelize CLI:
```bash
npm run migrate           # Применить миграции
npm run migrate:undo     # Откатить последнюю миграцию
```

## 5. Безопасность

### 5.1 Аутентификация
- Проверка JWT токенов от Authentication Service
- Валидация прав доступа к ресурсам

### 5.2 Защита данных
- Хеширование паролей (bcrypt)
- Валидация входных данных
- Защита от SQL-инъекций (через ORM)
- CORS настройки

## 6. Развертывание

### 6.1 Требования
- Node.js 14+
- PostgreSQL 12+
- Docker (опционально)

### 6.2 Переменные окружения
```env
PORT=3001
NODE_ENV=development
DB_HOST=localhost
DB_PORT=5432
DB_NAME=pcbuilder_users
DB_USER=postgres
DB_PASSWORD=your_password
JWT_PUBLIC_KEY=your_public_key
ALLOWED_ORIGINS=http://localhost:3000
```

### 6.3 Запуск
```bash
# Установка зависимостей
npm install

# Запуск в режиме разработки
npm run dev

# Запуск в production
npm start
```

## 7. Разработка

### 7.1 Установка окружения
```bash
git clone <repository-url>
cd user-profile-service
npm install
```

### 7.2 Соглашения по коду
- Airbnb JavaScript Style Guide
- Документирование функций и методов
- Типизация через JSDoc

## 8. Тестирование

### 8.1 Виды тестов
- Модульные тесты (unit tests)
- Интеграционные тесты
- E2E тесты

### 8.2 Запуск тестов
```bash
# Запуск всех тестов
npm test

# Запуск с покрытием
npm run test:coverage

# Запуск конкретного теста
npm test -- tests/user.test.js
``` 