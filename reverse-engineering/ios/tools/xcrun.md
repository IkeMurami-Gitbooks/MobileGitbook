---
description: >-
  XCode (на macos) поставляет немало интересных тулзовин, которые помогают при
  реверсе
---

# xcrun

`xcrun` - запуск внутренних тулзовин \
Пример: \
`$ xcrun swift-demangle`&#x20;



Допустим в IDA нашли какую-то подобную строку:

```
_$s9MessProto0b11AuthSer...
```

Убираем `_$` и отправляем деманглеру:

```
$ xcrun swift-demangle s9MessProto0b11AuthSer...
s9MessProto0b11AuthSer... ----> MessProto.ProtoAuthSer...
```

