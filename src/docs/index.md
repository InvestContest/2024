## Соревнуемся в доходности торговых роботов

Мы проводим конкурс доходностей торговых роботов на базе Тинькофф Инвестиций. 
Участникам рисковать реальными деньгами не требуется - соревнование проходит на виртуальных счетах "песочницы", без вывода сделок на биржу.
Участникам необходимо разработать торгового бота, показавшего максимальную доходность за период конкурса и опубликовать его код в opensource. 
Победители получат 150, 120 и 90 тысяч рублей за первое, второе и третье место соответственно.

Мы ждем граждан РФ, разработчиков старше 18 лет уровня junior и выше, интересующихся алгоритмическим трейдингом. 
Ограничений по языкам программирования нет, но предпочтительнее писать на Java, Go, Python, Java Script и php 
Чтобы зарегистрироваться, оставьте заявку на странице конкурса и следуйте инструкциям.

## С чего начать 

1. Вы должны быть клиентом Тинькофф Инвестиций. В разделе [настройки](https://www.tinkoff.ru/invest/settings/) сгенерируйте новый токен для доступа к песочнице API.
2. Ознакомьтесь с [описанием API](https://RussianInvestments.github.io/investAPI/), если вы никогда не сталкивались с протоколом [gRPC](https://grpc.io/docs/), прочтите [документацию](https://RussianInvestments.github.io/investAPI/grpc/)
3. Посмотрите [примеры коннекторов на разных языках программирования](https://github.com/RussianInvestments/investAPI/) или можете сгенерировать коннекторы на любом языке программирования самостоятельно на основе [proto-контрактов](https://github.com/RussianInvestments/investAPI/tree/main/src/docs/contracts)
4. Ознакомьтесь с нашим [глоссарием](https://RussianInvestments.github.io/investAPI/glossary/) и помощью [Тинькофф Инвестиций](https://help.tinkoff.ru/investments/?)
5. Выберите (или придумайте) торговые гипотезы, которые хотите проверить. Потестируйте их на истории котировок.
6. Попробуйте реализовать работу торговой гипотезы на "[песочнице](https://RussianInvestments.github.io/investAPI/head-sandbox/)" - специальном сервисе-эмуляторе брокера, при котором ваши торговые поручения не выводятся на биржу и вы не несете рисков потери средств.
7. Если получилось интересное решение - присоединяйтесь к конкурсу. Подробнее про этапы проведения [тут](/2024/stages/).
8. **Важно:** Вы можете создать до 10 счетов в песочнице для тестирования стратегий. Отладку и настройку можно делать в любое время.
Счета, участвующие в конкурсе должны быть открыты не раньше 25.03.2024. Конкурсных счетов в "песочнице" должно быть не более двух. 
Названия счетов должны быть в следующем формате: "contest2024:login/repo:1", 
где login - ваш логин на гитхабе, repo - название репозитория, 1 - порядковый номер счета (1 или 2).



Желаем успеха!  

## Примеры торговых стратегий
Так как цель конкурса - в разработке примеров кода роботов, работающих через Tinkoff Invest, то стратегии могут быть любые по выбору участника. 
Выбранный тип стратегии не влияет на итоговую оценку работы. 
Примеры текстовых описаний стратегий можно посмотреть [здесь](https://github.com/InvestContest/2024/blob/main/examples.md)

## Каким должно быть решение

### Требования к работам участников
* описание торгового алгоритма в свободной форме; 
* реализация исполнения поручений (заявок на продажу/покупку ценных бумаг) в "песочнице";
* [желательно] ведение статистики работы алгоритма;
* [желательно] предварительная загрузка системой истории рыночных котировок и проведение бэктеста(тестирования стратегии на исторических данных) на уже загруженных данных;

### Загруженный на GitHub код

* Свободно скачивается и компилируется на операционных системах Mac, Windows и Linux-подобных;
* Не требует установленных дополнительных решений, кроме компилятора конкретного языка;
* Все используемые компоненты и библиотеки должны распространяться под лицензией apache 2 или аналогичных;
* Важно маркировать все выставляемые ордера с [appname](https://RussianInvestments.github.io/investAPI/grpc/#appname), соответствующему нику участника на Github.com или названию решения;
* К коду нужно приложить инструкцию по установке и запуску решения;


## Коммуникации

Если у вас есть любые вопросы по конкурсу - задавайте их:

* либо в Телеграмм чате https://t.me/tinkoff_invest_robot_contest
* либо в [Issue Github](https://github.com/InvestContest/2024/issues) 