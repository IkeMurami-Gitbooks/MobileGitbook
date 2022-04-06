---
description: Хуки для Frida можно писать на JavaScript и TypeScript (с Frida 14.0.0)
---

# Общие команды (JS API)

## Общие функции

```
hexdump(target[, options]) - генерация хексдампа по ArrayBuffer или указателю (NativePointer) с опциями
    например: hexdump(buf, {offset: 0, length: 64, header: true, ansi: true})
int64(v): краткое объявление для new Int64(v)
uint64(v): то же для new UInt64(v)
ptr(v): то же для new NativePointer(s)
NULL: то же для ptr("0")
recv([type, ]callback) - получения ответа от нашего Frida-based приложения.
send(message[, data]) - отправка сообщения приложению (дб сериализовано в JSON)
```

## Вывод в консоль

```javascript
console.log(line)
console.warn(line) 
console.error(line)
```

## Работа с памятью

```
Указатель: 
Memory.readPointer(address) - читает указатель по адресу address и возвращает NativePointer
Числа:
Memory.readS8(address), 
Memory.readU8(address), 
Memory.readS16(address), 
Memory.readU16(address), 
Memory.readS32(address), 
Memory.readU32(address), 
Memory.readShort(address), 
Memory.readUShort(address), 
Memory.readInt(address), 
Memory.readUInt(address), 
Memory.readFloat(address), 
Memory.readDouble(address)
Memory.readS64(address), 
Memory.readU64(address), 
Memory.readLong(address), 
Memory.readULong(address),
Массивы:
Memory.readByteArray(address, length) - возвращает ArrayBuffer
Строки:
Memory.readCString(address[, size = -1])
Memory.readUtf8String(address[, size = -1])
Memory.readUtf16String(address[, length = -1])
Memory.readAnsiString(address[, size = -1])
```

## Работа с числами

```
new Int64(v) = int64(v) - делает число из JSNumber, или строки с числом, или хекса, начинающегося с '0x'
Методы Int64, UInt64:
num.add(rhs) = num + rhs
Другие методы: sub, and, or, xor, shr, shl, compare
Конвертирование: toNumber(), toString(), toInt32()
```

## Работа с указателями

```
new NativePointer(s) = ptr(s)
isNull()
add, sub, and, or, xor, shr, shl
equals(num), compare(num)
toInt32()
toString([radix = 16])
toMatchPattern(): returns a string containing a Memory.scan()-compatible match pattern for this pointer’s raw value

NativeFunction() - !!!! Можно вызывать свои функции?!!!!!
```

####
