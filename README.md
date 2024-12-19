## 1. Установка и запуск Postman

1.Убедитесь, что Postman установлен. Скачать его можно на официальном сайте: [https://www.postman.com/downloads/](https://www.postman.com/downloads/)
2. Запустите Postman.
3. Нажмите кнопку "New" (или "New request" в зависимости от версии Postman) в левом верхнем углу.


## 2. Создание нового запроса

1. В появившемся окне, выберите "Request".


## 3. Заполнение параметров запроса

1. В поле "Request URL" введите API URL: `https://demo.altcraft.com/api/v1.1/profiles/import`
2. Выберите метод HTTP: `POST`.
3. Перейдите на вкладку "Body".
4. Выберите "raw".
5. Убедитесь, что выбран формат `JSON`.
6. Заголовки (Headers): Добавьте заголовок `Content-Type` со значением `application/json`. Для этого:
    * Нажмите на вкладку "Headers".
    * В поле "Key" введите `Content-Type`.
    * В поле "Value" введите `application/json`.


## 4. Тело запроса (JSON)

Используйте следующий JSON для импорта профиля по email:

JSON


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
    "_bdate": "YOUR_BDATE_HERE" // Замените на корректную дату в формате ISO 8601 (например, "1980-05-10T00:00:00Z")
  }
}


Замените `YOUR_BDATE_HERE` на корректную дату рождения в формате ISO 8601 (например, `1980-05-10T00:00:00Z`).


## 5. Отправка запроса

Нажмите кнопку "Send". Проверьте код ответа (Status Code). `200 OK` или `201 Created` обычно означают успешный импорт.


