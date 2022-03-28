ModX – это система управления контентом с открытым кодом. ModX предназначена для создания, редактирования и управления содержимым сайтов.

Требования
----------

*   Операционная система Ubuntu версии 18.04.
*   Пользователь с доступом к команде sudo.
*   Установленный стек LAMP. 

Если у вас еще не установлен стек LAMP:

*   Вы можете получить готовый стек LAMP в облаке [в виде настроенной виртуальной машины](https://mcs.mail.ru/app/services/marketplace/) на Ubuntu 18.04. При регистрации вы получаете бесплатный бонусный счет, которого хватает, чтобы тестировать сервер несколько дней.  
*   Вы можете установить стек LAMP самостоятельно. О том, как установить стек LAMP на Ubuntu 18.04, [читайте тут](https://mcs.mail.ru/help/lamp-setup/lamp-ubuntu-18).

Подготовка к установке ModX
---------------------------

Перед установкой ModX:

1.  Перейдите на сайт [https://modx.com/download](https://modx.com/download) и запомните номер версии ModX:
    
    ![](./assets/1554800077324-1554800077324.png)

2.  Откройте окно терминала.
    
3.  Установите архиватор ZIP, выполнив команду: 
    

```
sudo apt install unzip -y
```

4.  Установите дополнительные пакеты PHP, выполнив команду: 
    

```
sudo apt-get install php-common php-mbstring php-xmlrpc php-ldap php-sqlite3 -y
```

5.  Перейдите в домашний каталог, выполнив команду: 
    

```
cd ~
```

6.  Создайте временный каталог tempMX, выполнив команду:
    

```
mkdir tempMX 
```

7.  Перейдите в каталог tempMX, выполнив команду: 
    

```
cd ~/tempMX
```

8.  Скачайте архив ModX, выполнив команду:
    

```
wget [https://modx.s3.amazonaws.com/releases/<версия>/modx-<версия>.zip](https://modx.s3.amazonaws.com/releases/<Ð²ÐµÑÑÐ¸Ñ>/modx-<Ð²ÐµÑÑÐ¸Ñ>-pl.zip)
Например: wget [https://modx.s3.amazonaws.com/releases/2.7.1/modx-2.7.1-p1.zip](https://modx.s3.amazonaws.com/releases/<Ð²ÐµÑÑÐ¸Ñ>/modx-<Ð²ÐµÑÑÐ¸Ñ>-pl.zip)
```

9.  Распакуйте архив ModX, выполнив команду: 
    

```
sudo unzip modx-<версия>.zip
Например: sudo unzip modx-2.7.1-pl.zip
```

10.  Переместите файлы из текущего каталога в каталог /var/www/html/modx, выполнив команду:
    

```
sudo cp -r modx-<версия> /var/www/html/modx
Например: sudo cp -r modx-2.7.1-pl /var/www/html/modx
```

11.  Удалите временный каталог tempMX, выполнив команду: 
    

```
sudo rm -Rf ~/tempMX
```

12.  Замените владельца каталогов и файлов в корневом каталоге веб-сервера, используя команду: 
    

```
sudo chown -R имя_пользователя:www-data /var/www/html/modx
где имя_пользователя - это имя пользователя sudo, www-data - имя группы
Например: sudo chown -R www-data:www-data /var/www/html/modx
```

**Внимание**

Во избежание ошибок веб-сервера Apache при запуске скриптов используйте имя пользователя www-data и имя группы www-data по умолчанию.

13.  Если необходимо предоставить доступ к файлам корневого каталога веб-сервера другому пользователю, включите этого пользователя в группу www-data, используя команду:

```
sudo usermod -a -G www-data имя_пользователя
Например: sudo usermod -a -G www-data mxuser
```

14.  Настройте права доступа к файлам и папкам корневого каталога, используя команду: 
    

```
sudo chmod -R 775 /var/www/html/modx
```

15.  Создайте конфигурационный файл modx.conf, выполнив команду: 
    

```
sudo nano /etc/apache2/sites-available/modx.conf
```

В этот файл добавьте следующие строки:

```
<VirtualHost \*:80>
DocumentRoot /var/www/html/modx
ServerName <внешний IP-адрес вашего веб-сервера>
<Directory /var/www/html/modx/>
Options +FollowSymlinks
AllowOverride All
Require all granted
</Directory>
ErrorLog ${APACHE_LOG_DIR}/modx_error.log
CustomLog ${APACHE_LOG_DIR}/modx_access.log combined
</VirtualHost>
```

Сохраните изменения, используя сочетание клавиш CTRL+O, и завершите редактирование, используя сочетание клавиш CTRL+X.

16.  Отключите сайт по умолчанию 000-default.conf, выполнив команду:
    

```
sudo a2dissite 000-default.conf
```

17.  Подключите новый виртуальный хост, выполнив команду: 
    

```
sudo a2ensite modx.conf
```

18.  Подключите модуль Apache rewrite, выполнив команду: 
    

```
sudo a2enmod rewrite
```

19.  Перезагрузите веб-сервер Apache, выполнив команду:
    

```
sudo systemctl reload apache2
```

Настройка СУБД MySQL
--------------------

Чтобы начать работу с ModX, необходимо создать и настроить выделенную базу данных MySQL. Для этого: 

1.  Откройте окно терминала.
    
2.  Для перехода в оболочку MySQL выполните команду: 
    

```
sudo mysql -u root -p 
```

Используйте аутентификацию учетной записи root, относящуюся исключительно к СУБД MySQL.

3.  Создайте новую базу данных для ModX, используя команду: 
    

```
CREATE DATABASE имя_базы;
Например: CREATE DATABASE modxdb;
```

**Внимание**

После всех команд СУБД MySQL должна ставиться точка с запятой.

4.  Создайте пользователя с правами полного доступа к созданной базе данных и назначьте ему пароль, используя команду:
    

```
CREATE USER имя_пользователя@localhost IDENTIFIED BY 'пароль';
Например: CREATE USER mxuser@localhost IDENTIFIED BY 'mypassword';
```

5.  Предоставьте пользователю привилегии, необходимые для создания и изменения таблиц базы данных, выполнив команду:  
    

```
GRANT ALL PRIVILEGES ON  имя_базы.\* TO имя_пользователя@localhost;
Например: GRANT ALL PRIVILEGES ON modxdb.\* TO mxuser@localhost;
```

6.  Актуализируйте предоставление привилегий к таблицам базы данных, выполнив команду:
    

```
FLUSH PRIVILEGES;
```

7.  Выйдите из оболочки MySQL, выполнив команду: 
    

```
exit
```

Установка ModX
--------------

Для установки ModX в адресной строке веб-браузера введите:

```
http://<внешний IP-адрес вашего веб-сервера>/setup
```

В результате будет запущен мастер установки ModX, следуйте его указаниям:

1.  Выберите язык установки:

![](./assets/1554806010157-1554806010157.png)

Рекомендуется выбрать английский язык - **en**.

2.  Нажмите кнопку **Next**:

![](./assets/1554806296501-1554806296501.png)

3.  Выберите параметры установки и нажмите кнопку **Next**:

![](./assets/1554805874285-1554805874285.png)

4.  Выполните конфигурацию базы данных:

![](./assets/1554806322914-1554806322914.png)

Используйте имя пользователя базы данных, пароль и имя базы данных, которые вы указали при настройке БД MySQL . Другим параметрам рекомендуется оставить значения по умолчанию.

5.  Проверьте параметры подключения к БД MySQL. При успешной проверке отобразится примерно следующая строка:

```
Connecting to database server: Success!
```

6.  Выберите кодировку подключения:

![](./assets/1554806432781-1554806432781.png)

Рекомендуется использовать параметры, приведенные в примере. При успешном создании или выборке из БД отобразится строка:

```
Database check: Success!
```

7.  Укажите данные для создания учетной записи администратора ModX и нажмите кнопку **Next**:

![](./assets/1554806539101-1554806539101.png)

8.  Убедитесь, что все параметры проверки имеют статус **OK**, и нажмите кнопку **Install**:
    
    ![](./assets/1554806612165-1554806612165.png)
9.  Если установка ModX прошла успешно, откроется страница с отчетом об установке. Просмотрите сообщения или предупреждения, возникшие в процессе установки. Для завершения установки нажмите кнопку **Next**:

![](./assets/1554806657027-1554806657027.png)

9.  Чтобы выполнить аутентификацию и начать работу, нажмите кнопку **Login**:

**![](./assets/1554808358007-1554808358007.png)**

10.  Введите имя пользователя и пароль, которые вы указали при создании учетной записи администратора ModX:

**![](./assets/1558300426818-1558300426817.jpeg)**  

В результате откроется главная страница ModX:

![](./assets/1554808550477-1554808550477.png)

**Обратная связь**

Возникли проблемы или остались вопросы? [Напишите нам, мы будем рады вам помочь](https://mcs.mail.ru/help/contact-us).