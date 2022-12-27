## DNS как сервис

DNS — это иерархическая распределенная база данных, которая позволяет хранить IP-адреса и другие данные, а также просматривать информацию по DNS именам.

VK Cloud позволяет публиковать ваши зоны и записи в DNS без необходимости разворачивания ваших собственных DNS-серверов.

VK Cloud предлагает:

- **Публичный DNS** — позволяет управлять DNS-зонами, которые видны в интернете.
- **Приватный DNS** —  существует внутри приватной сети, созданные здесь зоны не видны из интернета. Подробнее о приватном DNS читайте [здесь](https://mcs.mail.ru/help/ru_RU/networks/private-dns).

## Создание зоны

Зона DNS — это логическое объединение доменных имен ваших ресурсов, содержащее их ресурсные записи.

Чтобы добавить зону, нажмите “Виртуальные сети” -> “DNS”  -> “Добавить зону”. На странице отобразится экран добавления зоны DNS.

В поля ввода введите данные:

- DNS-зона — имя создаваемой зоны, например, домен, который был ранее вами приобретен;
- Контактный email — почтовый адрес администратора зоны;
- TTE — время (в секундах), после которого вторичный DNS перестает отвечать на запросы для данной зоны, если первичный сервер не отвечает. Значение должно быть больше, чем сумма TTRefresh и TTRetry.
- TTRefresh — время (в секундах), по истечению которого вторичный DNS сервер должен обновлять информацию о зоне с первичного DNS сервера.
- TTRetry — если первичный сервер недоступен, то по истечении времени (в секундах), указанного в данном параметре, сервер попытается синхронизировать информацию повторно.
- TTL — время жизни кэша для негативного ответа на запрос в зоне.

Нажмите “Добавить зону”. После обратитесь к регистратору, у которого был куплен домен, чтобы делегировать управление зоной на DNS сервера VK Cloud (ns1.mcs.mail.ru, ns2.mcs.mail.ru). У большинства провайдеров, вы можете самостоятельно делегировать управление зоной. Если у вас возникли вопросы, как это сделать, обратитесь за помощью к регистратору.

## Создание подзоны

Подзона — это зона DNS, которая находится уровнем ниже чем текущая. Например, для домена example.com (для которого вы создали зону в DNS сервисе VK Cloud) подзоной будет subzone.example.com.

Подзона может быть создана:

1.  В текущем проекте для отделения ресурсных записей подзоны от записей основной зоны.
2.  В другом проекте VK Cloud.
3.  У стороннего DNS провайдера.

Если вы хотите создать подзону в текущем проекте или другом проекте VK Cloud, вам необходимо создать 2 NS записи с именем подзоны, повторно делегирующие подзону на DNS сервера VK Cloud ([ns1.mcs.mail.ru](http://ns1.mcs.mail.ru), [ns2.mcs.mail.ru](http://ns2.mcs.mail.ru)). Процесс создания NS записей описан ниже.

Если вы хотите создать подзону у стороннего провайдера, то созданные NS записи должны будут указывать на DNS сервера стороннего провайдера.

После создания NS записей вы можете создать зону для делегированного поддомена. О том, как создать зону в проекте VK Cloud описано выше.

## Добавление ресурсных записей

Ресурсная запись — DNS-запись домена в системе доменных имен. С их помощью вы определяете, куда направлять запросы, которые поступают на доменные имена, а также предоставляете дополнительную информацию о домене.

VK Cloud поддерживает типы ресурсных записей, описанные ниже.

### A

A — запись DNS, которая сопоставляет доменное имя с адресом IPv4.

При добавлении записи типа А необходимо указать:

- Имя — имя записи. Возможны следующие значения:

  • @, example.com или пустое значение — указывает на саму зону;

  • subzone или subzone.example.com — указывает на подзону subzone;

  • \*.dns.zone — Wildcard символ. Указывает допустимость любого имени в запросе к домену.

- TTL — время жизни кеша в секундах.
- IP-адрес (IPv4).

### AAAA

AAAA — аналогично записи типа А для IPv6.

При добавлении записи типа AAAA необходимо указать:

- Имя — имя записи. Возможны следующие значения:

  • @, example.com или пустое значение — указывает на саму зону;

  • subzone или subzone.example.com — указывает на подзону subzone;

  • \*.dns.zone — Wildcard символ. Указывает допустимость любого имени в запросе к домену.

- TTL — время жизни кеша в секундах.
- IP-адрес (IPv6).

### NS

NS — запись DNS, которая содержит адрес сервера имен, обслуживающего данную зону или подзону. Две записи NS будут установлены в зоне по умолчанию, эти записи устанавливаются на стороне регистратора доменных имен для передачи прав управления доменом серверу имен VK Cloud.

При добавлении записи типа NS необходимо указать:

- Имя — имя записи. Возможны следующие значения:

  • @, example.com или пустое значение — указывает на саму зону;

  • subzone или subzone.example.com — указывает на подзону subzone.

- TTL — время жизни кеша в секундах.
- Значение — адрес NS сервера, например, [ns1.mcs.mail.ru](http://ns1.mcs.mail.ru) или [ns2.mcs.mail.ru](http://ns2.mcs.mail.ru).

### CNAME

CNAME — запись DNS, привязывающая псевдоним к доменному имени. Обычно используется для привязки поддомена (например, www) к домену, в котором размещен контент этого поддомена.

При добавлении записи типа CNAME необходимо указать:

- Имя — имя записи. Возможны следующие значения:

  • @, example.com или пустое значение — указывает на саму зону,

  • subzone или subzone.example.com — указывает на подзону subzone;

  • \*.dns.zone — Wildcard символ. Указывает допустимость любого имени в запросе к домену.

- TTL — время жизни кеша в секундах.
- Значение — FQDN адрес назначения.

### MX

MX — запись DNS, сообщающая адрес сервера обрабатывающего электронную почту.

При добавлении записи типа MX необходимо указать:

- Имя — имя записи. Возможны следующие значения:

  • @, example.com или пустое значение — указывает на саму зону,

  • subzone или subzone.example.com — указывает на подзону subzone.

- Приоритет — приоритет хоста. Чем ниже значение, тем более предпочтительный хост.
- TTL — время жизни кеша в секундах.
- Значение — FQDN адрес почтового сервера.

### SRV

SRV — запись DNS, определяющая местоположение (имя хоста и порт сервера) определенных сетевых служб.

При добавлении записи типа SRV необходимо указать:

- Имя — имя записи. Возможны следующие значения:

  • @, example.com или пустое значение — указывает на саму зону,

  • subzone или subzone.example.com — указывает на подзону subzone;

  • \*.dns.zone — Wildcard символ. Указывает допустимость любого имени в запросе к домену.

- Сервис — символьное имя сервиса (например, \_sip).
- Протокол — символьное имя протокола (например, \_tcp или \_udp).
- Приоритет — приоритет хоста. Чем ниже значение, тем более предпочтительный хост.
- Вес — вес для хостов с одинаковым приоритетом. Чем это значение ближе к 0, тем меньше шансов, что хост будет выбран.
- Порт — порт, который использует служба.
- TTL — время жизни кеша в секундах.
- Хост — FQDN хоста, на котором размещается служба.

### TXT

TXT — запись DNS, которая содержит текстовую информацию для источников за пределами домена.

При добавлении записи типа TXT необходимо указать:

- Имя — имя записи. Возможны следующие значения:

  • @, example.com или пустое значение — указывает на саму зону,

  • subzone или subzone.example.com — указывает на подзону subzone;

  • \*.dns.zone — Wildcard символ. Указывает допустимость любого имени в запросе к домену.

- TTL — время жизни кеша в секундах.
- Значение — текстовое значение.

## Ролевая модель

Для работы с DNS пользователям нужны роли.

<table style="width: 100%;"><tbody><tr><td style="width: 50%; background-color: rgb(209, 213, 216);"><div style="text-align: center;"><span style="font-family: Arial, Helvetica, sans-serif; font-size: 14px; color: rgb(0, 0, 0);">Действие</span></div></td><td style="width: 50%; text-align: center; background-color: rgb(209, 213, 216);"><div style="text-align: center;"><span style="font-family: Arial, Helvetica, sans-serif; font-size: 14px; color: rgb(0, 0, 0);">Роль</span></div></td></tr><tr><td style="width: 50%; text-align: center;"><p id="isPasted" style="text-align: left;"><span style="font-family: Arial, Helvetica, sans-serif; font-size: 14px; color: rgb(0, 0, 0);">Просмотр DNS зон и ресурсных записей</span></p></td><td style="width: 50.0000%;"><p id="isPasted" style="text-align: left;"><span style="font-family: Arial, Helvetica, sans-serif; font-size: 14px; color: rgb(0, 0, 0);">Наблюдатель</span></p></td></tr><tr><td style="width: 50.0000%;"><p id="isPasted"><span style="font-family: Arial, Helvetica, sans-serif; font-size: 14px; color: rgb(0, 0, 0);">Редактирование DNS зон и ресурсных записей</span></p></td><td style="width: 50.0000%;"><span style="font-family: Arial, Helvetica, sans-serif; font-size: 14px; color: rgb(0, 0, 0);">Владелец проекта, Владелец проекта, Администратор сети, Суперадминистратор</span></td></tr></tbody></table>

## Квоты и лимиты

Сервис не имеет квот, но существуют лимиты:

- максимальное количество DNS зон внутри проекта — 100;
- максимальное количество ресурсных записей для зоны DNS — 500.