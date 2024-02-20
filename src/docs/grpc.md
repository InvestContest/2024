#Протокол взаимодействия с TINKOFF INVEST API

Вся работа с TINKOFF INVEST API происходит по протоколу gRPC, документацию которого можно найти по ссылке:
[https://grpc.io/docs/](https://grpc.io/docs/). 

Одной из ключевых особенностей протокола следует назвать Bidirectional-stream — это особый режим работы, 
при котором открывается одно стрим-соединение, отправлять сообщения в которое могут оба участника 
взаимодействия. Это позволяет более гибко и оперативно реализовать работу. Например, 
bidirectional-stream [сервиса котировок](/investAPI/head-marketdata/) 
в одном и том же соединении принимает сообщения об изменении статуса подписки и предоставляет различные 
виды биржевой информации (стаканы, свечи, поток обезличенных сделок и т.д.). Подробнее о данном режиме 
работы можно ознакомиться в [документации протокола](https://grpc.io/docs/what-is-grpc/core-concepts/).

##Авторизация в TINKOFF INVEST API

Для успешной работы с TINKOFF INVEST API требуется передавать токен доступа в metadata
каждого запроса.

Формат заголовка: 
`Authorization: Bearer [токен доступа]`

Например:
`Authorization: Bearer t.QtEo8ahkNFX4RTpbqp0u4z4GDZq27HzUp6AotJASBx7_DVqmqZMHfM2Cy7JmUjS80boI9eVg`

<a name="tracking"></a>

##Адреса сервиса TINKOFF INVEST API

Все вызовы продового сервиса выполняются по адресу **`invest-public-api.tinkoff.ru:443`**.

Все вызовы сервиса песочницы выполняются по адресу **`sandbox-invest-public-api.tinkoff.ru:443`**.

Различия работы контуров описаны на [странице](/investAPI/url_difference/).

##tracking-id запросов

Во все ответы unary-методов TINKOFF INVEST API добавляется заголовок **x-tracking-id**. 
Это уникальный uuid-идентификатор запроса, который необходим технической поддержке для разбора различных 
инцидентов. При обращении в службу технической поддержки обязательно указывайте **tracking-id** запроса, 
это ускорит решение вашего вопроса. 

*Для сообщений управления статусом подписки в рамках stream-соединений **tracking-id** передаётся явно в
виде выходного параметра.*

##Полный текст ошибки

В случае ошибки при исполнении методов в TINKOFF INVEST API в ответ добавляется заголовок **message**.
В данном заголовке приходит полный текст описания ошибки. 

В Kreya вы можете посмотреть текст ошибки на вкладке **Header**, в поле **message**.


##Appname запросов

TINKOFF INVEST API позволяет в запросах добавлять служебный заголовок **x-app-name**, который 
нужен для сбора статистики по используемым инструментам. Если вы разработчик SDK, рекомендуется
использовать app-name формата *<Ник в GitHub>.<Название репозитория>*. Например, **user.tinkoffSDK**.

##Рекомендации по тестированию методов TINKOFF INVEST API

Для тестирования работы TINKOFF INVEST API можно использовать любой доступный grpc-клиент.
Ниже приведены краткие инструкции по настройке двух наиболее популярных из них, а также ссылка на гайд по настройке встроенного gRPS клиента IntelliJ от энтузиаста.

###Kreya

Данный бесплатный gRPC-клиент предоставляет довольно широкий набор функциональности и возможностей.
Скачать его можно с официального сайта [kreya.app](https://kreya.app/).

1. После установки и запуска приложения перейдите в меню Project — Importers.
![Интерфейс Kreya](/investAPI/img/Kreya-1.png "Интерфейс Kreya")
2. Нажмите *New importer*. Укажите название источника данных и его тип (*gRPC proto files*).
![Интерфейс Kreya](/investAPI/img/Kreya-2.png "Интерфейс Kreya")
3. Нажмите *Add proto directory* и укажите папку со скаченными proto-контрактами сервиса TINKOFF INVEST API. Вы также можете нажать *Add proto files* и выбрать ручным способом все proto-файлы из папки.
Скачать актуальную версию контрактов можно по ссылке на официальном GitHub: [TINKOFF INVEST API](https://github.com/RussianInvestments/investAPI/tree/main/src/docs/contracts).

**Важно!!** TINKOFF INVEST API использует `google.api.field_behavior` нотацию применительно к полям контрактов.
Необходимо дополнительно загрузить [field_behavior.proto](https://github.com/googleapis/googleapis/blob/master/google/api/field_behavior.proto) и сохранить
его в папке `<contracts_dir>\google\api`, где `<contracts_dir>` - папка с контрактами TINKOFF INVEST API.
![Интерфейс Kreya](/investAPI/img/Kreya-3.png "Интерфейс Kreya")
4. Сохраните изменения и нажмите *Back*.
5. В левом окне нажмите на появившуюся папку *Tinkoff*. Укажите Endpoint сервиса: https://invest-public-api.tinkoff.ru:443.
Остальные настройки укажите в соответствии со скриншотом. Обратите внимание на блок *Metadata*, 
в нём требуется указать заголовок Authorization, в значении которого передаётся строка **"Bearer <токен доступа>"**.
Подробнее о том, как получить токен можно узнать по ссылке: [Токен доступа](/investAPI/token/)
![Интерфейс Kreya](/investAPI/img/Kreya-4.png "Интерфейс Kreya")
6. Теперь вы можете выполнять запросы к сервису.
![Интерфейс Kreya](/investAPI/img/Kreya-5.png "Интерфейс Kreya")

Kreya позволяет увидеть служебные заголовки ответа сервера:
![Интерфейс Kreya](/investAPI/img/Kreya-6.png "Интерфейс Kreya")

* **x-ratelimit-limit** — показывает текущий лимит пользователя по данному методу.

* **x-ratelimit-remaining** — количество оставшихся запросов данного типа в минуту.

* **x-ratelimit-reset** — время в секундах до обнуления счётчика запросов. 

####Обновление proto-контрактов
Для получения доступа к новому функионалу, который появился после обновления версии, необходимо актуализировать proto-контракты. 
Для этого необходимо заново скачать [контракты](https://github.com/RussianInvestments/investAPI/tree/main/src/docs/contracts), 
заменить скаченные файлы в папке проекта и нажать "Run all importers" в левом верхнем углу приложения. 
Необходимо убедиться, что все proto-контракты имеют актуальную версию.
**Не храните бекапы и копии контрактов в одной папке с актуальными!**
![Обновления контрактов в Kreya](/investAPI/img/Kreya-update_contracts.png "Обновления контрактов в Kreya")

**Важно!!** Если вы получили ошибку "The referenced gRPC method is not imported.", убедитесь, что вы очистили проект в Kreya от прошлых загруженных контрактов. 
Также всегда проверяйте путь до папки с загружаемыми контрактами, полное наличие всех proto-файлов и их соответствие актуальной версии на официальном GitHub: [TINKOFF INVEST API](https://github.com/RussianInvestments/investAPI/tree/main/src/docs/contracts). 
Изменение proto-файлов повлечет за собой ошибку их прочтения у gRPS-клиентов.

###BloomRPC

Скачать данный клиент можно по ссылке: [bloomrpc releases](https://github.com/uw-labs/bloomrpc/releases).

Интерфейс утилиты достаточно прост, для старта работы требуется указать домен сервиса и импортировать
все proto-файлы. 

1. Импортируйте скаченные proto-контракты сервиса. Скачать актуальную версию контрактов можно по ссылке на официальном GitHub: [TINKOFF INVEST API](https://github.com/RussianInvestments/investAPI/tree/main/src/docs/contracts).
![Интерфейс BloomRPC](/investAPI/img/Bloom-1.png "Интерфейс BloomRPC")
2. Укажите адрес сервера invest-public-api.tinkoff.ru:443 и заполните поле metadata значением (подставьте своё значение [токена доступа](/investAPI/token/)).

`{
   "Authorization": "Bearer t.QtEo8ahkNFX4RTpbqp0u4z4GDZq27HzUp6AotJASBx7_DVqmqZMHfM2Cy7JmUjS80boI9eVg"
}`
![Интерфейс BloomRPC](/investAPI/img/Bloom-2.png "Интерфейс BloomRPC")
3. При выполнении запросов следует обязательно указывать тип TLS-подключения:
![Интерфейс BloomRPC](/investAPI/img/Bloom-3.png "Интерфейс BloomRPC")
4. Теперь вы можете выполнять запросы к сервису.
![Интерфейс BloomRPC](/investAPI/img/Bloom-4.png "Интерфейс BloomRPC")

BloomRPC не позволяет увидеть заголовки ответа сервера, поэтому команда TINKOFF INVEST API 
рекомендует использовать Kreya.

### Встроенный GRPS клиент IntelliJ

[Использование встроенного в продукты Intellij gRPC-клиента](https://github.com/tinkoff/investAPI/issues/4)