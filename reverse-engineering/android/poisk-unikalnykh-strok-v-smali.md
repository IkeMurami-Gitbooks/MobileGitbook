# Поиск уникальных строк в Smali

```
grep -ir "const-string" $1 | grep "\\u" | cut -d'"' -f2 | sort | uniq
```

