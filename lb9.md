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

### 1.3 Опрацювання команд Lab 16 - Creating Users and Groups

| Назва команди | Її призначення та функціональність |
|---|---|
| su - | Команда **su** використовується для перемикання на користувача root для виконання адміністративних дій. |
| groupadd -r research | Команда **groupadd** створює нову групу. Параметр **-r** означає створення системної групи з GID у діапазоні 1–999. |
| groupadd -r sales | Команда **groupadd** створює нову системну групу **sales**. |
| getent group research | Команда **getent** використовується для отримання інформації про групу **research** з системної бази даних. |
| grep sales /etc/group | Команда **grep** використовується для пошуку запису про групу **sales** у файлі **/etc/group**. |
| groupmod -n clerks sales | Команда **groupmod** використовується для зміни назви групи з **sales** на **clerks**. |
| groupmod -g 10003 clerks | Команда **groupmod** використовується для зміни GID групи **clerks**. |
| grep clerks /etc/group | Команда **grep** використовується для перевірки змін у групі **clerks**. |
| groupdel clerks | Команда **groupdel** використовується для видалення групи **clerks**. |
| useradd -D | Команда **useradd -D** використовується для перегляду значень за замовчуванням при створенні користувачів. |
| useradd -D -f 30 | Команда **useradd** з параметром **-D -f 30** змінює значення INACTIVE, дозволяючи користувачу входити ще 30 днів після завершення дії пароля. |
| nano /etc/default/useradd | Команда **nano** використовується для редагування конфігураційного файлу налаштувань створення користувачів. |
| useradd -G research -c 'Linux Student' -m student | Команда **useradd** створює нового користувача. Параметр **-G** додає до додаткової групи, **-c** задає опис, **-m** створює домашню директорію. |
| grep student /etc/passwd | Команда **grep** використовується для перевірки створення користувача у файлі **/etc/passwd**. |
| grep student /etc/group | Команда **grep** використовується для перевірки членства користувача у групах. |
| usermod -aG research sysadmin | Команда **usermod** використовується для додавання користувача до додаткової групи (**-aG**). |
| getent group research | Команда **getent** показує список користувачів у групі **research**. |
| getent group student | Команда **getent** показує інформацію про групу **student**. |
| getent passwd student | Команда **getent** використовується для перегляду інформації про користувача. |
| getent shadow student | Команда **getent** використовується для перегляду інформації про пароль користувача. |
| passwd student | Команда **passwd** використовується для встановлення або зміни пароля користувача. |
| last | Команда **last** використовується для перегляду історії входів у систему. |
| last student | Команда **last student** перевіряє, чи входив конкретний користувач у систему. |
| usermod -L student | Команда **usermod -L** використовується для блокування облікового запису користувача. |
| usermod -U student | Команда **usermod -U** використовується для розблокування облікового запису. |
| userdel student | Команда **userdel** використовується для видалення користувача. |
| userdel -r student | Команда **userdel -r** видаляє користувача разом із його домашньою директорією. |
| grep student /etc/group | Команда **grep** використовується для перевірки, чи залишилась інформація про користувача після видалення. |

### 1.4 Практичне виконання роботи у терміналі (керування користувачами та групами)

#### Перегляд інформації про поточного користувача

Для перегляду інформації про поточного користувача я використала команди:

```bash
id
whoami
grep $(whoami) /etc/passwd
```
- Команда id показує UID, GID та групи користувача.
- Команда whoami відображає ім’я поточного користувача.
- Команда grep $(whoami) /etc/passwd дозволяє знайти запис користувача у системному файлі.

<img width="818" height="118" alt="image" src="https://github.com/user-attachments/assets/97b417ad-3d0f-42bc-beff-3aaab1b9bf4e" />

Скрін - перегляд інформації про користувача за допомогою команди id  

<img width="599" height="81" alt="image" src="https://github.com/user-attachments/assets/d7ede247-5507-44cd-8581-b89b7b057ee2" />

Скрін - перегляд запису користувача у файлі /etc/passwd за допомогою команди grep  

<img width="378" height="88" alt="image" src="https://github.com/user-attachments/assets/73171e37-e48c-48c1-b3db-954bb58630c8" />

Скрін - визначення імені поточного користувача командою whoami

#### Порівняння команд who, w та last

Для аналізу користувачів системи я використала команди:
```bash
who
w
last
```
- Команда who показує тільки базову інформацію про користувачів (ім’я, термінал, час входу).
- Команда w надає детальну інформацію (активність, навантаження системи, процеси).
- Команда last показує історію входів у систему.

<img width="600" height="83" alt="image" src="https://github.com/user-attachments/assets/f30d9e58-a95a-406c-9bda-7e70b8ed775f" />

Скрін - who

<img width="817" height="101" alt="image" src="https://github.com/user-attachments/assets/2e04902f-f632-4d37-a545-5661a6f54bff" />

Скрін - w

<img width="838" height="532" alt="image" src="https://github.com/user-attachments/assets/ea601e8f-6dc5-4d0b-a1a8-b967a43e523b" />

Скрін - last

#### Створення нових груп користувачів та визначення їх ідентифікаторів

Для виконання завдання було створено три нові групи користувачів: **super_admins**, **noob_users** та **good_students** за допомогою команди:

