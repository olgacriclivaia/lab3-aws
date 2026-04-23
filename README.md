# Лабораторная работа №3 — Облачные сети (AWS VPC)

---

## Ссылка на GitHub
https://github.com/olgacriclivaia/lab3-aws

---

## Цель работы
Изучение принципов построения виртуальной сети в AWS (VPC), настройка подсетей, таблиц маршрутизации, Internet Gateway, NAT Gateway, а также организация взаимодействия между публичными и приватными EC2-инстансами.

---

# Практическая часть

---

## 1. Подготовка среды

Вход в AWS Management Console.

Регион:
- Frankfurt (eu-central-1)

Все ресурсы создаются в одном регионе.

### Скриншот:
![01](img/01-aws-console-frankfurt.png)

---

## 2. Создание VPC

Параметры:
- Name: student-vpc-k9
- CIDR: 10.9.0.0/16

### Объяснение CIDR /16
Даёт около 65 000 IP-адресов для подсетей.

### Скриншоты:
![02](img/02-vpc-create-form.png)
![03](img/02-vpc-created.png)

---

## 3. Internet Gateway (IGW)

Обеспечивает доступ VPC в интернет.

### Скриншоты:
![04](img/03-igw-create.png)
![05](img/03-igw-attached.png)

---

## 4. Подсети

### Public Subnet
- CIDR: 10.9.1.0/24

### Private Subnet
- CIDR: 10.9.2.0/24

### Скриншоты:
![06](img/04-public-subnet-create.png)
![07](img/04-public-subnet-list.png)
![08](img/04-private-subnet-create.png)
![09](img/04-private-subnet-list.png)

---

## 5. Route Tables

### Public Route Table
- 0.0.0.0/0 → Internet Gateway

### Private Route Table
- 0.0.0.0/0 → NAT Gateway

### Скриншоты:
![10](img/05-public-route-table.png)
![11](img/05-public-route-routes.png)
![12](img/05-public-route-association.png)
![13](img/05-private-route-table.png)

---

## 6. NAT Gateway

### Elastic IP
- используется для NAT

### NAT Gateway
- размещён в public subnet

### Скриншоты:
![14](img/06-elastic-ip.png)
![15](img/06-nat-gateway.png)
![16](img/06-private-route-nat.png)

---

## 7. Security Groups

### Web SG
- HTTP/HTTPS открыт

### Bastion SG
- SSH только с моего IP

### DB SG
- MySQL (3306) только от Bastion SG

### Скриншоты:
![17](img/07-web-sg.png)
![18](img/07-bastion-sg.png)
![19](img/07-db-sg.png)

---

## 8. EC2 Instances

### Web Server
- public subnet

### DB Server
- private subnet

### Bastion Host
- доступ в private subnet

### Скриншоты:
![20](img/08-web-ec2.png)
![21](img/08-db-ec2.png)
![22](img/08-bastion-ec2.png)

---

## 9. Проверка работы

### Web в браузере
- PHP страница открывается

### SSH доступ
- подключение к bastion

### Ping
- проверка NAT

### MySQL через bastion

### Скриншоты:
![23](img/09-web-browser.png)
![24](img/09-ssh-bastion.png)
![25](img/09-ping-google.png)
![26](img/09-mysql-connection.png)

---

## 10. Bastion + Private subnet

Проверка доступа через SSH jump host.

Проверяется:
- DB доступ
- MySQL
- system tools (htop)

### Скриншоты:
![27](img/10-ssh-mysql-htop.png)

---

# Контрольные вопросы

## 1. Почему /16?
Большой диапазон IP для гибкой архитектуры.

## 2. Почему subnet не публичная сама по себе?
Публичность задаётся маршрутом через IGW.

## 3. Зачем Bastion Host?
Безопасный доступ к private subnet.

## 4. Что делает опция -A и -J?
-J → подключение через промежуточный сервер (bastion) автоматически
-A → передаёт SSH-ключи дальше по цепочке (без копирования на сервер)

---

# Вывод
Создана полноценная AWS VPC архитектура с разделением публичной и приватной сети, NAT Gateway для исходящего доступа и Bastion Host для безопасного администрирования.