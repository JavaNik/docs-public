Где располагаются сервера
-------------------------

Виртуальные машины можно создать в дата-центрах VK CS, находящихся в Российской Федерации:

*   dp1 - дата-центр DataPro;
*   ms1 (ko1) - дата-центр DataLine NORD4;

Какая гарантированная скорость соединения
-----------------------------------------

VK CS обеспечивает виртуальные машины входящим и исходящим каналом связи с Интернет с пропускной способностью 1Гбит/с, без ограничения по трафику.

Не могу установить пароль при создании
--------------------------------------

Установить пароль на инстанс можно по истечении 15 минут после его создания. В этом время на виртуальной машине работает набор скриптов, позволяющих конфигурировать операционную систему в соответствии с выбранными настройками в мастере создания.

Нет доступа к высокопроизводительным CPU
----------------------------------------

Для получения доступа к высокопроизводительным процессорам необходимо [обратиться в техническую поддержку](mailto:support@mcs.mail.ru). 

После получения доступа, создать виртуальную машину с высокопроизводительным процессором можно в личном кабинете, отметив опцию "Высокопроизводительные CPU" в мастере создания инстанса.

Сколько стоит внешний IP-адрес
------------------------------

Плавающий IP-адрес (эквивалентный "серому") предоставляется бесплатно для каждой виртуальной машины.

Хочу создать инстанс, но нет подходящей конфигурации
----------------------------------------------------

Все конфигурации (флейворы) виртуальных машин доступны в личном кабинете в мастере создания инстанса. В случае, если необходимо получить индивидуальную конфигурацию, следует обратиться с запросом в техническую поддержку.

Не рекомендуется использовать конфигурации, в которых коэффициент соотношения CPU и RAM 1:1 или менее этого значения. Подобные конфигурации обладают узкими местами в производительности и могут использоваться для выполнения конкретных задач, например: для машинного обучения или распознавания объектов. 

Мой инстанс медленно работает
-----------------------------

В момент снижения скорости работы следует обратить внимание на операции, производимые операционной системой и ее процессами, а также установленным программным обеспечением.

Также не рекомендуется создавать инстансы с низким количеством CPU и объемом RAM для использования операционной системы семейства Windows, поскольку потребление ресурсов операционной системы может мешать нормальной работе с ВМ. 

Не создается инстанс
--------------------

Если в процессе создания инстанса возникла ошибка, следует обратить внимание на всплывающее окно в правом верхнем углу панели VK CS, в котором отображается сообщение об ошибке. В случае, когда сообщение не появляется, при этом мастер создания сообщает об ошибке, рекомендуем [обратиться в техническую поддержку](mailto:support@mcs.mail.ru).

Не хватает квот при создании ВМ
-------------------------------

Механизм создания ВМ требует наличия достаточного количества квот в проекте для успешного выполнения операции создания. В случае, когда квот не достаточно, следует [обратиться в техническую поддержку](mailto:support@mcs.mail.ru), сообщив данные проекта, учетной записи, а также объема ресурсов, необходимых для добавления в проект. 

Не могу создать ВМ с Windows 8 / 10
-----------------------------------

Клиентские операционные системы семейства Windows, такие как Windows 7 / 8 / 10 невозможно использовать в облаке VK CS. Это ограничение установлено для всех проектов и оно не может быть снято. 

На инстансе плохо обрабатываются графические элементы
-----------------------------------------------------

В системе виртуализации для обработки графики используются ресурсы CPU, которые не предназначены для обработки графических элементов, требующих наличие видеодрайвера. На, уже привычном, домашнем компьютере или ноутбуке для обработки графики используется выделенный чип - графический адаптер, который оптимизирован и специализируется на обработке графики, потому даже при аналогичной конфигурации домашнего компьютера и виртуальной машины, обработка графических элементов будет существенно отличаться.

Не могу удаленно подключиться к инстансу
----------------------------------------

Подключение к инстансу может быть доступно при выполнении нескольких условий:

*   наличие доступа в Fiewall
*   наличие ключей авторизации (ключевая пара для Linux и логин-пароль для Windows)

В первую очередь необходимо убедиться в корректном назначении групп безопасности на созданный инстанс, которые должны включать разрешение на доступ соответствующим портам и протоколам: RDP для Windows (TCP 3389), SSH для Linux (TCP 22).

Необходимо также убедиться что инстанс включен и у него есть назначенный IP адрес из сети ext-net. 

Потерял приватный ключ
----------------------

При утере приватного ключа доступа его необходимо сгенерировать заново и добавить на инстанс вручную. Для выполнения процедуры можно воспользоваться информацией в статье о [Восстановлении доступа к ВМ](https://mcs.mail.ru/help/ru_RU/vm-connect/recover-access-vm).

Не подключается Openstack CLI
-----------------------------

Доступ в Openstack CLI осуществляется с использованием файла конфигурации (Linux) или набором параметров из файла и пароля. Информация об установке, настройке и параметрах подключения можно получить в статье об Использовании [утилит управления (CLI)](https://mcs.mail.ru/help/ru_RU/create-vm/vm-create-cli).

Не отображается консоль инстанса в панели VK CS
---------------------------------------------

Веб консоль управления позволяет работать с виртуальной машиной без необходимости использования удаленного подключения к инстансу. Консоль доступна в карточке виртуальной машины на вкладке "Виртуальные машины" сервиса "Облачные вычисления".

Для корректного отображения окна консоли рекомендуется использовать браузер Chrome. В консоли не доступны привычные комбинации клавиш, передача звука и буфера обмена. 

Можно ли увеличить CPU или RAM
------------------------------

В некоторых случаях, когда производительности инстанса не достаточно, или, наоборот - запаса слишком много, можно произвести изменение конфигурации используя функцию "Изменить тип виртуальной машины" в контекстном меню инстанса сервиса "Облачные вычисления". Изменить конфигурацию можно как в большую, так и в меньшую сторону.

При изменении типа VK CS создает дополнительную виртуальную машину с запрошенной конфигурацией, затем переключает все ресурсы с текущего инстанса на новый. Для выполнения этой операции в проекте должно быть достаточно квот для создания виртуальной машины.