# Разница между Android Library, Android APP и Java Library

## О Android App, Android Library и Java Library

Библиотека Android аналогична приложению Android в структуре каталогов и может содержать все необходимое для создания приложения (например, исходный код, файлы ресурсов, манифест Android).

Приложение Android скомпилировано и упаковано в файлы APK, которые можно запускать на устройствах Android, но библиотека Android скомпилирована в файлы архива Android (AAR), на которые полагаются другие приложения Android.

Библиотека Java компилируется и упаковывается в файлы архива Java (JAR). Она не может упаковывать файлы ресурсов Android.

## Зачем нужны Android Library

* Когда вам нужно предоставить общий модуль другим приложениям. Например: вам необходимо предоставить общий модуль управления учетной записью для других приложений.
* Когда вам нужно создать разные APK, но эти APK имеют общие основные функции. Например: у вашего приложения есть бесплатная и платная версии, но они имеют одинаковые основные функции.

В обоих случаях просто переместите файлы для повторного использования в библиотеку Android, а затем добавьте библиотеку как зависимость для каждого модуля приложения, и приложение сможет напрямую вызывать функции в библиотеке, не беспокоясь о конкретной реализации библиотеки.

## Как из модуля приложения сделать Android Library

1. Откройте файл build.gradle модуля приложения
2. Удалите строку applicationId. Только модуль приложения должен определять applicationId
3. Измените применить плагин: `com.android.application` в верхней части файла, чтобы применить плагин: `com.android.library`
4. Сохраните файл и нажмите Инструменты> Android> Синхронизировать проект с файлами Gradle

На этом преобразование модуля приложения в модуль библиотеки Android завершено. Вся структура модуля остается неизменной после преобразования, но это уже модуль библиотеки Android, и после компиляции будут созданы файлы AAR вместо файлов APK.

Выберите модуль библиотеки на панели «Проект» и нажмите `«Сборка»` `> «Сборка APK»`, чтобы скомпилировать и сгенерировать соответствующий файл AAR в каталоге `build> output> aar`.

## Как использовать Android Library

Если хотим поддерживать только один код библиотеки, следует использовать первый метод.

### Первый метод

Добавьте скомпилированный файл AAR в проект.

а. Щелкните Файл> Создать> Новый модуль.

б) Щелкните Импорт пакета .JAR / .AAR, а затем щелкните Далее.

В. Введите путь к файлу AAR и нажмите «Готово».

### Второй метод

Импортируйте библиотеку Android в проект (исходный код библиотеки становится частью проекта).

а. Щелкните Файл> Создать> Импортировать модуль.

б) Введите адрес каталога библиотеки и нажмите Готово.

Таким образом, библиотека копируется в проект, и вы можете редактировать код библиотеки.
