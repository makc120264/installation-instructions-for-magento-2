# installation-instructions-for-magento-2
Установка Magento 2.x

    Настройка сервера:

    1.1 Создать пользователя:
        Зайти под root
            adduser [имя_пользователя_системы]
            usermod -aG sudo [имя_пользователя_системы] // Дать пользователю права sudo
            su - [имя_пользователя_системы]             // Переключиться на созданного пользователя

    1.2 Создать базу данных для Magento:
        su - root
        mysql                                  // Зайти в mysql        
        mysql> CREATE DATABASE [имя_базы];     // Создать БД
        mysql> CREATE USER '[имя_пользователя_mysql]'@'localhost' IDENTIFIED BY '[пароль_пользователя_mysql]'; // Создать пользователя БД
        mysql> GRANT ALL PRIVILEGES ON [имя_базы].* TO [имя_пользователя_mysql]@localhost IDENTIFIED BY '[пароль_пользователя_mysql]'; // Дать привелегии созданному пользователю на новую БД 
        mysql> SELECT User FROM mysql.user; // Посмотреть всех пользователей mysql
        mysql> SHOW DATABASES; // Посмотреть имеющиеся базы данных mysql
        su - [имя_пользователя_системы]
                
    1.3 Установка php:    
        sudo apt-get install python-software-properties
        sudo add-apt-repository ppa:ondrej/php
        sudo apt-get update
        sudo apt-get install -y php7.x 
        sudo update-alternatives --config php // переключить версию php 
        sudo a2enmod php7.x // включить модуль php
        sudo /etc/init.d/apache2 restart // перезапустить apache

    1.4 Установка Magento 2.x:
        запустить в браузере [host]/setup/#/landing-install
        установить необходимые модули php
            sudo apt-get install php7.1-[имя_модуля]
            sudo /etc/init.d/apache2 restart // перезапустить apache

        настроить права доступа для папок:
        cd /var/www/html                                                             // cd <your Magento install dir> 
        sudo find . -type f -exec chmod 644 {} \;                                    // 644 permission for files
        sudo find . -type d -exec chmod 755 {} \;                                    // 755 permission for directory
        sudo find /var/www/html/var -type d -exec chmod 777 -R {} \;                 // 777 permission for var folder
        sudo find /var/www/html/pub/media -type d -exec chmod 777 -R {} \;
        sudo find /var/www/html/pub/static -type d -exec chmod 777 -R {} \;
        sudo chmod 777 -R /var/www/html/app/etc
        sudo chmod 777 -R /var/www/html/generated
        sudo chmod 644 -R /var/www/html/app/etc/*.xml
        sudo chown -R www-data /root                                                 //chown -R :<web server group> .
        sudo chmod -R u+x /var/www/html/bin/magento
        sudo chmod 777 /var/www/html/app/etc/di.xml
        
    Если чистая система запустилась "криво" (не подключились стили), надо изменить кофигурацию apache:
    в файле /etc/apache2/apache2.conf
    изменить строки:
        <Directory /var/www/>
            ......
            AllowOverride none
            ......
    на:
        <Directory /var/www/>
            .......
            AllowOverride All
            ........

    потом выполнить в терминале
        sudo a2enmod rewrite // включить модуль mod_rewrite
        sudo /etc/init.d/apache2 restart // перезапустить apache
