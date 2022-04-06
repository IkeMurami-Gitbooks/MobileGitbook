# Upgrade Gradle Version

Для подключения gradle, надо задать версию для зависимости `com.android.tools.build:gradle`.

`build.gradle`:

```
buildscript {
    ext {
        gradle_version = "7.1.2"
    }

    repositories {
        google()
    }
    
    dependencies {
        classpath "com.android.tools.build:gradle:${gradle_version}"
    }
}
```

Вот здесь можно посмотреть какие версии доступны в google maven репозитории (`google()`): [https://mvnrepository.com/artifact/com.android.tools.build/gradle-api?repo=google](https://mvnrepository.com/artifact/com.android.tools.build/gradle-api?repo=google)

Есть два способа обновить версию gradle:

* Android Studio: File > Project Structure > установить версию Gradle из выпадающего списка. Нужная версия Gradle сразу подтянется
* Terminal:&#x20;

```
Информация по текущей версии
$ gradle help --scan
$ gradle --version

Обновить до 7.0:
$ gradle wrapper --gradle-version 7.0
```

