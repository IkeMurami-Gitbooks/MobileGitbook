# Frida CLI

## Соотв frida-tools и frida

| Frida version | Frida-Tools version | Objection version |
| ------------- | ------------------- | ----------------- |
| 15.1.3        | 10.4.1              | Все равно         |
| 14.2.18       | 9.2.5               |                   |
| 12.11.18      | 8.2.0               |                   |
|               |                     |                   |

## Список запущенных процессов

```
frida-ps -Ua
```

## frida-trace

```
frida-trace -U -i CCCrypt* -m "-[NSURLRequest *]" -a WhatsApp!FF1235 WhatsApp
-i - нативные функции
-m - iOS методы
-a - перехват пользовательский функций, (sub_FF1235 или sub_10FF1235 ? ), 
        -a module!offset - в общем виде, если функция не в модуле, то module = название приложения
        Daniel: "Только там нюансы есть, на айос иногда надо 1 байт прибавить"
	(http://o2m.dyndns.org:39581/~petrakov/clxphpm58wwh5db.png)
-[NSURLRequest *] - хотим перехватить все методы класса NSURLRequest,
        если надо что-то конкретное, то вместо * - название метода

Перехват конкретных инструкций (по сути: breakpoint - можем перехватить любое место в коде)
Пример хука в hooks*.txt: -a Module!0x1001abbca! GOOD	# Просто добавляем ! в конец. Это адрес инструкции в IDA (нормализованное значение - без ASLR)
Пример hook.js обработчика:
{
	onInstruction: function (log, args, state)
	{
		log("--> Snapchat!0x1001abbca: x0 = " + this.context.x0 + ", x1 = " + this.context.x1 + ", x8 = " + find_module_offset_by_addr(this.context.x8));
		// x* - регистры, которые хотим перехватить (значение не нормализовано - с ASLR)
	}
}		
```

## frida-ls-devices

узнать device-id (для ключа device)
