# VIPER (Architecture)

На проекте принята архитектура VIPER ([https://habr.com/ru/post/358412/](https://habr.com/ru/post/358412/)).&#x20;

* Мы следуем принятым в команде архитектурным паттернам и названиям.
* Предпочтительнее использовать dependency injection, чем dependencies.
* Предпочтительнее композиция, чем наследование, так как в Swift не поддерживает множественное наследование.
* Используем расширения для протоколов, чтобы сделать реализацию методов по-умолчанию.

Темплейт под VIPER архитектуру: [https://github.com/infinum/iOS-VIPER-Xcode-Templates](https://github.com/infinum/iOS-VIPER-Xcode-Templates)
