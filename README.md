Задание:
1) запустить контейнер с БД, отличной от mariaDB, используя инструкции на сайте: https://hub.docker.com/
2) добавить в контейнер hostname такой же, как hostname системы через переменную
3) заполнить БД данными через консоль
4) запустить phpmyadmin (в контейнере) и через веб проверить, что все введенные данные доступны

```
root@linux-gb:/home/rust# docker run -d -dit -h ubuntu --name test2 -e MYSQL_ROOT_PASSWORD=0000 mysql
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
4 rows in set (0.00 sec)

mysql> CREATE SCHEMA `productsdb` ;
Query OK, 1 row affected (0.03 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| productsdb         |
| sys                |
+--------------------+
5 rows in set (0.01 sec)

mysql> CREATE TABLE `productsdb`.`products` (
    ->   `id` INT NOT NULL AUTO_INCREMENT,
    ->   `ProductsName` VARCHAR(45) NULL,
    ->   `Vanufacturer` VARCHAR(45) NULL,
    ->   `ProductCount` INT NULL,
    ->   `Price` INT NULL,
    ->   PRIMARY KEY (`id`));
Query OK, 0 rows affected (0.07 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| productsdb         |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

mysql> INSERT INTO `productsdb`.`products` (`ProductsName`, `Vanufacturer`, `ProductCount`, `Price`) VALUES ('iPhone X', 'Apple', '3', '76000');
Query OK, 1 row affected (0.01 sec)

mysql> INSERT INTO `productsdb`.`products` (`ProductsName`, `Vanufacturer`, `ProductCount`, `Price`) VALUES ('iPhone 8', 'Apple', '2', '51000');
Query OK, 1 row affected (0.05 sec)

mysql> INSERT INTO `productsdb`.`products` (`ProductsName`, `Vanufacturer`, `ProductCount`, `Price`) VALUES ('Galaxy S9', 'Samsung', '2', '56000');
Query OK, 1 row affected (0.08 sec)

mysql> INSERT INTO `productsdb`.`products` (`ProductsName`, `Vanufacturer`, `ProductCount`, `Price`) VALUES ('Galaxy S8', 'Samsung', '1', '41000');
Query OK, 1 row affected (0.01 sec)

mysql> INSERT INTO `productsdb`.`products` (`ProductsName`, `Vanufacturer`, `ProductCount`, `Price`) VALUES ('P20 Pro', 'Huawei', '5', '36000');
Query OK, 1 row affected (0.03 sec)

mysql> use productsdb;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> SELECT ProductsName,Vanufacturer,Price
    -> FROM products
    -> WHERE ProductCount>2;
+--------------+--------------+-------+
| ProductsName | Vanufacturer | Price |
+--------------+--------------+-------+
| iPhone X     | Apple        | 76000 |
| P20 Pro      | Huawei       | 36000 |
+--------------+--------------+-------+
2 rows in set (0.00 sec)

mysql> exit;
Bye
bash-4.4# exit
exit

root@linux-gb:/home/rust# docker exec -it test2 bash
bash-4.4# exit
exit
root@linux-gb:/home/rust# docker container ls -a
CONTAINER ID   IMAGE     COMMAND                  CREATED        STATUS                    PORTS                 NAMES
9f7567ae7761   mysql     "docker-entrypoint.s…"   24 hours ago   Up 21 minutes             3306/tcp, 33060/tcp   test2
e6a5c48e3f04   mariadb   "docker-entrypoint.s…"   7 days ago     Exited (0) 24 hours ago                         test

root@linux-gb:/home/rust# docker run -d -dit -h ubuntu --name test5 --link test2:db -p 8085:80 phpmyadmin/phpmyadmin
94aa5078ea190d8d2d5ddfd8aa95bd98ed46b12df4c18d5bc962a3a3dbc14e60
root@linux-gb:/home/rust# docker container ls -a
CONTAINER ID   IMAGE                   COMMAND                  CREATED         STATUS                    PORTS                                   NAMES
94aa5078ea19   phpmyadmin/phpmyadmin   "/docker-entrypoint.…"   3 seconds ago   Up 2 seconds              0.0.0.0:8085->80/tcp, :::8085->80/tcp   test5
9f7567ae7761   mysql                   "docker-entrypoint.s…"   24 hours ago    Up 28 minutes             3306/tcp, 33060/tcp                     test2
e6a5c48e3f04   mariadb                 "docker-entrypoint.s…"   7 days ago      Exited (0) 24 hours ago                                           test

```
![phpmyadmin](https://github.com/Ledsager/container_homework3/blob/main/phpmyadmin.PNG)
