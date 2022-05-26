# Общая информация о системе: Полезные тулзы и команды

## ADB

### cmd command

Для новых телефонов (с обновой от первого июля 2018), взамен `pm` пришла команда `cmd` (посмотреть, что может: `cmd package help`) \
Посмотреть список установленных пакетов:&#x20;

```
host $> adb shell
android $>  cmd package list packages -f
```

cmd package - работа с менеджером пакетов\
cmd package list \<features | packages | libraries | instrumentation | permission-groups | permissions>

### Get an APK-file without a root access

Забрать APK с нерученного телефона:&#x20;

```
host $> adb shell
android $> pm list packages # Получили список пакетов
android $> pm path com.test.package # Получаем путь до пакета
android $> exit
host $> adb pull <Путь> # и никакого рута не надо :)
```

### Logcat & PID

Получить PID приложения:\
`adb shell pidof -s com.app.test`

Запустить logcat для конкретного приложения:\
`adb logcat --pid 11111`

`pidcat` - инструмент для логирования по pid

### Disable or delete any app without a root access

Как отключить или удалить любое приложение с телефона Android:\
Отключить: \
`$ adb shell pm disable-user --user 0 <имяотключаемогопакета>`\
Включить: \
`$ adb shell pm enable <имяотключенногопакета>`\
Список отключенных приложений: \
`$ adb shell pm list packages -d`\
Полностью удалить приложение: \
`$ adb pm uninstall -k --user 0 <имя_пакета>`

### Get android\_id

Получить android\_id: `adb shell settings get secure android_id`

## Some tools

### Termux

[Termux](https://termux.com/) - linux on Android\
Можно использовать для запуска git и прочего дерьма на андроиде\
Пример установки пакета:

```
pkg install git
pkg install inotify-tools
```

Прописать путь до своих библиотек в системную переменную:

```
export LD_LIBRARY_PATH=/data/data/com.termux/files/usr/lib
export PATH=$PATH:~/data/data/com.termux/files/usr/bin:/data/data/com.termux/files/usr/bin/applets
```

Для git'а:\
git прописывает email/password в директорию HOME, а в Android она / => read-only\
Чтобы не было проблем HOME-директорию можно переопределить на :\
`export HOME=/data/data/com.termux/files/home`

### Some scripts

Некоторые скрипты, которые облегчают действия на телефоне\
[https://github.com/zhenleiji/AndroidScripts](https://github.com/zhenleiji/AndroidScripts)

В Android приложении могут быть папки вида `%container%/app_/*` \
В приложении Android к ним доступ осуществляется через функцию `Context.getDir("")`;

## Others

### UIDs & App's permissions&#x20;

UID's приложений `/data/system/packages.list` \
в `packages.xml` еще и `Permissions`, и к каким сервисам системным для приложения необходим доступ

Все UID'ы, которые могут быть в Android (Linux?) системах:&#x20;

{% file src="../../.gitbook/assets/uids.txt" %}

### Processor Info

Узнать информацию о процессоре `cat /proc/cpuinfo`

### accounts.db

База accounts\[\_de|\_ce|].db хранит токены гугла и другую информацию, которую пожелает приложение там хранить. \
Где находится: \
1\. `/data/system/users/0/accounts.db` \
2\. На новых телефонах `/data/system[_ce|_de]/users[_ce|_de]/0/accounts*`

### API Levels

| OS version        | Name        | API Level |
| ----------------- | ----------- | --------- |
| Android 8.0       | Oreo        | 26        |
| Android 7.1       | Nougat      | 25        |
| Android 7.0       | Nougat      | 24        |
| Android 6.0       | Marshmallow | 23        |
| Android 5.1       | Lollipop    | 22        |
| Android 5.0       | Lollipop    | 21        |
| Android 4.4-4.4.4 | KitKat      | 19        |

### AT-команды

У андроида есть AT-команды, которые позволяют с ним взаимодействовать даже, если он выключен

### Отслеживание процессов на Android

Отслеживание процессов и приложений (варианты)&#x20;

* fsmon (inotify) - отслеживает обращение к файловой системе (модно указать типы событий и перечислить файлы). Есть ограничения на количество файлов для одного пользователя (ограничение дает inotify, его можно изменить)&#x20;
* atrace - отслеживает кучу событий приложений. Возможно, встроена в любом андроиде
* systrace - часть Platform Tools. atrace - часть systrace
* strace - отдельная тулза для Unix систем

Про inotify: \
inotify нюансы: \
1\. Информация об отслеживаемых дескрипторах: /proc/{PID}/fdinfo \
2\. API не предоставляет никакой инфы о процессе (или пользователе), который запустил событие inotify \
В частности, для процесса, который отслеживает события с помощью inotify, нет простого способа отличить события, которые он запускает, от событий, которые запускаются другими процессами

### Архитектура Android OS

![](../../.gitbook/assets/Урок\_1\_Введение\_в\_безопасность\_мобильных.pdf\_-\_Google\_Chrome\_2018-10-29\_01.11.15.png)

### Чтения сертификатов .RSA на aндроиде

`keytool -printcert -file ANDROID_.RSA`
