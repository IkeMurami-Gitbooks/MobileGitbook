# Permissions

## Использование

Создание разрешений:

```xml
<permission android:name="com.mycoolcam.USE_COOL_CAMERA" android:protectionLevel="dangerous" />
```

```xml
<activity android:name=".CoolCamActivity" android:exported="true" android:permission="com.mycoolcam.USE_COOL_CAMERA">
    <intent-filter>
        <action android:name="com.mycoolcam.LAUNCH_COOL_CAM" />
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</activity>
```

Если другое приложение захочет использовать это разрешение:

```markup
<uses-permission android:name="com.mycoolcam.USE_COOL_CAMERA" />
```

Это разрешение будет запрошено у пользователя перед установкой (и использованием, если это разрешение было запрошено динамически).

## Protection Level

* `normal`: это разрешение невидимо для пользователя (оно выдается автоматически без участия пользователя)
* `dangerous`: требует подтверждение пользователя
* `signature`: приложение, которое запрашивает доступ, должно быть подписано тем же сертификатом, что и приложение, которое это разрешение определило (система не позволит установиться приложению, которое запросило это разрешение через `uses-permission` и не было подписано корректным сертификатом).
* Другие привилегии, используемые системными приложениями: `system`, `installer`, `privileged`, ... Эти привилегии не могут быть запрошены через `uses-permission`. &#x20;

## Mistakes

### Forgot ProtectionLevel

Если не указать `ProtectionLevel`, по умолчанию будет установлен `normal`.

### Ecosystem mistakes

Пусть есть два приложения, подписанные на одном сертификате. В одном из приложений определено разрешение с `ProtectionLevel = signature`:

App 1:

```markup
<manifest xmlns:android="http://schemas.android.com/apk/res/android" package="com.mycoolcam">
    <permission android:name="com.mycoolcam.USE_COOL_CAMERA" android:protectionLevel="signature" />
    <uses-permission android:name="com.mycoolcam.USE_COOL_CAMERA" />

    <application android:label="My Cool Cam">
        <activity android:name=".CoolCamActivity" android:exported="true" android:permission="com.mycoolcam.USE_COOL_CAMERA">
            <intent-filter>
                <action android:name="com.mycoolcam.LAUNCH_COOL_CAM" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>
        <!-- ... -->
    </application>
</manifest>

```

App 2:

```markup
<manifest xmlns:android="http://schemas.android.com/apk/res/android" package="com.mycoolreader">
    <uses-permission android:name="com.mycoolcam.USE_COOL_CAMERA" />

    <application android:label="My Cool Reader">
        <provider android:name=".AllUserNotesContentProvider" android:authorities="com.mycoolreader.notes_provider" android:exported="true" android:permission="com.mycoolcam.USE_COOL_CAMERA" />
        <!-- ... -->
    </application>
</manifest>

```

К `AllUserNotesContentProvider` только App 1 сможет получить доступ (ну и само App 2). Но что, если будет установлено только упражнение App 2? Система поставит для разрешения `com.mycoolcam.USE_COOL_CAMERA` `ProtectionLevel` в `normal` и любое приложение сможет получить доступ к `ContentProvider`'у.

Mitigation: убедиться, что все расширения вашей экосистемы так же определены в каждом вашем приложении.

## File permissions

Не должны создаваться файлы с разрешениями: `MODE_WORLD_READABLE`, `MODE_WORLD_WRITABLE`

Для расшаривания файлов существует `ContentProvider`!
