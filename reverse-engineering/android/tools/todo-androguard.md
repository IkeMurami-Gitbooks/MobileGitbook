# TODO: Androguard

Это отличнейшая Python-библиотека для парсинга APK, Dex и др файлов, связанных с Android-приложениями. Есть возможность подключать различные декомпиляторы и строить XREF'ы в обе стороны (что класс или метод вызывают, кто их вызывает).

Минусы: не достает информацию и статических полях.

Пример

```python
"""
Получаем список всех классов, что ресширяют (extends) com.google.protobuf.GenerateMessageLite
"""

from androguard.core.bytecodes.apk import APK
from androguard.core.bytecodes.dvm import DalvikVMFormat
from androguard.core.analysis.analysis import Analysis
from androguard.core.analysis.analysis import ClassAnalysis
from androguard.misc import AnalyzeAPK
from pathlib import Path
from androguard.decompiler.decompiler import DecompilerJADX


def ClassToProto(classAnalysis: ClassAnalysis):
    # Androguard не может в статические поля (((
    # А LIEF вообще не может в поля..

    for fieldClassAnalysis in classAnalysis.get_fields():
        encodedField = fieldClassAnalysis.field
        print(encodedField.name)

    print(classAnalysis.name)


# Загружаем APK в Androguard
apk_file = Path('my.apk')
BASE_CLASS_NAME = 'Lcom/google/protobuf/GeneratedMessageLite;'


def test_apk():

    apk_info, dex_info_list, analysis = AnalyzeAPK(apk_file)

    # Получаем информацию о всех классах, что расширяют com.google.protobuf.GeneratedMessageLite
    generatedMessageLiteClassAnalysis = analysis.get_class_analysis(BASE_CLASS_NAME)

    res = []

    if generatedMessageLiteClassAnalysis:
        for classAnalysis in generatedMessageLiteClassAnalysis.get_xref_from():
            if classAnalysis.extends == BASE_CLASS_NAME:  # We can extend only one class!
                res.append(classAnalysis.name)
                # ClassToProto(classAnalysis)
                ...
            ...
        ...
    else:
        print('com.google.protobuf.GeneratedMessageLite not found :(')

    # Save classes
    with Path('out/classes.text').open(mode='w') as out_stream:
        out_stream.write('\n'.join(res))


def test_jadx():
    apk_info = APK(apk_file)
    d = DalvikVMFormat(apk_info)
    dx = Analysis(d)

    decompiler = DecompilerJADX(d, dx)

    d.set_decompiler(decompiler)
    d.set_vmanalysis(dx)

    class_ = d.get_class(BASE_CLASS_NAME)
    ...

test_apk()
```
