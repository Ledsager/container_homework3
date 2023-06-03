Задание:
1) запустить контейнер с БД, отличной от mariaDB, используя инструкции на сайте: https://hub.docker.com/
2) добавить в контейнер hostname такой же, как hostname системы через переменную
3) заполнить БД данными через консоль
4) запустить phpmyadmin (в контейнере) и через веб проверить, что все введенные данные доступны

```
root@linux-gb:/home/rust# docker run -d -dit -h ubuntu --name test2 -e MYSQL_ROOT_PASSWORD=rust mysql
root@linux-gb:/home/rust# docker container ls -a
CONTAINER ID   IMAGE        COMMAND                  CREATED          STATUS                  PORTS                 NAMES
856f9fed8a84   mysql        "docker-entrypoint.s…"   45 seconds ago   Up 41 seconds           3306/tcp, 33060/tcp   test2

root@linux-gb:/home/rust# docker exec -it test2 bash
bash-4.4# mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.33 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)

mysql> CREATE DATABASE newdb DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
Query OK, 1 row affected, 2 warnings (0.02 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| newdb              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

mysql> exit;
Bye
bash-4.4# exit
exit

root@linux-gb:/home/rust# docker run -d -dit -h ubuntu --name test2 --link mysql:db -p 8080:80 phpmyadmin/phpmyadmin

```
