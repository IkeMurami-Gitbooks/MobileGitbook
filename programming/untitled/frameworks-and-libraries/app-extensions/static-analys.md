# Static Analys

## Проверить, что приложение содержит расширения&#x20;

### XCode

cmd+shift+f и ищем `NSExtensionPointIdentifier`

или открываем Build Phases / Embed App exetensions

Все, что с расширением .appex - расширения, теперь можем перейти в сорцы

### IPA

Грепаем по bundl'у

```
$ grep -nr NSExtensionPointIdentifier Payload/Telegram\ X.app/
```

или заходим на девайс плагины приложения на устройстве:

```
/var/containers/Bundle/Application/15E6A58F-1CA7-44A4-A9E0-6CA85B65FA35/Telegram X.app/PlugIns
```

## Определить поддерживаемые типы данных

В Info.plist ищем строку `NSExtensionActivationRule`

## Проверить обмен данными с Containing App

Не забываем, что напрямую доступа к контейнерам друг друга нет у Containing App и App Extension. Однако, data sharing доступен. Это сделано за счет App Groups и NSUserDefaults API.

![](<../../../../.gitbook/assets/изображение (23).png>)

Если расширение скачивает файлы через NSURLSession, то надо настроить shared container, чтобы содержащее расширение приложение могло получить доступ к скачиваемым файлам.

## Проверить, ограничивает ли приложение использование расширений

Можно отклонить определенный тип для расширния, используя  следующий метод:

```
application:shouldAllowExtensionPointIdentifier:
```

Однако, это доступно только для расширения Custom Keyboard (и это надо проверять, если приложения (например, банкоовские) обрабатывают конфид информации)
