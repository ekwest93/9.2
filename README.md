# 9.2
Система мониторинга Zabbix

### Задание 1 

Установите Zabbix Server с веб-интерфейсом.

#### Процесс выполнения
1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
3. Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
4. Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.

#### Требования к результаты 
1. Прикрепите в файл README.md скриншот авторизации в админке.

   
<img width="1791" height="852" alt="image" src="https://github.com/user-attachments/assets/21baf5b7-76e7-4c96-995b-9d42e4d1d04d" />



3. Приложите в файл README.md текст использованных команд в GitHub.
```bash

apt update
apt install postgresql
wget https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/zabbix-release_6.4-1+debian11_all.deb
dpkg -i zabbix-release_6.4-1+debian11_all.deb
sudo apt update
sudo apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts
su - postgres -c 'psql --command "CREATE USER zabbix WITH PASSWORD '\'12345678\'';"'
su - postgres -c 'psql --command "CREATE DATABASE zabbix OWNER zabbix;"'
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
sudo vim /etc/zabbix/zabbix_server.conf
sudo systemctl restart zabbix-server apache2
sudo systemctl enable zabbix-server apache2
```
---

### Задание 2 

Установите Zabbix Agent на два хоста.

<img width="744" height="274" alt="image" src="https://github.com/user-attachments/assets/11906413-8801-4707-a1df-7b5e9b16640b" />



#### Процесс выполнения
1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
3. Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
4. Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
5. Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.

#### Требования к результаты 
1. Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу

<img width="1412" height="518" alt="image" src="https://github.com/user-attachments/assets/7b02ed77-0797-4b71-9eff-3ad1b61746b7" />


2. Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
<img width="1161" height="596" alt="image" src="https://github.com/user-attachments/assets/83c66aa5-4fb3-416c-bc79-eee6e2a32249" />


<img width="1040" height="425" alt="image" src="https://github.com/user-attachments/assets/d631d8cc-7be6-4260-b5ba-120b995ee343" />

3. Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.

<img width="1538" height="809" alt="image" src="https://github.com/user-attachments/assets/2a86d9f3-ae86-45e6-b316-04c1fb4d0d58" />


4. Приложите в файл README.md текст использованных команд в GitHub
```bash
sudo -i
apt update
wget https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/zabbix-release_6.4-1+debian11_all.deb
dpkg -i zabbix-release_6.4-1+debian11_all.deb
apt update
apt install zabbix-agent
sed -i 's/Server=127.0.0.1/Server=89.169.176.175/g' /etc/zabbix/zabbix_agentd.conf
# На машине с Zabbix Server добавил 'Server=127.0.0.1,89.169.176.175' в файл /etc/zabbix/zabbix_agentd.conf
systemctl restart zabbix-agent
systemctl enable zabbix-agent
```
---
