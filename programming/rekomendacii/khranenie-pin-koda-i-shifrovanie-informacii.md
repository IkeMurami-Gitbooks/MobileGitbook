# Хранение PIN-кода и шифрование информации

## Хранение PIN-кода

Для онлайн приложения (работа приложения в оффлайн-режиме исключена) - использовать для проверки PIN-кода SRP-протокол ([https://ru.wikipedia.org/wiki/SRP](https://ru.wikipedia.org/wiki/SRP)). Этот подход не потребует хранения пин-кода в любом виде на устройстве

Для оффлайн приложения - шифровать с использованием ключа из iOS Keychain / Android KeyStore. Хеш не подходит по той причине, что PIN-код - легко перебираемое значение, следовательно, при утечке хеша исходное значение будет восстановлено.

Про SRP: [http://srp.stanford.edu/design.html](http://srp.stanford.edu/design.html)

## Шифрование информации&#x20;

Для хранения локального кеша, шифровать информацию следует с использованием ключей, сгенерированных в iOS Keychain / Android KeyStore.

В Android, PIN-код можно использовать как пароль для доступа к ключу шифрования: [https://developer.android.com/reference/java/security/KeyStore#getKey(java.lang.String,%20char%5B%5D)](https://developer.android.com/reference/java/security/KeyStore#getKey\(java.lang.String,%20char%5B%5D\))

Для старых версий Android-устройств:

На старых версиях устройств KeyStore сервис не реализован. В этом случае допустимо использовать для генерации ключей pbkdf2 от PIN-кода и android\_id как соли.\
