# OTPService

Сервис для отправки и верификации одноразовых паролей (OTP) через различные каналы связи.

## Описание

OTPService - это Spring Boot приложение, которое предоставляет API для:
- Генерации и отправки OTP через SMS (SMPP), Telegram и Email
- Верификации OTP
- Управления пользователями и их правами доступа

## Технологии

- Java 17
- Spring Boot 3.4.4
- Spring Security
- Spring Data JPA
- PostgreSQL
- SMPP (OpenSMPP)
- Telegram Bot API
- JWT для аутентификации
- Swagger/OpenAPI для документации API

## Требования

- Java 17 или выше
- PostgreSQL
- Локальный SMPP сервер
- Docker и Docker Compose (опционально)

## Установка и запуск

1. Клонируйте репозиторий:
```bash
git clone https://github.com/your-username/OTPService.git
cd OTPService
```

2. Запустите базу данных через Docker Compose:
```bash
docker-compose up -d postgres
```

3. Настройте Telegram бота:
- Перейдите по ссылке: [@otp_fladx_bot](https://t.me/otp_fladx_bot)
- Напишите команду `/start` боту, чтобы активировать его
- Получите chatId из ответа бота

4. Соберите и запустите проект:
```bash
# Сборка проекта
./gradlew build

# Запуск приложения
./gradlew bootRun
```

## Тестирование API

### 1. Регистрация пользователя
```bash
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "username": "testuser",
    "password": "password123",
    "email": "test@example.com",
    "phone": "+79991234567"
  }'
```

### 2. Вход в систему
```bash
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "testuser",
    "password": "password123"
  }'
```

### 3. Отправка OTP через SMS
```bash
curl -X POST http://localhost:8080/api/otp/send \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{
    "phone": "+79991234567",
    "channel": "SMS"
  }'
```

### 4. Отправка OTP через Telegram
```bash
curl -X POST http://localhost:8080/api/otp/send \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{
    "telegramChatId": "YOUR_CHAT_ID",
    "channel": "TELEGRAM"
  }'
```

### 5. Отправка OTP через Email
```bash
curl -X POST http://localhost:8080/api/otp/send \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{
    "email": "test@example.com",
    "channel": "EMAIL"
  }'
```

### 6. Верификация OTP
```bash
curl -X POST http://localhost:8080/api/otp/verify \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{
    "code": "123456",
    "identifier": "test@example.com"
  }'
```

## Конфигурация

### База данных
База данных PostgreSQL запускается автоматически через Docker Compose. Настройки по умолчанию:
- Порт: 5432
- Пользователь: postgres
- Пароль: postgres
- База данных: otpservice

### SMPP сервер
- Локальный SMPP сервер должен быть запущен
- Настройки подключения в `application.yaml`:
```yaml
smpp:
  host: localhost
  port: 2775
  systemId: your_system_id
  password: your_password
```

### Почтовый сервер
- Локальный почтовый сервер запускается автоматически
- Настройки по умолчанию:
  - SMTP порт: 1025
  - POP3 порт: 1100
  - Web интерфейс: http://localhost:8081

### Telegram бот
- Бот доступен по ссылке: [@otp_fladx_bot](https://t.me/otp_fladx_bot)
- После запуска приложения необходимо:
  1. Найти бота в Telegram
  2. Отправить команду `/start`
  3. Сохранить полученный chatId для отправки OTP

## Документация API

После запуска приложения, документация API доступна по адресу:
- Swagger UI: `http://localhost:8080/swagger-ui.html`
- OpenAPI спецификация: `http://localhost:8080/v3/api-docs`

## Безопасность

- Все пароли хранятся в зашифрованном виде
- Используется JWT для аутентификации
- Реализована защита от брутфорса
- Все API endpoints защищены с помощью Spring Security

## Лицензия

MIT License 