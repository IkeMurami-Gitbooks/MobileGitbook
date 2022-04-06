# Настройка резервного копирования

В AndroidManifest.xml:

```
android:fullBackupContent="true"

<full-backup-content>
 ...//your custom rules
    <exclude domain="sharedpref" path="appsflyer-data"/>
</full-backup-content>
```
