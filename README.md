# Final Task(Golang version >= 1.23.0)

Этот сервис реализует управление кошельками с использованием REST API и gRPC.

## Микросервисы

1. Auth(gRPC) - реализует логику идентификации, аутентификации и авторизации.
2. Exchanger(gRPC) - реализует логику обмена валют.
3. Wallet(HTTP) - основное приложение, по gRPC запрашивает данные о пользователях и актуальных курсах валют.


## Описание
## Auth-service
### API

- **Auth/Register**

  Регистрация пользователя.

  Пример запроса:
  ```json
  {
      "email": "test@gmail.com",
      "username": "test",
      "password": "12345678"
  }
  ```

- **Auth/Login**

  Логин пользователя. В ответ возвращает токен.

  Пример запроса:
  ```json
  {
    "username": "baneb1ad3",
    "password": "123"
  }
  ```

## Exchanger-service
### API

- **ExchangeService/GetExchangeRates**
  
  Получение курса валют.

- **ExchangeService/GetExchangeRateForCurrency**

  Получение курса обмена из одной валюты в другую.

  Пример запроса:
  ```json
  {
    "from_currency": "EUR",
    "to_currency": "RUB"
  }
  ```

## Wallet-service
### API

- **POST /api/v1/auth/register**

  Регистрация.
  Пример запроса:
  ```json
  {
      "email": "test@gmail.com",
      "username": "test",
      "password": "12345678"
  }
  ```
  Пример ответа
  ```json
  {
    "message": "User registered successfully"
  }
    ```

- **POST /api/v1/auth/login**

  Логин.

  Пример запроса:
  ```json
  {
    "username": "bane",
    "password": "12345678"
  }
  ```
  Пример ответа:
  ```json
  {
	  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE3MzMzMjExMDAsImlkIjoiM2QxM2M4ZjYtYjU3NC00NGQ4LTk0MTUtMmQyYjQ5NGYwN2IwIiwidXNlcm5hbWUiOiJiYW5lIn0.GUG97znTDzLe6oQwW9T8ALOyRO0DBYHvQXWJ1rOYmO0"
  }
  ```
- **GET /api/v1/wallet/balance/**

  Получить баланс пользователя.
  
  Headers:
  
  Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE3MzMzMjExMDAsImlkIjoiM2QxM2M4ZjYtYjU3NC00NGQ4LTk0MTUtMmQyYjQ5NGYwN2IwIiwidXNlcm5hbWUiOiJiYW5lIn0.GUG97znTDzLe6oQwW9T8ALOyRO0DBYHvQXWJ1rOYmO0
  
    Пример запроса:
    ```json
    {
      "username": "bane",
      "password": "12345678"
    }
    ```
    Пример ответа:
  ```json
  {
    "balance": {
      "EUR": 0,
      "USD": 0,
      "RUB": 0
    }
  }
  ```

- **POST /api/v1/wallet/deposit/**

  Пополнить баланс.

  Headers:
  
  Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE3MzMzMjExMDAsImlkIjoiM2QxM2M4ZjYtYjU3NC00NGQ4LTk0MTUtMmQyYjQ5NGYwN2IwIiwidXNlcm5hbWUiOiJiYW5lIn0.GUG97znTDzLe6oQwW9T8ALOyRO0DBYHvQXWJ1rOYmO0

  Пример запроса:
    ```json
  {
      "amount": 100,
      "currency": "USD"
  }
    ```
  Пример ответа:
    ```json
  {
      "message": "Account topped up successfully",
      "new_balance": {
        "EUR": 0,
        "USD": 100,
        "RUB": 0
      }
  }
  ```

- **POST /api/v1/wallet/withdraw/**

  Снять деньги с баланса.

  Headers:
  
  Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE3MzMzMjExMDAsImlkIjoiM2QxM2M4ZjYtYjU3NC00NGQ4LTk0MTUtMmQyYjQ5NGYwN2IwIiwidXNlcm5hbWUiOiJiYW5lIn0.GUG97znTDzLe6oQwW9T8ALOyRO0DBYHvQXWJ1rOYmO0
  
  Пример запроса:
    ```json
  {
      "amount": 100,
      "currency": "USD"
  }
    ```
  Пример ответа:
    ```json
  {
      "message": "Withdrawal successful",
      "new_balance": {
        "EUR": 0,
        "USD": 150,
        "RUB": 0
      }
  }
  ```

- **GET /api/v1/exchange/rates**

  Получить курс валют.

  Headers:

  Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE3MzMzMjExMDAsImlkIjoiM2QxM2M4ZjYtYjU3NC00NGQ4LTk0MTUtMmQyYjQ5NGYwN2IwIiwidXNlcm5hbWUiOiJiYW5lIn0.GUG97znTDzLe6oQwW9T8ALOyRO0DBYHvQXWJ1rOYmO0

  Пример ответа:
    ```json
  {
      "rates": {
      "EUR": 1.5,
      "RUB": 0.8,
      "USD": 1
      }
  }
  ```
- **POST /api/v1/exchange**

  Перевести деньги из одной валюты в другую.

  Headers:

  Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE3MzMzMjExMDAsImlkIjoiM2QxM2M4ZjYtYjU3NC00NGQ4LTk0MTUtMmQyYjQ5NGYwN2IwIiwidXNlcm5hbWUiOiJiYW5lIn0.GUG97znTDzLe6oQwW9T8ALOyRO0DBYHvQXWJ1rOYmO0

  Пример запроса:
    ```json
  {
    "from_currency": "USD",
    "to_currency": "EUR",
    "amount": 150
  }
    ```
  Пример ответа:
    ```json
  {
    "message": "Exchange successful",
    "exchanged_amount": 100,
    "new_balance": {
      "EUR": 100,
      "USD": 0
    }
  }
  ```


Стек технологий

    Golang – основной ЯП.
    Goose - инструмент миграций.
    Postgresql – база данных для хранения информации о кошельках и операциях.
    Redis - кэш.
    gRPC - для общения микросервисов.
    Docker – для контейнеризации приложения и базы данных.
    Docker Compose – для автоматического поднятия всей системы (приложения и базы данных).

### Запуск

Клонируйте репозиторий:

```bash
 git clone --recurse-submodules git@github.com:baneb1ade/final-task.git
```
Перейдите в директорию:
```bash
cd final-task
```
Запустите deploy.sh
```bash
./deploy.sh
```
Скрипт поднимет 5 контейнеров: exchanger_service_container, auth_service_container, wallet_service_container, postgres_container, redis_container
и накатит миграции на БД

Приложение будет доступно по адресу

    http://localhost:{PORT}
