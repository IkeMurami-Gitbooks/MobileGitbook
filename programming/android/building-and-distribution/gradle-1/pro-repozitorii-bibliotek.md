# Про репозитории библиотек

Все пакеты публикуются в репозитории, чтобы каждый мог их себе подтянуть. Раньше были популярны JCenter и Google Maven, однако JCenter объявлен как deprecated и сейчас есть три варианта репозиториев:

* `google()` (Google Maven)
* `mavenCentral()`
* приватные Maven-репозитории

Посмотреть, какие версии доступны в публичных репозиториях можно по ссылке:

```
https://mvnrepository.com/artifact/<package-root>/<package-name>
```

Например, для пакета `org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version` узнать информацию о доступных версиях в открытых репозиториях можно по ссылке: [https://mvnrepository.com/artifact/org.jetbrains.kotlin/kotlin-gradle-plugin](https://mvnrepository.com/artifact/org.jetbrains.kotlin/kotlin-gradle-plugin)

