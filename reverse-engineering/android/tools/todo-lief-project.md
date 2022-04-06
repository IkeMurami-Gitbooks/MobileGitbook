# TODO: LIEF-project

Есть проект по парсингу бинарей, в том числе и DEX, APK, ODEX, OAT, ...

Минусы: не достает поля, не строит xrefs

Но работает очень быстро

Пример:

```python
from pathlib import Path
import lief

# Загружаем DEX в LIEF
lief_dex_file = lief.DEX.parse('classes2.dex')
lief_odex_file = lief.OAT.parse('base.odex')

CLASS_NAME = 'Lcom/example/SomeClass$SomeOtherClass;'
if lief_dex_file.has_class(CLASS_NAME):
    lief_dex_class = lief_dex_file.get_class(CLASS_NAME)

    if lief_dex_class.parent.pretty_name == 'com.google.protobuf.GeneratedMessageLite':

        for method in lief_dex_class.methods:
            print(method)

```
