# Bluetooth

Класс девайса по 3 байтам, их кодирующим:&#x20;

{% file src="../.gitbook/assets/cod_definition.pdf" %}

Где искать информацию на девайсе:\
**Android**\
Скорее всего из: \
/data/misc/bluedroid/bt\_config.xml \
Но можно ещё проверить из: \
/data/misc/bluetoothd\* \
/data/misc/bluedroid/bt\_config.conf\
\
**iOS**\
/private/var/mobile/Library/Preferences/com.apple.MobileBluetooth.devices.plist\
/private/var/mobile/Applications/[systemgroup.com](http://systemgroup.com).apple.bluetooth/Library/Preferences/com.apple.MobileBluetooth.devices.plist\
/private/var/containers/Shared/SystemGroup/\<GUID>/Library/Preferences/com.apple.MobileBluetooth.devices.plist

Bluetooth RCE [https://insinuator.net/2020/04/cve-2020-0022-an-android-8-0-9-0-bluetooth-zero-click-rce-bluefrag/](https://insinuator.net/2020/04/cve-2020-0022-an-android-8-0-9-0-bluetooth-zero-click-rce-bluefrag/)

