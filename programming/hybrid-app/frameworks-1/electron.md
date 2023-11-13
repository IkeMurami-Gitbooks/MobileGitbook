# Electron

Этот фреймворк часто используют для написания приложений под Desktop (Windows / MacOS)

## Security

### Signature

Проверка целостности библиотек стоит важным вопросом тут, чтобы злоумышленник не смог исполнить код от лица нашего приложения (так как приложение подписано нашей подписью).

* Само приложение и установщик подписывается сертификатом разработчика, OS не даст установить или запустить приложение без подписи (или по крайней мере будет противиться)
* Подпись и проверка целостности исполняемых файлов-зависимостей (dll, exe): через флаг signDll (подробнее: [https://www.electron.build/configuration/win](https://www.electron.build/configuration/win))
* Отдельно стоит проверка целостности asar-архива (тут js и статика): macos — поддержка есть [https://www.electronjs.org/docs/latest/tutorial/asar-integrity#toggling-the-fuse](https://www.electronjs.org/docs/latest/tutorial/asar-integrity#toggling-the-fuse), win — пока нет [https://github.com/electron/electron/pull/40504](https://github.com/electron/electron/pull/40504).
