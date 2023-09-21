# Xamarin

## Papers and notes

Xamarin RE for pentesters: [https://www.appknox.com/security/xamarin-reverse-engineering-a-guide-for-penetration-testers](https://www.appknox.com/security/xamarin-reverse-engineering-a-guide-for-penetration-testers)

Из источника [https://t.me/android\_guards/50630](https://t.me/android\_guards/50630):

Современные приложения на Xamarin хранят dll-ки в `XABA` архиве, который лежит в ресурсах приложения по пути `assemblies/assemblies.blob` (могут быть также разделены по архитектурам). Там же рядом лежит `assemblies.manifest` с списком всех сборок. Для того чтобы извлечь все сборки в виде dll-файлов и спокойно реверсить, можно воспользоваться утилитой pyxamstore ([https://github.com/jakev/pyxamstore](https://github.com/jakev/pyxamstore)). Она также умеет собирать все обратно.

## Frida

### Xamarin App: Frida Hooks

Про то как цеплять хуки frida на xamarin app (написаны на mono C# приложения)\
[https://orangewirelabs.wordpress.com/2019/05/30/hacking-ios-xamarin-apps-with-frida/](https://orangewirelabs.wordpress.com/2019/05/30/hacking-ios-xamarin-apps-with-frida/)\
[https://github.com/freehuntx/frida-mono-api](https://github.com/freehuntx/frida-mono-api)
