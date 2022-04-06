# iOS

## Common

### JS API

Получение объекта ios:

```
const webSocket = new ObjC.Object(args[2]);
```

Свойства объекта в Frida JS API:

```
$className - имя класса
$kind = {instance, class, meta-class}
$super - super-class method impl
$superClass - super-class as ObjC.Object instance
$class - class this object as ...
$methods - методы текущего и родительского классов
$ownMethods - методы текущего класса
$ivars - название полей
```

Доступ к приватным полям

```
for (k in objectClass.$ivars) {
    log("Test " + k);
    // out:
    // Test _test_property
}
prop = objectClass.$ivars._test_property
```

NSData

```
// NSData -> String:
const data = new ObjC.Object(args[2]);
Memory.readUtf8String(data.bytes(), data.length());

// NSData -> Binary Data
const data = new ObjC.Object(args[2]);
Memory.readByteArray(data.bytes(), data.length());
```

NSArray

```
// Enumeration by NSArray
const array = new ObjC.Object(args[2]);
const count = array.count().valueOf();

for (let i = 0; i !== count; i++) {
  const element = array.objectAtIndex_(i);
}
```

NSDictionary

```
const dict = new ObjC.Object(args[2]);
const enumerator = dict.keyEnumerator();
const key;
while ((key = enumerator.nextObject()) !== null) {
    const value = dict.objectForKey_(key);
}
```

Structs

If args\[0] is a pointer to a struct, and let’s say you want to read the uint32 at offset 4, you can do it as shown below:

```
Memory.readU32(args[0].add(4));
```

### Helpful functions

```javascript
function prints(s) {
    console.log(s);
}

function nsstring(s) {
    const { NSString } = ObjC.classes
    return NSString.stringWithString_(s)
}

function GetModules() {
    let modules = Process.enumerateModules();
    modules.forEach(function(item) {
        if (!item.path.includes("/System/Library") && !item.path.includes("/usr/lib")) {

            // prints(`Name: ${item.name}; Path: ${item.path}`);
        }
    });

    return modules;
}

function GetModule(name) {
    let modules = GetModules();

    return modules.filter(function(item) {
        return item.name == name
    })[0];
}

function ModuleExports(module) {

    let exports = module.enumerateExports();
    let funcs = exports.filter(function(item) {
        return item.type === 'function'
    });
    let vars = exports.filter(function(item) {
        return item.type === 'variable'
    });

    return {
        'funcs': funcs,
        'vars': vars
    }
}

function VarsTest(vars) {
    vars.forEach(function(item) {
        // prints(`Name: ${item.name}`);
    })
}

function GetVariableByName(vars, name) {
    return vars.filter(function(item) {
        return item.name === name
    })[0];
}
```

## Swift

### Swift Support

Начиная с 15.0.0 frida поддерживает swift.&#x20;

