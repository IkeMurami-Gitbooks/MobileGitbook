# Архитектура iOS приложения

![MVC в iOS приложениях](../../../.gitbook/assets/MVC\_в\_iOS\_приложениях.png)



![Архитектура iOS приложений](../../../.gitbook/assets/Архитектура\_iOS\_приложений.png)

## BOMStore and .car-files

В IPA-образе можно увидеть `Assets.car`-файл, который начинается с `BOMStore` байтов.

BOM — Bill of Materials

CAR — Compiled Asset Catalog

Чем это смотреть на MacOS:

`actool` — lets you compile, print, update, and verify asset catalogs

`assetutil` — process car files. It can remove unneeded assets from a car file but it can also parse a car file and produce a JSON output.

`$ assetutil -I Assets.car` — посмотреть, какие файлы внутри

`$ acextract -i Assets.car -o ./catalog` – распаковать с помощью [acextract](https://github.com/bartoszj/acextract) (проверил, работает только для изображений).

Вот этот бинарь экстрактит все: `./carDump Assets.car ~/out`

{% file src="../../../.gitbook/assets/carDump" %}

Основная статья про парсинг этого типа файлов: [https://blog.timac.org/2018/1018-reverse-engineering-the-car-file-format/](https://blog.timac.org/2018/1018-reverse-engineering-the-car-file-format/)[https://blog.timac.org/2018/1018-reverse-engineering-the-car-file-format/](https://blog.timac.org/2018/1018-reverse-engineering-the-car-file-format/)

## NIB Archive

Это специальный файл ресурсов для хранения UI.
