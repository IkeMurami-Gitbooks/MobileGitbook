# Android WebView

## Intro

## Security

### Improper usage WebView

Есть два sandbox bypass:&#x20;

#### JS может вызывать Java код

```javascript
wv.addJavascriptInterface(new FileUtils(), “file”);
<script>
filename = ‘/data/data/com.Foudnstone/data.txt’;
file.write(filename, data, false);
</script>
```

У класса, который подключается подобным образом, должен быть реализован интерфейс  JavascriptInterface

#### Java код может вызывать JS

```java
String javascr = “javascript: var newscript=document.
createElement(\”script\”);”;
javascr += “newscript.src=\”http://www.foundstone.com\”;”;
javascr += “document.body.appendChild(newscript);”;
myWebView.loadUrl(javascr);
```

Что безопаснее WebView или Chrome Custom Tabs: [https://developer.chrome.com/multidevice/android/customtabs](https://developer.chrome.com/multidevice/android/customtabs)

### Разрешения

`setJavaScriptEnabled(true)` — можем исполнять JS-код

`setAllowFileAccess(true)` — можем читать файлы через схему `file:///data/data/`



### Что еще можно проверить / попробовать, если мы можем влиять на WebView

oversecured's paper: [https://blog.oversecured.com/Android-security-checklist-webview/](https://blog.oversecured.com/Android-security-checklist-webview/)

### Какие методы хукать

```
$ objection --gadget app.example.com explore
objection #  android hooking watch class_method android.webkit.WebView.loadData --dump-args
objection #  android hooking watch class_method android.webkit.WebView.loadUrl --dump-args
objection #  android hooking watch class_method android.webkit.WebView.loadDataWithBaseURL --dump-args
objection #  android hooking watch class_method android.webkit.WebView.evaluateJavascript --dump-args
objection #  android hooking watch class_method android.webkit.WebView.getUrl --dump-args
```
