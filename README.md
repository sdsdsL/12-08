# Домашнее задание к занятию 12.8. «Резервное копирование баз данных» - Денис Беляев 

### Задание 1. Резервное копирование

### Кейс
Финансовая компания решила увеличить надёжность работы баз данных и их резервного копирования. 

Необходимо описать, какие варианты резервного копирования подходят в случаях: 

1.1. Необходимо восстанавливать данные в полном объёме за предыдущий день.

Если необходимо восстановить данные за предыдущий день целиком, то можно использовать ежедневное полное резервное копирование. При таком подходе ежедневно создаётся полная копия базы данных, которая позволяет восстановить все данные и настройки базы на начало предыдущего дня. Можно конечно еще исспользовать инкрементное или дифференциальное резервное копирование данных, но в этом случае будут сохраняться только изменные данные и занимают меньше места, но при восстановлении данных потребуют применения нескольких копий для получения полного объема информации.

1.2. Необходимо восстанавливать данные за час до предполагаемой поломки.

В этом случае лучше подойдет инкрементное резервное копирование. Оно позволялет делать слепок изменений базы данных, произошедших после последнего полного или инкрементного копирования. То есть создается полное резервное копирование базы данных, а затем ежечасно или через определенный промежуток времени производится инкрементное копирование измененных данных. Это позволяет восстановить данные до конкретного момента времени и использовать более небольшой объем данных при восстановлении. Это позволяет оперативно восстановить данные до момента поломки.

1.3.* Возможен ли кейс, когда при поломке базы происходило моментальное переключение на работающую или починенную базу данных.



*Приведите ответ в свободной форме.*

---

### Задание 2. PostgreSQL

2.1. С помощью официальной документации приведите пример команды резервирования данных и восстановления БД (pgdump/pgrestore).

pg_dump -U username -h hostname -Fc -f /path/to/dumpfile.backup dbname - эта команда создаст бинарный дамп базы данных dbname и сохранит его в файл /path/to/dumpfile.backup в формате custom.

- -U username: задает имя пользователя для подключения к базе данных.
- -h hostname: задает имя хоста или IP-адрес машины, на которой запущен сервер PostgreSQL.
- -Fc: задает формат резервной копии как "custom" - сжатый двоичный формат, который можно использовать с утилитой pg_restore для восстановления базы данных.
- -f /path/to/dumpfile.backup: задает имя файла и путь для создания резервной копии.
- dbname: задает имя базы данных для создания резервной копии.

pg_restore -U username -h hostname -d dbname /path/to/dumpfile.backup - эта команда восстановит базу данных dbname из бинарного дампа, сохраненного в файле /path/to/dumpfile.backup.

- -U username: указывает имя пользователя, под которым будет выполнено подключение к базе данных.
- -h hostname: указывает имя хоста или IP-адрес машины, на которой запущен сервер PostgreSQL.
- -d dbname: указывает имя базы данных, в которую будет выполнено восстановление данных.
- /path/to/dumpfile.backup: указывает путь к файлу резервной копии базы данных, который будет использован для восстановления.

2.1.* Возможно ли автоматизировать этот процесс? Если да, то как?


*Приведите ответ в свободной форме.*

---

### Задание 3. MySQL

3.1. С помощью официальной документации приведите пример команды инкрементного резервного копирования базы данных MySQL. 

mysqldump --user=username --password=password --host=hostname --single-transaction --master-data=2 --incremental --incremental-basedir=/path/to/incremental/directory dbname > /path/to/dumpfile.sql

Опция --user указывает имя пользователя базы данных, которое должно иметь права на чтение всех таблиц в базе данных dbname.

Опция --password указывает пароль для пользователя.

Опция --host указывает имя хоста, на котором работает MySQL-сервер, к которому мы хотим подключиться.

Опция --single-transaction гарантирует, что mysqldump выполняет операцию в рамках транзакции и предотвращает блокирование таблиц во время процесса резервного копирования.

Опция --master-data=2 добавляет к дампу информацию о мастер-бинарном логе и позиции, которая может быть использована для восстановления базы данных до конкретной точки во времени.

Опция --incremental: параметр, который указывает, что будет создана инкрементальная резервная копия базы данных.

Опция --incremental-basedir=/path/to/incremental/directory: путь к каталогу, в котором хранятся предыдущие инкрементальные резервные копии. Этот параметр используется при создании инкрементальных резервных копий.

Опиция - dbname: имя базы данных MySQL, которую необходимо скопировать.

"> /path/to/dumpfile.sql" перенаправление вывода команды в файл /path/to/dumpfile.sql, где будет сохранена резервная копия базы данных.

3.1.* В каких случаях использование реплики будет давать преимущество по сравнению с обычным резервным копированием?

*Приведите ответ в свободной форме.*

---

Задания, помеченные звёздочкой, — дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.