Документация: [https://github.com/frida/frida-swift-bridge/blob/master/docs/api.md](https://github.com/frida/frida-swift-bridge/blob/master/docs/api.md)

```
$ frida -U MyApp
> Swift.available
true
```

### Swift example

```javascript
console.log("[+] Plugin started");
if (Swift.available) {
    try {
        console.log("[+] Swift Runtime Available");

        let modules = GetModules();  // См выше эту функцию, в разд Common
        let messProtoModule = GetModule("MessProto");  // См выше эту функцию, в разд Common
        let exports = ModuleExports(messProtoModule);  // См выше эту функцию, в разд Common

        let verifyCodeRequestInit = GetVariableByName(exports.vars, '$s9MessProto0B17VerifyCodeRequestVACycfC');  // См выше эту функцию, в разд Common
        // prints(`Name: ${verifyCodeRequestInit.name}, Address: ${verifyCodeRequestInit.address}`)

        let obj = new Swift.Object(verifyCodeRequestInit.address);
    }
    catch(err) {
        console.log("[!] Exception: " + err.message);
    }
}
else {
    console.log("Swift Runtime is not available!");
}


```

## ObjC

### Alamofire SSL Pinning Disable

```javascript
// frida -U -l afnetwork_sslbypass.js --no-pause -f my.test.app
 
console.log("[*] Started: SSL Unpinning");
if (ObjC.available) {
    /* AFNetwork */
    if (ObjC.classes.AFSecurityPolicy) {
        Interceptor.attach(ObjC.classes.AFSecurityPolicy['- setSSLPinningMode:'].implementation, {
            onEnter: function (args) {
                send("[*] Replacing AFSecurityPolicy setSSLPinningMode = 0 was " + args[2]);
                args[2] = ptr('0x0');
            }
        });
        Interceptor.attach(ObjC.classes.AFSecurityPolicy['- setAllowInvalidCertificates:'].implementation, {
            onEnter: function (args) {
                send("[*] Replacing AFSecurityPolicy setAllowInvalidCertificates = 1 was " + args[2]);
                args[2] = ptr('0x1');
            }
        });   
    }
}
else
{
    console.log("Objective-C Runtime is not available!");
}
console.log("[*] Completed: SSL Unpinning");
```

### ASLR Bypass: Получение адреса функции

```javascript
var modules_dict = {};

// save module addresses for bypass ASLR on iOS
function get_modules_dict() {
    var native_modules = Process.enumerateModulesSync();
    var i;
    for (i = 0; i < native_modules.length; i++)
    {
        modules_dict[native_modules[i].path] = {name: native_modules[i].name, path: native_modules[i].path, base: parseInt(native_modules[i].base.toString(), 16), size: parseInt(native_modules[i].size, 10), slide: -1, min: -1, max: -1};
    }

    // unsigned long _dyld_image_count();
    var dyldImageCountPtr = Module.findExportByName("libSystem.B.dylib", "_dyld_image_count");
    var dyldImageCount = new NativeFunction(dyldImageCountPtr, 'int', []);

    // unsigned long _dyld_get_image_vmaddr_slide(unsigned long image_index);
    var dyldImageVMAddrSlidePtr = Module.findExportByName("libSystem.B.dylib", "_dyld_get_image_vmaddr_slide");
    var dyldImageVMAddrSlide = new NativeFunction(dyldImageVMAddrSlidePtr, 'ulong', ['ulong']);

    //pointer _dyld_get_image_name(ulong);
    var dyldImageNamePtr = Module.findExportByName("libSystem.B.dylib", "_dyld_get_image_name");
    var dyldImageName = new NativeFunction(dyldImageNamePtr, 'pointer', ['ulong']);

    var count = dyldImageCount();
    for (i=0; i<count; i++)
    {
        var imgName = Memory.readCString(dyldImageName(i));
        var imgAddr = dyldImageVMAddrSlide(i);
        modules_dict[imgName]['slide'] = parseInt(imgAddr.toString(), 10);
        modules_dict[imgName]['min'] = modules_dict[imgName]['base'];
        modules_dict[imgName]['max'] = modules_dict[imgName]['base'] + modules_dict[imgName]['size'];
    }
    return modules_dict;
}
// Get original address for iOS binary
function find_module_offset_by_addr(orig_addr, lookup_func_name)
{
    var mp;
    for (mp in modules_dict)
    {
        if (orig_addr >= modules_dict[mp]['min'] && orig_addr <= modules_dict[mp]['max'])
        {
            var func_name = '';
            if (lookup_func_name === true)
            {
                func_name = " " + DebugSymbol.fromAddress(orig_addr)['name'];
            }
            var dbg_str = " (B:" + modules_dict[mp]['base'] + "|S:" + modules_dict[mp]['slide'] + "|O:" + orig_addr + ")";
            return orig_addr.sub(modules_dict[mp]['slide']) + " " + modules_dict[mp]['name'] + "+" + (orig_addr - modules_dict[mp]['min']).toString(16) + func_name; // + dbg_str;
        }
    }
    return "[DBGS]" + DebugSymbol.fromAddress(orig_addr);
    //return "UNKNOWN!" + orig_addr
}

/*
* Example usage
*/

if (ObjC.available) {	

   var module = "SDSearch";
   var functionName = "sbf_wasabit";

   var sbf_wasabit_ptr = Module.findExportByName(module, functionName);

   modules_dict = get_modules_dict();

   console.log("Wasabit module offset: " + find_module_offset_by_addr(sbf_wasabit_ptr)); // По полученному адресу можно осуществлять поиск в IDA
}
```
