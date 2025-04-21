# Практическое задание
Приложение работает с брокером сообщений RabbitMQ и состоит из 2 микросервисов:
- [test-consumer](https://github.com/rabbit-test-task/test-consumer) принимает сообщения от RabbitMq
- [test-producer](https://github.com/rabbit-test-task/test-producer) отправляет сообщения в очередь RabbitMq

# test-producer (Python)
- Python 3.11+
- aio_pika для работы с RabbitMQ
- Pydantic для моделей
- asyncio
- Docker
- CI/CD с Github Actions
  
test-producer отправляет 2 типа сообщений:
```
{"datetime_now": datetime}
```
где datetime - текущая дата и время (в utc)


```
{"id" : i , "value": j}
```
где i - автоинкремент (идентификатор сообщения), прибавляющий 1 к значению с каждым новым сообщением.
j – случайное целочисленное число.

### Переменные окружения:
```
# RabbitMQ
RABBITMQ_HOST=localhost
RABBITMQ_PORT=5672
RABBITMQ_USERNAME=guest
RABBITMQ_PASSWORD=guest
RABBITMQ_EXCHANGE=message-exchange
RABBITMQ_QUEUE=message-queue
RABBITMQ_ROUTING_KEY=message-routing-key

# Настройки отправителя
DATETIME_INTERVAL=10.0  # интервал в секундах для сообщений с датой
VALUE_INTERVAL=5.0      # интервал для сообщений с значением

# Логирование
LOG_LEVEL=INFO
```

### Направления дальнейшего развития/улучшения:
- Написание интеграционных тестов (для RabbitMQ)
- Механизм доставки сообщений и повторная отправка неудавшихся
- Добавление новых сообщений для отправки, реализация для них сервисов отправки
- Брать данные откуда то, а не генерировать (например с бд/чата и т.д)
  
# test-consumer (Java)
- Java 17, Spring Boot 3.4.4
- Spring AMQP для интеграции с RabbitMQ
- Spring Data JPA с H2 в качестве БД
- WebSockets для передачи сообщений
- Thymeleaf для рендеринга страниц
- Docker
- CI/CD с Github Actions

Принимает 2 типа сообщений:
```
{"datetime_now": datetime}
```
где datetime - текущая дата и время (в utc)


```
{"id" : i , "value": j}
```
где i - автоинкремент (идентификатор сообщения), прибавляющий 1 к значению с каждым новым сообщением.
j – случайное целочисленное число.

Распознает тип сообщений, сохраняет их в бд и отображает на странице.

### Переменные окружения
```
# RabbitMQ
RABBITMQ_HOST=localhost
RABBITMQ_PORT=5672
RABBITMQ_USERNAME=guest
RABBITMQ_PASSWORD=guest
RABBITMQ_QUEUE=message-queue
RABBITMQ_EXCHANGE=message-exchange
RABBITMQ_ROUTING_KEY=message-routing-key

# База данных
DB_USERNAME=sa
DB_PASSWORD=password
```

### Направления дальнейшего развития/улучшения:
- Заменить H2 (InMemory) БД на нормальную (Posgre/Mysql)
- Реализовать механизм повторной обработки элемента
- Добавить механизм отслеживания доставки сообщений
- Добавить мониторинг (Grafana/Prometheus)
- Развитие в целом панели (добавление авторизации, ролей, механизм обработки сообщений, а не просто отображение в панели администратора и т.д)
- Написание интеграционных тестов

# Развертывание
1. Клонировать репозиторий
```shell
git clone https://github.com/rabbit-test-task/entrypoint.git
```

2. Настроить переменные окружения в docker-compose.yml
```shell
cd entrypoint
nano docker-compos.yml
```

3. Запустить проект
```shell
docker compose up -d
```





