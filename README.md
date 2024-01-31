# test_task

Практическая задачка для dev junior python.

### Структура файла README:
- [Заметка](#zametka)
- [Описание задания](#description_task)
- [Алгоритм работы интеграции](#algo_service)
- [Задачи проекта](#task_project)
- [Архитиктура данных проекта](#arch_data)
  - [Список данных, которые возвращает сайт](#list_data_site)
  - [Примеры входных/выходных данных](#list_data_input_output)

----
<a name="zametka"><h3>Заметка</h3></a>

Основная цель данного задания – понять уровень знаний и навыки кандидата.
Поэтому просьба в процессе реализации проекта не использовать всякие нейронные сети с названием GPT, copilot и прочие.

---

<a name="description_task"><h3>Описание задания:</h3></a>

Банк АО "Рога и Копыта" открыл новую услугу – брокерский счёт.
Нужно сделать интеграцию формы с сайта с внутренними ресурсами организации через **REST API**.

Ориентировачная нагруженность формы **~20 человек в час**.

Форма состоит из трех шагов:
Шаг 1 – Заполнение паспортных данных
Шаг 2 – Выбор типа счёта
Шаг 3 – Подписание договора

---

<a name="algo_service"><h3>Алгоритм работы интеграции:</h3></a>

Шаг 1: Сайт отправляет запрос с данными в сервис –≥ сервис проверяет данные, записывает их в базу, создаёт идентификатор пользователя и отдает его в ответе в формате JSON.
Шаг 2: Сайт отправляет запрос с данными в сервис –≥ сервис проверяет данные, записывает их в базу, генерирует документ (файл dogovor.docx), отдаёт сайту в формате PDF в формате JSON. Также сервис сохраняет обе версии документа в файловой структуре.
Шаг 3: Сайт отправляет запрос с данными в сервис –≥ сервис проверяет данные, записывает их в базу.

---

<a name="task_project"><h3>Задачи проекта:</h3></a>
1. Исходя из условий задания подобрать оптимальный стек технологий;
2. На основаниии своего опыта продумать алгоритм работы сервиса и собрать архитектуру;
3. Собрать боевой стенд в docker для последующего деплоя на сервер.

В качестве базы можно взять sqlite. 
>Таблицы в бд: 
>1. client (данные клиента)
>2. dogType(данные типа выбранного договора)
>3. sms(данные по смс)

##### Ограничения:
– Python 3.8+;
– Тип взаимодействия – синхронный;
– Тип протокола http для простоты разработки.

##### Основные задачи:
– Клонировать данный репозиторий. Создать ветку dev и вносить изменения в неё.
– Собрать и продумать архитектуру проекта и методов API;
– Реализовать каждый метод;
– Соблюдать основные правила PEP8;
– Соблюдение форматов ответов;
– Отлов ошибок и сбоев.

##### Будет плюсом:
– Учесть в коде все возможные "подводные камни" и ошибки пользователя;
– docstrings;
– Логирование;

---
<a name="arch_data"><h3>Архитектура данных</h3></a>

<a name="list_data_site"><h4>Список данных, которые возвращает сайт:</h4></a>
```
Шаг 1:
1. Фамилия
2. Имя
3. Отчество 
4. Дата рождения
5. Место рождения
6. Серия паспорта
7. Номер паспорта
8. Дата выдачи
9. Код подразделения
10. email
11. ИНН
12. Номер телефона
13. Текст СМС
14. Дата СМС
```
```
Шаг 2:
1. Тип счёта
2. Идентификатор клиента
3. Дата выбора
```
```
Шаг 3:
1. Дата заполнения
2. Текст СМС
3. Идентификатор клиента
```




<a name="list_data_input_output"><h4>Примеры входных/выходных данных:</h4></a>

Шаг 1:
```json
{
  "lastName": "Иванов",
  "firstName": "Алексей",
  "middleName": "Сергеевич",
  "dateOfBirth": "1984-06-10",
  "placeOfBirth": "г.москва",
  "passportSeries": "4512",
  "passportNumber": "123456",
  "dateOfIssue": "15.06.2005",
  "departmentCode": "250-351",
  "email": "alexey.ivanov@dooolo.com",
  "inn": "563565286576",
  "phoneNumber": "89123456789",
  "smsText": "Ваш код подтверждения: 1234",
  "smsDate": "2024-01-31Е09:46:21.832"
}
```

```json

{
    "status": "success", 
    "data": {
        "idClient": "Какой-то ID"
    }
}
```
```json
{
    "status": "error", 
    "data": {}
}

```
Шаг 2:

```json
{
  "accountType": "bs",
  "clientID": "Какой-то ID",
  "smsDate": "2024-01-31T12:34:11.999"
}

```


```json

{
    "status": "success", 
    "data": {
        "fileType": "pdf",
        "fileData": "Тут сам файл"
    }
}
```
```json
{
    "status": "error", 
    "data": {}
}

```

Шаг 3:

```json
{
  "date": "2024-01-31T18:34:12.736",
  "smsText": "Ваш код подтверждения: 4321",
  "clientID": "9876543210"
}
```


```json

{
    "status": "success", 
    "data": {}
}
```
```json
{
    "status": "error", 
    "data": {}
}

```

