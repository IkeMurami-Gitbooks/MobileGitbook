# sdkmanager

Образы, доступные для установки:

`sdkmanager --list | grep system-images`

Устанавливаем образ конкретный:

`sdkmanager --install <image-name>`

`sdkmanager "platform-tools"` - Устанавливаем так platform tools&#x20;

Другие пакеты: `"build-tools;2.0.34"` - конкретная версия&#x20;

`"platforms;android-28"` - устанавливаем Android API 28

```
# Набор джентельмена:
# Доставляем тулзы ддля запуска emulator, adb ...
sdkmanager "build-tools;29.0.3"
sdkmanager "platforms;android-25"
sdkmanager "platform-tools"
sdkmanager "emulator"
sdkmanager "system-images;android-25;google_apis;x86_64"
```