```bash
sudo groupadd super_admins
sudo groupadd noob_users
sudo groupadd good_students
```
Після створення груп я перевірила їх наявність та ідентифікатори (GID) за допомогою команд:
```bash
cat /etc/group | grep super_admins
cat /etc/group | grep noob_users
cat /etc/group | grep good_students
```
У результаті було встановлено, що кожній групі автоматично присвоюється унікальний ідентифікатор (GID):
- super_admins - 2001
- noob_users - 2002
- good_students - 2003

Ці ідентифікатори використовуються системою для керування доступом до ресурсів.

<img width="567" height="99" alt="image" src="https://github.com/user-attachments/assets/556e6538-a653-415a-9041-9870f18d3c56" />

Скрін - створення груп користувачів

<img width="656" height="158" alt="image" src="https://github.com/user-attachments/assets/4f548c25-c0e2-44ba-bc17-d4ee2d3e9150" />

Скрін - перегляд груп та їх GID у файлі /etc/group

#### Створення користувачів та встановлення паролів

Було створено трьох користувачів:
```bash
sudo useradd -m user1
sudo useradd -m user2
sudo useradd -m user3
```

Після цього для кожного користувача встановлено пароль:
```bash
sudo passwd user1
sudo passwd user2
sudo passwd user3
```

Перевірка:
```bash
cat /etc/passwd | grep user
```

<img width="548" height="67" alt="image" src="https://github.com/user-attachments/assets/d83c848a-bec6-4641-bdeb-7e1d5b18ec19" />

Скрін - створення користувачів

<img width="471" height="295" alt="image" src="https://github.com/user-attachments/assets/4f9c80be-d9e2-4601-9253-9366be56b5f5" />

Скрін - встановлення паролів

<img width="932" height="206" alt="image" src="https://github.com/user-attachments/assets/7f62f5b6-43c3-4b48-8148-e3eaf3616264" />

Скрін - перевірка користувачів

#### Додавання користувачів до груп

Користувачі були додані до груп відповідно до умов завдання:
```bash
sudo usermod -aG super_admins user1
sudo usermod -aG super_admins user2

sudo usermod -aG noob_users user1
sudo usermod -aG noob_users user3

sudo usermod -aG good_students user1
sudo usermod -aG good_students user2
sudo usermod -aG good_students user3
```

Перевірка:
```bash
getent group super_admins
getent group noob_users
getent group good_students
```

<img width="644" height="164" alt="image" src="https://github.com/user-attachments/assets/ad4a0a62-7320-4e4e-8ccf-39b1af914af9" />

Скрін - додавання користувачів у групи

<img width="548" height="164" alt="image" src="https://github.com/user-attachments/assets/326a1447-2a66-46c1-9a5d-cb4b448e8ec8" />

Скрін - перевірка складу груп

#### Перегляд інформації про групи
```bash
getent group
```
Було переглянуто всі групи та їх склад.

<img width="509" height="42" alt="image" src="https://github.com/user-attachments/assets/bdae1095-b850-4cb5-ba1f-179b962d4adf" />

<img width="580" height="166" alt="image" src="https://github.com/user-attachments/assets/207a6026-cb5c-4202-a17a-b40ff760830a" />

Скрін - перегляд груп 

#### Видалення користувачів

Користувачі були видалені по черзі:
```bash
sudo userdel user1
sudo userdel user2
sudo userdel user3
```
Після кожного видалення було перевірено склад груп:
```bash
getent group
```

<img width="509" height="42" alt="image" src="https://github.com/user-attachments/assets/cac5f93d-a7ab-4e30-a6eb-1a9b76c27ad9" />

<img width="580" height="166" alt="image" src="https://github.com/user-attachments/assets/6a5c15c2-2c59-4920-abb0-7673b3fdfc1e" />

<img width="485" height="43" alt="image" src="https://github.com/user-attachments/assets/07471467-f198-4e58-b225-00430bfa1523" />

<img width="552" height="156" alt="image" src="https://github.com/user-attachments/assets/50e59926-e3c9-499a-a063-770a8e8a871c" />

<img width="471" height="32" alt="image" src="https://github.com/user-attachments/assets/dc2ce367-3f54-4bab-9906-34df514d3e15" />

<img width="550" height="146" alt="image" src="https://github.com/user-attachments/assets/2cb47265-a8a0-4b38-80ea-df2586888f8d" />

Скрін - видалення користувачів

<img width="405" height="48" alt="image" src="https://github.com/user-attachments/assets/05d90759-6b9b-43e7-89f4-ed315cc04627" />

<img width="426" height="219" alt="image" src="https://github.com/user-attachments/assets/27177823-4b86-4e56-8367-78246b3b6613" />

Скрін - перевірка груп після видалення

#### Видалення груп користувачів

Було видалено створені групи:
```bash
sudo groupdel super_admins
sudo groupdel noob_users
sudo groupdel good_students
```

Перевірка:
```bash
getent group
```

<img width="563" height="68" alt="image" src="https://github.com/user-attachments/assets/96c73cc1-457a-427b-a396-ddd053a6da1a" />

<img width="612" height="85" alt="image" src="https://github.com/user-attachments/assets/e677c4b3-4bbb-403f-a673-5f52a5866686" />

Скрін - видалення груп

<img width="419" height="45" alt="image" src="https://github.com/user-attachments/assets/1d840b23-4571-4d67-b843-f4caaee3eb47" />

<img width="359" height="228" alt="image" src="https://github.com/user-attachments/assets/cec7ac82-f645-4a17-ad85-512f12545c07" />

Скрін - фінальна перевірка
