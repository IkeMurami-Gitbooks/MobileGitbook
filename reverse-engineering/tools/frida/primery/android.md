# Android

## Ссылки

Детект библиотеки для пиннинга: [https://codeshare.frida.re/@akabe1/frida-multiple-unpinning/](https://codeshare.frida.re/@akabe1/frida-multiple-unpinning/)

universal script unpinning: [https://codeshare.frida.re/@pcipolloni/universal-android-ssl-pinning-bypass-with-frida/](https://codeshare.frida.re/@pcipolloni/universal-android-ssl-pinning-bypass-with-frida/)

Набор скриптов: \
[https://github.com/m0bilesecurity/Frida-Mobile-Scripts](https://github.com/m0bilesecurity/Frida-Mobile-Scripts)\
[https://github.com/LizhangHuang/FridaScript](https://github.com/LizhangHuang/FridaScript)\


Извлечение информации о Bluetooth: [https://github.com/k3170makan/FridaAndroidScripts/tree/master/bluecrawl](https://github.com/k3170makan/FridaAndroidScripts/tree/master/bluecrawl)

## Скрипты

## Common

#### Список методов и полей класса

```javascript
// Get class
const java_class = Java.use('com.example.j$R')

// Object cast
const java_class_obj = Java.cast(data, java_class)

// Get object via constructor
const java_class_obj = java_class.$new() // или java_class.$init()

// Methods
Java.enumerateMethods(`com.example.j$R!*/isu`) // Include method signatures (s) and User-defined classes only, ignoring system classes. (u) and case sensitive (i)

// All Fields and Methods names
Object.getOwnPropertyNames(java_class)
Object.getOwnPropertyNames(java_class_obj)
```

#### Обращение к приватным полям

```javascript
// Что б получить доступ к приватному полю, используем свойство value:
class A {
	private int b;
}
var classA = Java.use("A");
var objClassA = Java.cast(pointer, classA);
java_msg(Object.getOwnPropertyNames(objClassA.__proto__).join(' ')); -> посмотрели какие методы и поля у класса
var field_b = objClassA._b.value; -> а это уже наше поле
```

#### Обращение к любым полям объекта по имени

```javascript
const klass = Java.use('com.example.a$b')
const klass_obj = klass.$new()

const somePropertyOrMethodObj = klass_obj['some_prop']
```

#### Значение поля

```javascript
const klass = Java.use('com.example.a$b')
const klass_obj = klass.$new()

const somePropertyOrMethodObj = klass_obj['some_prop']
const value = somePropertyOrMethodObj.value
```

#### Вывести массив как hex-строку:

```javascript
Вывести array как hex-строку: byte[3412341241]:
const ret = this.fn();
const buffer = Java.array('byte', ret);
console.log(buffer.length);
const result = "";
for(let i = 0; i < buffer.length; ++i){
    result += ('0' + (buffer[i] & 0xff).toString(16)).slice(-2);
}
console.log(result);
```

#### Вывести объект

```javascript
console.log(`${JSON.stringify(someObject, null, 2)}`)
```

## Wrapper

```javascript
console.log("[+] Start");
Java.perform(() => {
    try {
        // Hook
        const someClass = Java.use('some.class.Class')
        const someClassObject = someClass.$new(arg1, arg2, ..)
        const retval = someClassObject.some_method()
        
        console.log(`${JSON.stringify(retval, null, 2)}`)
    } catch(err) {
        console.log("[-] Fail");
    }
}
```

### PhoneGap & Outsystem ssl pinning bypass

src: [https://github.com/clviper/android/blob/master/pinning.js](https://github.com/clviper/android/blob/master/pinning.js)

### OkHttp3 SSL Pinning bypass

```javascript
Java.perform(() => {
    try {
        console.log("[+] Start 2");
        const OkHttpClient_Builder = Java.use('okhttp3.OkHttpClient$Builder');
        console.log("[+] OkHTTP 3.x Found");

        // Disable adding CertificatePinner Objects
        
        OkHttpClient_Builder.certificatePinner.overload('okhttp3.CertificatePinner').implementation = function(certificatePinner) {
            console.log("[+] OkHTTP 3.x certificatePinner() called. Not throwing an exception.");
            return this;
        };

        OkHttpClient_Builder.hostnameVerifier.overload('javax.net.ssl.HostnameVerifier').implementation = function(hostnameVerifier) {
            console.log("[+] OkHTTP 3.x hostnameVerifier() called. Not throwing an exception.");
            return this;
        };


        // Disable CertificatePinner checks
        
        OkHttpClient_Builder.certificatePinner.overload('okhttp3.CertificatePinner').implementation = function(certificatePinner) {
            console.log("[+] OkHTTP 3.x certificatePinner() called. Not throwing an exception.");
            return this;
        };

        const CertificatePinner = Java.use('okhttp3.CertificatePinner');
        CertificatePinner.check.overload('java.lang.String', 'java.util.List').implementation = function(s, l) {
            console.log("[+] Unpinning Type 1: " + s);
            return true;
        };

        CertificatePinner.check.overload('java.lang.String', 'java.security.cert.Certificate').implementation = function(s, cert) {
            console.log("[+] Unpinning Type 2: " + s);
            return true;
        }

        CertificatePinner.check.overload('java.lang.String', 'javax.security.cert.Certificate').implementation = function(s, cert) {
            console.log("[+] Unpinning Type 3: " + s);
            return true;
        }
    } catch (err) {
        console.log("[-] OkHTTP 3.x Not Found")
    }
})
```





