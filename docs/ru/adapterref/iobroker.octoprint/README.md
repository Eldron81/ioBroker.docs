---
BADGE-NPM version: https://img.shields.io/npm/v/iobroker.octoprint?style=flat-square
BADGE-Downloads: https://img.shields.io/npm/dm/iobroker.octoprint?label=npm%20downloads&style=flat-square
BADGE-Snyk Vulnerabilities for npm package: https://img.shields.io/snyk/vulnerabilities/npm/iobroker.octoprint?label=npm%20vulnerabilities&style=flat-square
BADGE-node-lts: https://img.shields.io/node/v-lts/iobroker.octoprint?style=flat-square
BADGE-Libraries.io dependency status for latest release: https://img.shields.io/librariesio/release/npm/iobroker.octoprint?label=npm%20dependencies&style=flat-square
BADGE-GitHub: https://img.shields.io/github/license/klein0r/iobroker.octoprint?style=flat-square
BADGE-GitHub repo size: https://img.shields.io/github/repo-size/klein0r/iobroker.octoprint?logo=github&style=flat-square
BADGE-GitHub commit activity: https://img.shields.io/github/commit-activity/m/klein0r/iobroker.octoprint?logo=github&style=flat-square
BADGE-GitHub last commit: https://img.shields.io/github/last-commit/klein0r/iobroker.octoprint?logo=github&style=flat-square
BADGE-GitHub issues: https://img.shields.io/github/issues/klein0r/iobroker.octoprint?logo=github&style=flat-square
BADGE-GitHub Workflow Status: https://img.shields.io/github/workflow/status/klein0r/iobroker.octoprint/Test%20and%20Release?label=Test%20and%20Release&logo=github&style=flat-square
BADGE-Snyk Vulnerabilities for GitHub Repo: https://img.shields.io/snyk/vulnerabilities/github/klein0r/iobroker.octoprint?label=repo%20vulnerabilities&logo=github&style=flat-square
BADGE-Beta: https://img.shields.io/npm/v/iobroker.octoprint.svg?color=red&label=beta
BADGE-Stable: http://iobroker.live/badges/octoprint-stable.svg
BADGE-Installed: http://iobroker.live/badges/octoprint-installed.svg
translatedFrom: en
translatedWarning: Если вы хотите отредактировать этот документ, удалите поле «translationFrom», в противном случае этот документ будет снова автоматически переведен
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/ru/adapterref/iobroker.octoprint/README.md
title: ioBroker.octoprint
hash: OI5uBS9Qmu79/syGAjVHZyQect9T5Nm8oG5INh1nd6U=
---
![Логотип](../../../en/adapterref/iobroker.octoprint/../../admin/octoprint.png)

# IoBroker.octoprint
**Протестировано с [Октопринт](https://github.com/OctoPrint/OctoPrint/releases) 1.8.6**

## Функции
### Информация
- Получить информацию о версии
- Получить информацию о принтере (когда «работает»)
- Получить текущую информацию о задании на печать (при ``печати``)
- Получить информацию о списке файлов (когда не ``печать``)

### Инструменты
- Установите температуру инструмента (когда «работает»)
- Установите температуру кровати (когда «работает»)
- Выдавливание/Втягивание (когда "работает")

### Команды
- Принтер: подключить, отключить и домой
- Работа: запуск, пауза, возобновление, отмена, перезапуск
- SD-карта: инициализация, обновление, выпуск
- Пользовательские команды принтера
- Системные команды
- Перемещение по осям X, Y и Z
- Выберите файл или распечатайте его

### Поддерживаемые плагины
- [Отображение хода выполнения слоя] (https://github.com/OllisGit/OctoPrint-DisplayLayerProgress) - протестировано с версией 1.28.0 (требуется **адаптер версии 2.1.0** или новее)
- [Slicer Thumbnails] (https://github.com/jneilliii/OctoPrint-PrusaSlicerThumbnails) - протестировано с версией 1.0.0 (требуется **адаптер версии 2.2.0** или выше)

## Важный!
НЕ перезапускайте свой экземпляр OctoPrint (или любой другой экземпляр) с таким кодом:

```javascript
var obj = getObject('system.adapter.octoprint.0');
obj.common.enabled = false;
setObject('system.adapter.octoprint.0', obj);
```

Поскольку `API key` является защищенным атрибутом, начиная с версии 1.1.0, настроенный ключ API будет удален. Причина в том, что `getObject` не возвращает защищенную информацию (поэтому ключ API не включается в возвращаемый объект). Когда вы сохраняете объект, вы сохраняете объект без ключа.

Используйте состояние `system.adapter.octoprint.0.alive`, чтобы остановить или запустить экземпляр.

## Changelog

<!--
  Placeholder for the next version (at the beginning of the line):
  ### **WORK IN PROGRESS**
-->
### **WORK IN PROGRESS**

Tested with OctoPrint 1.8.6

* (klein0r) Dropped Admin 5 support

### 4.0.1 (2022-10-14)

Tested with OctoPrint 1.8.4

* (klein0r) Just download every thumbnail once (requires plugin Slicer Thumbnails)

### 4.0.0 (2022-05-19)

NodeJS 14.x is required (NodeJS 12.x is EOL)

Tested with OctoPrint 1.8.0

* (klein0r) Added last and average layer duration (requires plugin Display Layer Progress)
* (klein0r) Moved thumbnail information of files to new structure **(BREAKING CHANGE - CHECK YOUR SCRIPTS AND VIS)**
* (klein0r) Improved handling of thumbnails and states for plugins

### 3.2.2 (2022-04-29)

* (klein0r) Updated depedency for js-controller to 4.0.15

### 3.2.1 (2022-04-28)

* (klein0r) Get thumbnail url of current file and copy value to printjob (requires plugin Slicer Thumbnails)
* (klein0r) Updated log messages

### 3.2.0 (2022-03-07)

Tested with OctoPrint 1.7.3

* (klein0r) Added print times as readable states (seconds to string)
* (klein0r) Added formatted date when print job will finish
* (klein0r) Added fan speed and feedrate from plugin Display Layer Progress

## License

The MIT License (MIT)

Copyright (c) 2022 Matthias Kleine <info@haus-automatisierung.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.