# Activities

### Intro

Могут быть вызваны любыми приложениям, если установлены флаги exported и enabled. Это может позволить загружать формы в обход блокировок (если не правильно настроено что или не делаются доп проверки). По умолчанию, exported=false, но если объявлен intent-filter, то true.\
Активити могут гарантировать правильное поведение, если отслеживают состояние приложения. Например, если приложение находится в заблокированном состоянии, перепрыгивать на LockScreen.

### Жизненный цикл активити

![](<../../../../.gitbook/assets/изображение (9).png>)

### Состояния активити

4 состояния:

* **Active** Когда активити взаимодействует с пользователем, находится на верху стека активити и видна пользователю. Android убьет любые другие сервисы или активити в стеке, чтобы сохранить активную активити в работоспособном состоянии.
* **Paused** На активити нет фокуса, но она видна пользователю.
* **Stopped** Активити не видна пользователю, но она ее хранится в памяти и все данные, в нее загруженные - тож.
* **Inactive** Активити до запуска и после убийства ее.

### Пример объявления активити

![](<../../../../.gitbook/assets/изображение (8).png>)

В коде:

```kotlin
package com.zfr.emapt

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        /*
        Чтобы можно было обратиться так к кнопке, надо добавить в
        app/build.gradle в самый вверх поддержку kotlinx:
        
            plugins {
                id 'kotlin-android-extensions'
            }
        или
            apply plugin: 'kotlin-android-extensions'
            
        */  
        button_test.setOnClickListener {
            // Обработчик нажатия
        }
    }
}
```

### Пример запуска Activity

```java
public static Intent getStartingIntent(Context context, String userId) {
 Intent i = new Intent(context, UserDetailsActivity.class);
 i.putExtra(EXTRA_USER_ID, userId);
 return i;
}
```

Здесь мы видим, что форма принимает userId - строку. Это хороший подход. Если бы принимался объект, то это плохо (чем?)

Хорошая практика избегать использования intent-filters для приватных активити

На стороне Activity

```java
.. onCreate(Bundle bundle) {
 Intent intent = getIntent()
 intent.getString(...)
}
```

Общий вид вызова Intent для Activity:&#x20;

```java
Intent(MainActivity.this, TargetActivity.class)
```

### Список запущенных активити

```
adb shell dumpsys activity activities
```

