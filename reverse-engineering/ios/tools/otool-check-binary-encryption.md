# otool: check binary encryption

Проверить, зашифрован ли бинарь (по умолчанию, они пошифрованы на устройстве)

* Зашифрован:

```
$ cd /var/containers/Bundle/Application/GUID/FB.app/
// Здесь будет бинарь Facebook на телефоне
$ otool -l Facebook | grep LC_ENCRYPTION_INFO -A5
      cmd LC_ENCRYPTION_INFO_64
      cmdsize 24
      cryptoff 20480
      cryptsize 4096
      cryptid 1
      pad 0

```

* Не зашифрован:

```
// Получили Facebook.ipa через frida-ios-dump
$ unzip Facebook.ipa
$ cd Payload/Facebook.app/
$ otool -l Facebook | grep LC_ENCRYPTION_INFO -A5
    cryptid 0
```
