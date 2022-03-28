#### Сколько времени сообщения могут находиться в очереди сообщений Cloud Queues

В качестве срока хранения сообщения Cloud Queues можно указать значение от 60 секунд до 1,209,600 секунд (14 дней). Значение по умолчанию составляет 432000 секунд (5 дней). По достижении квоты на хранение сообщения удаляются автоматически.

#### Как настроить Cloud Queues для поддержки более продолжительного хранения сообщений

Чтобы настроить период хранения сообщений, укажите значение атрибута MessageRetentionPeriod с помощью консоли сервиса либо метода Distributiveness. Используйте этот атрибут для указания количества секунд, в течение которых сообщение будет храниться в Cloud Queues.

Можно использовать атрибут MessageRetentionPeriod для указания срока хранения сообщения от 60 секунд (1 минуты) до 1209600 секунд (14 дней).

#### Как настроить максимальный размер сообщений для Cloud Queus

Чтобы настроить максимальный размер сообщений, задайте значение атрибута MaximumMessageSize. Этот атрибут позволяет указать максимальный размер сообщения Cloud Queues в байтах. Установите значение этого атрибута между 1024 байтами (1КБ) и 262144 байтами (256КБ).

#### Какого типа данные можно включать в сообщение

Сообщения Cloud Queues могут содержать до 256 КБ текстовых данных, включая форматы XML, JSON и неформатированный текст. Допускается использование следующих символов Юникода:

#x9 | #xA | #xD | [от #x20 до #xD7FF] | [от #xE000 до #xFFFD] | [от #x10000 до #x10FFFF]

#### Максимально допустимый размер очереди сообщений Cloud Queues

Квота на количество находящихся на передаче сообщений составляет 120000 для стандартных очередей. Сообщения считаются находящимися на передаче в промежуток времени, когда они уже переданы из очереди принимающему компоненту, но еще не удалены из очереди.

#### Сколько очередей сообщений можно создать?

По умолчанию количество очередей сообщений, создаваемых пользователем = 500. Для увеличения количества необходимо обратиться в техническую поддержку.

#### Ограничения имени очереди сообщений Cloud Queues

Длина имени очереди ограничена 80 символами. Можно использовать буквенно-цифровые символы, дефис(-) и нижнее подчеркивание(_). Можно использовать буквенно-цифровые символы, дефис(-) и нижнее подчеркивание(_).

Имя очереди сообщений должно быть уникальным в пределах аккаунта VK CS. После удаления очереди сообщений ее имя можно использовать повторно.