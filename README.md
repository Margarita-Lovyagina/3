## Импорт профиля в Postman
Инструкция поможет вам импортировать данные профиля для работы с Altcraft Platform через Postman. 

## Установка и запуск Postman
1. Сначала убедитесь, что у вас установлен Postman. Вы можете скачать его на официальном сайте: https://www.postman.com/downloads/
Запустите Postman.
2. Создайте новый запрос. В Postman нажмите кнопку “Home” в левом верхнем углу.
 ![1](https://github.com/user-attachments/assets/acfcd14e-47b4-4134-9dc9-8d23aada030a)
Нажмите кнопку “New request” в открывшемся окне.
![2](https://github.com/user-attachments/assets/59be985d-23bf-4829-b3cf-e08609ef4e42)

 
4. Заполните параметры запроса
В поле “Request URL” введите API URL: https://demo.altcraft.com/api/v1.1/profiles/import
Выберите метод "POST" для вашего запроса.
 
Перейдите на вкладку “Body”, находящуюся под полем для URL, и выберите режим “raw”. Убедитесь, что выбран формат “JSON”


 
 


При использовании JSON в теле запроса необходимо использовать заголовок Content-Type: application/json.
Для этого перейдите в раздел “Headers”. В графе “Key” введите “Content-Type:”, а в графе “Value” – “application/json”. Затем нажмите “Save”.
 
4. Перейдем к написанию запроса. 
Для этого вернитесь на вкладку “Body”, “raw”. Предположим, вы хотите импортировать профиль клиента посредством его email-адреса. Тогда в поле matching следует указать email. Также известны:
•	token - c7f55f8f24204b9f91bfaaedda052e49,
•	db_id - 1, 
•	_fname – John,
•	_Iname – Doe
Составим запрос:
{
  "token": "c7f55f8f24204b9f91bfaaedda052e49",
  "db_id": 1,
  "matching": "email",
  "email": "john@example.com",  
  "skip_triggers": true,
  "skip_invalid_subscriptions": true,
  "detect_geo": true,
  "data": {
    "_fname": "John",
    "_lname": "Doe"
    "_bdate": "YOUR_BDATE_HERE"T21:00:00Z"
}
}
 
•	"token" — API-токен;
•	"db_id" — идентификатор базы данных;
•	"matching" и "email" — параметр для поиска профиля в базе. В данном случае для поиска профиля используется email-адрес. Если профиль с таким адресом уже есть в базе, то запрос обновит его данные, если нет — создаст новый профиль. Параметр "matching" может принимать различные значения. При этом он не является обязательным и, если его не указать, поиск профиля произойдёт по email;
•	"skip_triggers" - пропустить запуск триггеров
(по умолчанию – false);
•	"skip_invalid_subscriptions"- пропустить невалидные подписки
(по умолчанию – false);
•	"detect_geo" - включает автоопределение geo данных по полю _regip или _ip в data;
•	"data" — объект, содержащий данные профиля. 
•	"_fname", "_lname" – имя и фамилия клиента.
•	"_bdate" – дата рождения клиента в формате "1990-01-15T12:00:00Z".
5. Замените "YOUR_BDATE_HERE" на корректную дату рождения в формате ISO 8601 (YYYY-MM-DD). Для того, чтобы получить дату 20 лет назад, нужно использовать Pre-request Script в Postman. Перейдите на вкладку “Scripts”, "Pre-request Script" и вставьте следующий JavaScript код:
let today = new Date();
let pastDate = new Date();
pastDate.setFullYear(today.getFullYear() - 20);

let formattedDate = pastDate.toISOString().slice(0, 10);

pm.environment.set("pastDate", formattedDate);

 
Вернитесь во вкладку “Body”. После этого замените "YOUR_BDATE_HERE" на {{pastDate}} в JSON-запросе. Этот скрип вычисляет дату рождения 20 лет назад и сохраняет её в переменной pastDate. Postman использует двойные фигурные скобки для подстановки переменных.
 
6. Дополните запрос другими данными о профиле клиента.
Запрос будет выглядеть так (можно скопировать его и вставить в поле Body):
{
  "token": "c7f55f8f24204b9f91bfaaedda052e49",
  "db_id": 1,
  "matching": "email",
  "email": "john@example.com",  
  "skip_triggers": true,
  "skip_invalid_subscriptions": true,
  "detect_geo": true,
  "data": {
    "_fname": "John",
    "_lname": "Doe",
    "_bdate": "{{pastDate}}T21:00:00Z", 
    "_sex": 0,
    "_regdate": "2024-10-27T10:00:00Z",
    "_regip": "192.168.1.1",
    "_ip": "192.168.1.1",
    "_tz": "Europe/Moscow",
    "_postal_code": "12345",
    "_os": "Windows 10",
    "_browser": "Chrome",
    "_vendor": "form_#31",
    "phones": ["+79000000000"],
    "subscriptions": [
      {
        "channel": "email",
        "email": "john@example.com",
        "resource_id": 1,
        "custom_fields": {
          "_browser_name": "Chrome",
          "_device_type": "web"

        },
        "cats": [
          "category_1",
          "category_2"
        ]
      },
      {
        "channel": "sms",
        "phone": "+79000000000",
        "resource_id": 2
      },
      {
        "channel": "push",
        "subscription_id": "abcdefghijklmnqrstuvwxyz",
        "provider": "android-firebase",
        "resource_id": 2
      },
      {
        "channel": "telegram_bot",
        "cc_data": {} 
      },
      {
        "channel": "whatsapp",
        "cc_data": {
            "phone": "+79000000000"} 
      },
      {
        "channel": "viber",
        "cc_data": {
            "phone": "+79000000000"} 
      }
    ]

  }
}

Новые поля:
•	"_sex" — пол, "0" для мужчины, "1" для женщины;
•	"_city" — город;
•	"phones" — номер телефона; 
•	"subscriptions"– массив, который хранит данные о подписках профиля на ресурсы. Один объект — одна подписка;
•	"channel" — тип канала, например, "email", "sms", "push";
•	"resource_id" — идентификатор ресурса;
•	"cc_data" - id чата для Telegram-бота, телефон профиля для WhatsApp или Viber.
## Отправка запроса
Нажмите кнопку "Send" вверху справа.
 
## Проверка результата
Посмотрите на код ответа (Status Code) в нижней части окна. Код 200 OK или подобный обычно означает успешный импорт. 
Проверьте тело ответа “Body” в нижней части экрана на наличие сообщений об ошибках или дополнительной информации. Значение “profile_id” –  это ID импортированного профиля. Вы можете скопировать его значение и использовать для проверки правильности импорта.
 
Если есть ошибки, проверьте ответ сервера на сообщения об ошибках. Обратите внимание на синтаксис вашего запроса. Лишние символы или незакрытые скобки могут привести к ошибке.
Если что-то пошло не так, в ответном сообщении будет указан код ошибки и ее описание:
 
Если у вас не получилось самостоятельно устранить ошибку, передайте код и описание ошибки в службу поддержки Altcraft.
Проверка импорта (Необязательный шаг)
Создайте “New Request” так же как это было описано выше. Используйте метод Post (выберите слева в выпадающем меню). 
В поле “Request URL” введите API URL: https://demo.altcraft.com/api/v1.1/profiles/get
В поле “Body”, “raw” вставьте запрос:
{
    "token": "c7f55f8f24204b9f91bfaaedda052e49",
    "db_id": 1,
    "matching": "profile_id",
    "profile_id": "675f30d95ec2d7d61fd01e6a"
}

Вам нужно будет найти ID импортированного профиля в ответе на предыдущий POST запрос. Затем замените token, db_id и profile_id на фактический ID профиля.
 
Нажмите “Send”. В случае успешного импорта в поле “Body” появится код 200 OK и информация об импортируемом профиле, а также сообщение "error_text": "Successful operation"

 
 

##Возвращаемые параметры
Параметр	Тип	Описание
error	int	Код ошибки
error_text	string	Текст ошибки
profile_id	string	Идентификатор профиля



