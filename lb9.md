# МІНІСТЕРСТВО ОСВІТИ І НАУКИ УКРАЇНИ  
## КИЇВСЬКИЙ ФАХОВИЙ КОЛЕДЖ ЗВ’ЯЗКУ  

---

# ЗВІТ  
## про виконання лабораторної роботи №9
### з дисципліни «Операційні системи»

---

**Тема:**  
Захист системи та користувачів у Linux. Створення користувачів та груп

---

**Виконала:**  
студентка групи **БІКС-33**  
**Сербіна Ярослава Вячеславівна**

---

**Перевірила:**  
**Сушанова Вікторія Сергіївна**

---

**Київ - 2026**

---

# Лабораторна робота №9
## Операційні системи

---

## Тема
Захист системи та користувачів у Linux. Створення користувачів та груп

---

## Мета роботи
1. Отримання практичних навиків роботи з командною оболонкою Bash.
2. Знайомство з базовими діями при створенні нових користувачів та нових груп користувачів.


---

## Матеріальне забезпечення занять:
1. ЕОМ типу IBM PC.  
2. ОС сімейства Windows та віртуальна машина VirtualBox (Oracle).  
3. ОС GNU/Linux (будь-який дистрибутив).  
4. Сайт мережевої академії Cisco netacad.com та його онлайн курси по Linux.

---

## Завдання для попередньої підготовки
### Glossary of Basic English Terms

**Root user - a special administrative account with full system access and UID 0.**

**Administrative privileges - permissions required to perform system-level tasks and execute critical commands.**

**sudo - allows a user to execute commands with administrative privileges without logging in as root.**

**su - used to switch from one user account to another, typically to the root account.**

**User account - an account that enables a person to log into the system and use its resources.**

**System account - an account used by system services, usually not intended for interactive login.**

**UID - a unique identifier assigned to each user in the system.**

**GID - a unique identifier assigned to each group.**

**Group - a collection of users that share common access permissions.**

**Primary group - the main group associated with a user, defined in the /etc/passwd file.**

**Supplemental group - additional groups a user belongs to, defined in the /etc/group file.**

**/etc/passwd - a file that stores basic information about user accounts.**

**/etc/group - a file that contains information about groups and their members.**

**/etc/shadow - a file that stores encrypted passwords and security-related information.**

**Home directory - a personal directory assigned to a user for storing their files.**

**User Private Group (UPG) - a group created for a single user with the same name as the user.**

**id - displays information about a user and their group memberships.**

**who - shows users currently logged into the system and their session details.**

**last - displays login history based on records from the system log file.**

**/var/log/wtmp - a log file that stores login and logout activity of users.**

**groupadd - creates a new group in the system.**

**groupmod - modifies an existing group (name or GID).**

**grep - searches for specific patterns in files.**

**getent - retrieves entries from system databases such as users and groups.**

**Command options - parameters used with commands to modify their behavior (e.g., -g, -n).**

**Runlevel - a state of the system that determines which services are running.**

## Відповіді на теоретичні питання

### 1. Розкрийте поняття UPG, коли їх доцільно використовувати?

UPG (User Private Group) - це окрема група, яка створюється автоматично для кожного користувача і має таку ж назву, як і сам користувач. У такій групі є тільки один учасник - цей користувач.

Це зручно тим, що всі файли, які створює користувач, належать його особистій групі, що підвищує безпеку та дозволяє краще керувати доступом до файлів.

UPG доцільно використовувати у випадках, коли:
- система використовується кількома користувачами;
- потрібно забезпечити ізоляцію файлів кожного користувача;
- необхідно гнучко налаштовувати права доступу до файлів і директорій.

Скрін 1 - приклад запису користувача та його групи у файлі /etc/group 

<img width="827" height="306" alt="image" src="https://github.com/user-attachments/assets/151976dd-c38b-4762-981d-c2015cdd5722" />

### 2. Якими командами можна створити групи користувачів? Наведіть приклади

Для створення груп користувачів у Linux використовується команда groupadd.

Вона дозволяє створити нову групу з автоматичним або заданим ідентифікатором (GID).

Приклади:

- Створення групи:
```bash
sudo groupadd students
```
- Створення групи із заданим GID:
```bash
sudo groupadd -g 1001 developers
```
Після виконання команди інформація про групу додається до файлу /etc/group.

