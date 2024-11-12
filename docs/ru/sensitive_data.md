# Конфиденциальные данные и настройки безопасности

## База данных PostgreSQL

### Основные настройки:
```env
DB_HOST=localhost
DB_PORT=5432
DB_NAME=pcbuilder_users
DB_USER=pcbuilder_admin
DB_PASSWORD=your_secure_password_here
```

### Настройки пула соединений:
```env
DB_POOL_MIN=5
DB_POOL_MAX=20
DB_POOL_IDLE=10000
DB_POOL_ACQUIRE=30000
```

## JWT Verification

### Публичный ключ для проверки JWT:
```env
JWT_PUBLIC_KEY=your_public_key_from_auth_service
```

### Настройки проверки JWT:
```env
JWT_ISSUER=auth-service
JWT_AUDIENCE=user-profile-service
```

## Безопасность API

### Rate Limiting:
```env
RATE_LIMIT_WINDOW=15 # минут
RATE_LIMIT_MAX_REQUESTS=100
```

### CORS настройки:
```env
ALLOWED_ORIGINS=http://localhost:3000,http://localhost:3002
ALLOWED_METHODS=GET,POST,PUT,DELETE
```

## Сервисные URL

### Внутренние сервисы:
```env
AUTH_SERVICE_URL=http://localhost:3000
CATALOG_SERVICE_URL=http://localhost:3002
BUILD_SERVICE_URL=http://localhost:3003
```

## Логирование

### Winston настройки:
```env
LOG_LEVEL=info
LOG_FILE_PATH=/var/log/pcbuilder/user-profile-service.log
```

## Полный пример .env файла:

```env
# Сервер
NODE_ENV=development
PORT=3001

# База данных
DB_HOST=localhost
DB_PORT=5432
DB_NAME=pcbuilder_users
DB_USER=pcbuilder_admin
DB_PASSWORD=your_secure_password_here
DB_POOL_MIN=5
DB_POOL_MAX=20
DB_POOL_IDLE=10000
DB_POOL_ACQUIRE=30000

# JWT
JWT_PUBLIC_KEY=your_public_key_from_auth_service
JWT_ISSUER=auth-service
JWT_AUDIENCE=user-profile-service

# Rate Limiting
RATE_LIMIT_WINDOW=15
RATE_LIMIT_MAX_REQUESTS=100

# CORS
ALLOWED_ORIGINS=http://localhost:3000,http://localhost:3002
ALLOWED_METHODS=GET,POST,PUT,DELETE

# Сервисные URL
AUTH_SERVICE_URL=http://localhost:3000
CATALOG_SERVICE_URL=http://localhost:3002
BUILD_SERVICE_URL=http://localhost:3003

# Логирование
LOG_LEVEL=info
LOG_FILE_PATH=/var/log/pcbuilder/user-profile-service.log
```

ВАЖНО: 
1. Этот файл содержит конфиденциальную информацию и НЕ должен быть добавлен в систему контроля версий.
2. Добавьте его в .gitignore!
3. Создайте .env.example с теми же ключами, но без реальных значений для документации.
4. Храните реальные значения в безопасном месте (например, в менеджере секретов).
5. В production используйте более сложные пароли и ключи.
6. Регулярно меняйте учетные данные.

## Рекомендации по безопасности:

1. Используйте сложные пароли для базы данных (минимум 16 символов).
2. Храните ключи JWT в защищенном месте.
3. Настройте брандмауэр для ограничения доступа к базе данных.
4. Регулярно обновляйте зависимости для устранения уязвимостей.
5. Настройте SSL/TLS для всех соединений.
6. Используйте разные учетные данные для разработки и production. 