<img width="594" height="64" alt="image" src="https://github.com/user-attachments/assets/e0d0309f-74e1-41d4-a302-f5ce48479b6f" />

<img width="626" height="83" alt="image" src="https://github.com/user-attachments/assets/17c5d06f-2de6-491e-a3fb-52f2f611b8f9" />

### 3. Якими командами можна змінити налаштування груп користувачів? Наведіть приклади

Для зміни параметрів груп використовується команда groupmod.

Вона дозволяє змінювати назву групи або її ідентифікатор (GID).

Приклади:
- Зміна назви групи:
```bash
sudo groupmod -n newname oldname
```

<img width="626" height="107" alt="image" src="https://github.com/user-attachments/assets/7150a0d8-3990-4b67-a85c-b53df323b609" />

- Зміна GID групи:
```bash
sudo groupmod -g 2000 students
```

<img width="597" height="85" alt="image" src="https://github.com/user-attachments/assets/644c9ad1-53bf-4f2a-8fe1-9b573f91d0d3" />

Також для роботи з групами та перегляду інформації можуть використовуватись додаткові команди, наприклад:
- getent group - для перегляду інформації про групи;
- grep groupname /etc/group - для пошуку конкретної групи у файлі.

## Хід роботи
### 1.1 Запуск операційної системи

Для виконання лабораторної роботи було запущено віртуальну машину VM2_Linux_CLI у середовищі VirtualBox. Після запуску відбулося завантаження операційної системи Linux Ubuntu в режимі командного рядка (CLI)

### 1.2 Опрацювання команд Lab 15 - System and User Security
| Назва команди | Її призначення та функціональність |
|---|---|
| su - | Команда **su** використовується для перемикання на іншого користувача. У даному випадку команда **su -** дозволяє перейти до користувача root та відкрити нову оболонку з його правами. |
| id | Команда **id** використовується для перевірки поточного користувача. Вона показує UID, GID та групи, до яких належить користувач. |
| exit | Команда **exit** використовується для виходу з поточної оболонки. У цьому випадку вона дозволяє вийти з режиму root та повернутися до звичайного користувача. |
| head /etc/shadow | Команда **head** використовується для перегляду перших рядків файлу. У цьому випадку вона намагається показати вміст файлу **/etc/shadow**, але без прав доступу виникає помилка Permission denied. |
| sudo head /etc/shadow | Команда **sudo** дозволяє виконати команду з правами адміністратора. У цьому випадку вона дозволяє переглянути вміст файлу **/etc/shadow**, який доступний тільки для root. |
| head /etc/passwd | Команда **head** використовується для перегляду перших рядків файлу **/etc/passwd**, де зберігається інформація про користувачів (ім’я, UID, GID, домашня директорія, shell). |
| grep sysadmin /etc/passwd | Команда **grep** використовується для пошуку інформації у файлі. У цьому випадку вона знаходить дані про користувача **sysadmin** у файлі **/etc/passwd**. |
| head -3 /etc/shadow | Команда **head** використовується для перегляду перших 3 рядків файлу **/etc/shadow**, який містить зашифровані паролі користувачів. Без прав доступу виникає помилка. |
| ls -l /etc/shadow | Команда **ls -l** використовується для перегляду детальної інформації про файл. У цьому випадку вона показує права доступу до файлу **/etc/shadow**, власника та групу. |
| getent passwd sysadmin | Команда **getent** використовується для отримання інформації про користувача з системної бази даних. Вона може працювати як з локальними, так і мережевими обліковими записами. |
| man 5 passwd | Команда **man** використовується для перегляду довідкової інформації. У цьому випадку вона відкриває опис структури файлу **/etc/passwd**. |
| id root | Команда **id** використовується для перегляду інформації про конкретного користувача. У цьому випадку вона показує дані про користувача root. |
| who | Команда **who** використовується для перегляду користувачів, які зараз увійшли в систему. Вона показує ім’я, термінал, дату та час входу. |
| w | Команда **w** використовується для отримання детальної інформації про користувачів, включаючи їх активність, навантаження системи та виконувані процеси. |
| last | Команда **last** використовується для перегляду історії входів у систему. Вона читає дані з файлу **/var/log/wtmp** та показує всі входи та перезавантаження. |
