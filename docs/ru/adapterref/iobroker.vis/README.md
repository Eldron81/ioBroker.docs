---
translatedFrom: en
translatedWarning: Если вы хотите отредактировать этот документ, удалите поле «translationFrom», в противном случае этот документ будет снова автоматически переведен
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/ru/adapterref/iobroker.vis/README.md
title: Визуализация
hash: 5Zpnh5/bkkTaBytYaq0YgY6eUXzS908Utosymsl4Nbs=
---
![Логотип](../../../en/adapterref/iobroker.vis/admin/vis.png)

![Количество установок](http://iobroker.live/badges/vis-stable.svg)
![версия NPM](http://img.shields.io/npm/v/iobroker.vis.svg)
![Загрузки](https://img.shields.io/npm/dm/iobroker.vis.svg)
![НПМ](https://nodei.co/npm/iobroker.vis.png?downloads=true)

# Визуализация
WEB-визуализация для платформы ioBroker.

## Установка и документация
![Демонстрационный интерфейс](img/user0.png) ![Демонстрационный интерфейс](../../../en/adapterref/iobroker.vis/img/user7.png)

[Онлайн-демонстрации](https://iobroker.click/)

## Привязки объектов
Обычно большинство виджетов имеют атрибут ObjectID, и этот атрибут может быть связан с некоторым значением идентификатора объекта.
Но есть и другой вариант, как привязать *любой* атрибут виджета к какому-то ObjectID.

Просто напишите в атрибут ```{object.id}```, и он будет привязан (не в режиме редактирования) к значению этого объекта.
Если вы используете специальный формат, вы даже можете выполнять с ним некоторые простые операции, например. умножение или форматирование.
Патен имеет следующий формат:

```
{objectID;operation1;operation2;...}
```

Поддерживаются следующие операции:

- `\*` - умножение. Аргумент должен быть заключен в скобки, например "*(4)". В этом примере мы умножаем значение на 4.
- `\+` - доп. Аргумент должен быть в скобках, например "+(4.5)". В этом примере мы добавляем к значению 4,5.
- `\-` - вычесть. Аргумент должен быть заключен в скобки, например "-(-674,5)". В этом примере мы вычитаем из значения -674,5.
- `/` - деление. Аргумент должен быть в скобках, например "/(0,5)". В этом примере мы делим значение на 0,5.
- `%` - по модулю. Аргумент должен быть заключен в скобки, например "%(5)". В этом примере мы берем по модулю 5.
- `round` - округлить значение.
- `round(N)` - округлить значение до N знаков после запятой, например 34,678;округление(1) => 34,7
- `hex` - преобразовать значение в шестнадцатеричное значение. Все буквы строчные.
- `hex2` - преобразовать значение в шестнадцатеричное значение. Все буквы строчные. Если значение меньше 16, то будет добавлен начальный ноль.
- `HEX` - то же, что и шестнадцатеричный, но в верхнем регистре.
- `HEX2` - то же, что и hex2, но в верхнем регистре.
- `date` - форматировать дату в соответствии с заданным форматом. Формат такой же, как в [iobroker.javascript](https://github.com/iobroker/iobroker.javascript/blob/master/README.md#formatdate)
- `min(N)` - если значение меньше N, взять N, иначе значение
- `max(M)` - если значение больше M, взять M, иначе значение
- `sqrt` - квадратный корень
- `pow(n)` - степень числа N.
- `pow` - степень числа 2.
- `пол` - Math.floor
- `ceil` - Math.ceil
- `random(R)` - Math.random() * R или просто Math.random(), если нет аргумента
- `formatValue(decimals)` - форматировать значение в соответствии с настройками системы и использовать десятичные дроби
- `date(format)` - форматировать значение как дату. Формат такой: "ГГГГ-ММ-ДД чч:мм:сс.сссс"
- `momentDate(format, useTodayOrYesterday)` - отформатируйте значение как дату, используя Moment.js. [Утвержденные форматы должны быть введены в соответствии с библиотекой moment.js](https://momentjs.com/docs/#/displaying/format/). При `useTodayOrYesterday=true` формат моментов `ddd`/`dddd` перезаписывается сегодня/вчера
- `array(element1,element2[,element3,element4])` - возвращает элемент индекса. например: `{id.ack;array(ack is false,ack is true)}`

Вы можете использовать этот шаблон в любом тексте, например

```
My calculations with {objectID1;operation1;operation2;...} are {objectID2;operation3;operation4;...}
```

или цветовые расчеты:

```
#{objectRed;/(100);*(255);HEX2}{objectGreen;HEX2}{objectBlue;HEX2}
```

Чтобы показать временную метку объекта, напишите `.ts` или `.lc` (для последнего изменения) в конце идентификатора объекта, например:

```
Last change: {objectRed.lc;date(hh:mm)}
```

Существует еще одна возможность написать шаблон:

```
Hypotenuse of {height} and {width} = {h:height;w:width;Math.max(20, Math.sqrt(h*h + w*w))}
```

`{h:height;w:width;h*w}` будет интерпретироваться как функция:

```
value = (function () {
    var h = "10";
    var w = "20";
    return Math.max(20, Math.sqrt(h*h + w*w));
})();
```

Вы можете использовать *любые* функции javascript. Аргументы должны быть определены с помощью ':', в противном случае это будет интерпретироваться как формула.

Позаботьтесь о типах. Все они определены как строки. Чтобы быть уверенным, что это значение будет рассматриваться как число, используйте функцию parseFloat.

```
Hypotenuse of {height} and {width} = {h:height;w:width;Math.max(20, Math.sqrt(Math.pow(parseFloat(h), 2) + Math.pow(parseFloat(w), 2)))}
```

### Специальные привязки
Существует ряд различных внутренних привязок для предоставления дополнительной информации в представлениях:

* `username` - показывает вошедшего в систему пользователя
* `view` - имя фактического вида
* `wname` - имя виджета
* `виджет` - это объект со всеми данными виджета. Может использоваться только в части JS, например `{a:a;widget.data.name}`
* `wid` - имя актуального виджета
* `язык` - может быть `de`, `en` или `ru`.
* `instance` - экземпляр браузера
* `login` - требуется ли вход в систему или нет (например, чтобы показать/скрыть кнопку выхода)
* `local_*` - если имя состояния начинается с `local_`, оно не будет сообщаться ioBroker, но будет обновлять все виджеты, зависящие от этого состояния. (Локальная переменная для текущего сеанса браузера)

Примечание: чтобы использовать ":" в вычислениях (например, в строковой формуле), используйте вместо этого "::".

**Помните**, что определения стилей будут интерпретироваться как привязки, поэтому используйте `{{style: value}}` или просто

```
{
	style: value
}
```

для этого.

## Фильтры
Чтобы визуализировать в одном представлении все количество виджетов, вы можете использовать фильтры, чтобы уменьшить количество виджетов, одновременно отображаемых в представлении.

В каждом виджете есть поле `filter`. Если вы установите для него какое-то значение, например. `light`, поэтому вы можете использовать другой виджет `(bars - filters, filter - dropdown)`, чтобы контролировать, какой фильтр фактически активен.

## Интерфейс управления
Vis создает 3 переменные:

- `control.instance` – здесь должен быть указан экземпляр браузера или `FFFFFFFF`, если необходимо контролировать каждый браузер.
- `control.data` - Параметр для команды. См. описание конкретной команды.
- `control.command` - Имя команды. Запись этой переменной запускает команду. Это означает, что до того, как команда будет записана, «экземпляр» и «данные» должны быть подготовлены с данными.

Команды:

* `alert` - показывать окно алерта в вис. «control.data» имеет следующий формат: «сообщение; заголовок; значок jquery». Заголовок и значок jquery являются необязательными. Названия иконок можно найти [здесь](http://jqueryui.com/themeroller/). Чтобы отобразить значок "ui-icon-info", напишите ```Message;;info```.
* `changeView` - переключиться на нужный вид. "control.data" должно иметь имя представления. Вы также можете указать имя проекта как «проект/представление». Проект по умолчанию "основной".
* `refresh` - перезагрузить вид, например, после изменения проекта для перезагрузки во всех браузерах.
* `reload` - то же, что и обновление.
* `dialog` - Показать диалоговое окно. Диалог должен существовать в поле зрения. Один из:

    - `статический - HTML - Диалог`,
    - `статические - Иконка - Диалог`,
    - `контейнер - HTML - просмотр в диалоговом окне jqui`,
    - `контейнер - ext cmd - просмотр в диалоговом окне jqui`,
    - `контейнер - иконка - просмотр в диалоговом окне jqui`,
    - `Контейнер - Кнопка - просмотр в диалоговом окне jqui`.

    `control.data` должен иметь идентификатор виджета диалога, например. `w00056`.

* `диалогЗакрыть`
* `popup` - открывает новое окно браузера. Ссылка должна быть указана в `control.data`, например. http://google.com
* `playSound` - воспроизвести звуковой файл. Ссылка на файл указана в `control.data`, например http://www.modular-planet.de/fx/marsians/Marsiansrev.mp3.

  Вы можете загрузить свой собственный файл в vis и позволить ему воспроизводиться, например, `/vis.0/main/img/myFile.mp3`.

Если пользователь изменит представление или при запуске переменные будут заполнены визуализацией с

- `control.instance`: экземпляр браузера и `ack=true`
- `control.data`: имя проекта и представления в форме `проект/представление`, например. `main/view` (и `ack=true`)
- `control.command`: `changedView` и `ack=true`

Вы можете записать строку JSON или объект в `control.command` как `{instance: 'AABBCCDD', command: 'cmd', data: 'ddd'}`. В этом случае экземпляр и данные будут взяты из объекта JSON.

Пример для javascript-адаптера:

```
setState('vis.0.control.command', {"instance": "*", "command": "refresh", "data": ""});
```

## Вид по умолчанию
Вы можете определить для каждого вида желаемое разрешение (Меню=>Инструменты=>Разрешение). Это только визуальная граница в режиме редактирования, чтобы показать вам размер экрана на каком-то конкретном устройстве. В режиме реального времени его не будет видно и будут видны все виджеты за пределами границы.

Кроме того, вы можете определить, должно ли это представление использоваться по умолчанию для этого разрешения.

Таким образом, каждый раз, когда вызывается `index.html` (без `#viewName`), будет открываться вид, наиболее подходящий для этого разрешения.
Если только один вид имеет флаг *"По умолчанию"*, этот вид будет открыт независимо от разрешения или ориентации экрана.

Например. вы можете создать два вида: «Пейзаж-мобильный» и «Портрет-мобильный», и эти два вида будут автоматически переключаться при изменении ориентации или размера экрана.

Существует вспомогательный виджет «Основной — Разрешение экрана», который показывает фактическое разрешение экрана и наиболее подходящий вид по умолчанию для этого разрешения.

## Настройки
### Перезагрузить, если спать дольше, чем
Существует правило, что через некоторое время отключения вся страница ВИС будет перезагружена для синхронизации проекта.
Настроить его можно в меню «Настройки…». Если вы установите интервал «никогда», страница никогда не будет перезагружена.

### Интервал переподключения
Установите интервал между попытками подключения при отключении. Если вы установите 2 секунды, он будет пытаться установить соединение каждые 2 секунды.

### Темный экран переподключения
Иногда (ночью) требуется темный экран загрузки. С помощью этой опции вы можете установить его.

Обратите внимание, что эти настройки действительны только для повторного подключения, а не для первого подключения.

![Темный](../../../en/adapterref/iobroker.vis/img/dark_screen.png)

<!-- Заполнитель для следующей версии (в начале строки):

### **ВЫПОЛНЯЕТСЯ** -->

## Changelog
### 1.4.13 (2022-03-22)
* (pascal-hari) The group attributes will be replaced recursively

### 1.4.12 (2022-02-12)
* (bluefox) Fixed bug with the export of specific widgets

### 1.4.11 (2022-02-12)
* (bluefox) Removed ES6 commands to be supported by older browsers
* (oweitman) Fixed problem with update of grouped widgets
* (stephanritscher) Added web manifest to be able to handle vis as the web app

### 1.4.8 (2022-01-29)
* (bluefox) Added support for js-controller 4.x

### 1.4.7 (2021-12-28)
* (mswiege) Fixed subscribeOidAtRuntime if called with invalid IDs
* (bluefox) Added support of license manager

### 1.4.6 (2021-11-20)
* (bluefox) Added the license check even without internet

### 1.4.5 (2021-10-08)
* (jens-maus) Added frame-src spec to content-security-policy header fixing frame related content blocking issues (e.g. using KioskPro iOS app).
* (bluefox) Showed the extended error messages by license check
* (Scrounger) Patch visibility oid binding

### 1.4.4 (2021-08-31)
* (jobe451) Allowed to have ":" in the binding object IDs

### 1.4.3 (2021-07-11)
* (bluefox) Added possibility to check license offline (only special once)

### 1.4.0 (2021-07-01)
* (bluefox) Changed path for check of certificates 
* (thost96) fixes for issues found by adapter-checker

### 1.3.10 (2021-05-25)
* (bluefox) Fixed the support of admin5

### 1.3.9 (2021-04-29)
* (agav99) Added support of local browser variables
* (Scrounger) Bug fix for null & NaN values in width and height
* (bluefox) Added support for admin5

### 1.3.8 (2021-03-03)
* (bluefox) fix play sounds on iOS Safari an android
* (Scrounger) visEditInspect: format dimension added
* (foxriver76) Replace travis and appveyor by the GitHub actions
* (Excodibur) Allow resources to be loaded as blob
* (Excodibur ) Allow resources to be loaded as blob

### 1.3.7 (2021-01-20)
* (Scrounger) Bug Fixed - Binding in JSON string

### 1.3.6 (2020-12-13)
* (twonky4) Corrected: old browser issue
* (rbaranga) Corrected: play sounds on iOS Safari
* (Scrounger) Added the optional arguments to support Material Design Widgets

### 1.3.4 (2020-10-04)
* (foxriver76) Corrected the error on older devices

### 1.3.3 (2020-09-21)
* (bluefox) Return de-bounce settings back
* (bluefox) Corrected error with {username} binding
* (bluefox) Fixed "show last change" option

### 1.3.1 (2020-09-18)
* (bluefox) Added the auto-focus option to the input widgets

### 1.3.0 (2020-09-17)
* (foxriver76) on pending getStates, try again instead of drop
* (foxriver76) fixed the file manager typos
* (Scrounger) Added momentDate for the bindings

### 1.2.12 (2020-09-08)
* (foxriver76) only parse arrays and json objects, not booleans, normal strings etc

### 1.2.11 (2020-08-25)
* (bluefox) The error message about the non-found chart view was fixed.

### 1.2.10 (2020-08-23)
* (gsicilia82/fceller) JSON strings will be parsed in VIS bindings

### 1.2.9 (2020-08-22)
* (bluefox) Charts are now supported

### 1.2.6 (2020-03-22)
* (bluefox) Added the better error message if license could not be parsed

### 1.2.4 (2020-02-11)
* (bluefox) Table widget was extended with the selected object ID.

### 1.2.3 (2019-12-14)
* (bluefox) Small changes in license handling were made

### 1.2.2 (2019-10-27)
* (bluefox) Preparations for js-controller 2.0. Check undefined adn null.

### 1.2.1 (2019-09-10)
* (bluefox) fixed upload of files

### 1.2.0 (2019-05-07)
* (bluefox) add translations

### 1.1.11 (2019-02-07)
* (bluefox) improve Bool HTML

### 1.1.10 (2019-01-30)
* Add Chinese support

### 1.1.8 (2018-10-29)
* (bluefox) File dialog was corrected

### 1.1.7 (2018-07-24)
* (bluefox) view8 corrected

### 1.1.6 (2018-07-18)
* (bluefox) support of new variables (see [Special bindings](#special-bindings) )
* (bluefox) fix error if fast view changes
* (bluefox) fix "jqui - ctrl - IconState / val - Icon Bool"

### 1.1.5 (2018-06-10)
* (bluefox) show more information if widget cannot be rendered
* (bluefox) fix saving of widgets if they have bindings
* (bluefox) show error stack
* (bluefox) fix binding
* (Apollon77) fix testing
* (bluefox) fix for iobroker.pro and external socket.io settings
* (bluefox) A user variable was added into bindings.
* (bluefox) Fixed widget tabs

### 1.1.4 (2018-04-23)
* (bluefox) fix bool SVG

### 1.1.3 (2018-04-12)
* (bluefox) ignore click by scrolling on touch devices
* (bluefox) remove wrong state vis.0.command
* (bluefox) fix error with jPlot
* (bluefox) better widget behaviour in edit Mode (basic, jqui)
* Fix config dialog

### 1.1.2 (2018-02-02)
* (bluefox) Fixing the saving of project
* (bluefox) Fixing the background selector
* (bluefox) Fixing the null pointer problem
* (bluefox) Fixing the selection helper
* Update translations

### 1.1.1 (2018-01-07)
* (bluefox) The problem with view change on the touch devices fixed

### 1.0.5 (2017-11-19)
* (bluefox) show number of data points in every project

### 1.0.4 (2017-10-22)
* (bluefox) Add autocomplete for view CSS options
* (bluefox) change edit of view CSS background options

### 1.0.3 (2017-10-20)
* (bluefox) Fix parse of invalid bindings
* (bluefox) add moment.js

### 1.0.0 release candidate (2017-10-13)
* (bluefox) fix iframe and image updates
* (bluefox) fix fonts

### 0.15.7 (2017-10-01)
* (bluefox) allow update of images without additional query (but it works only in some very specific cases)
* (bluefox) zoom of iframes

### 0.15.5 (2017-07-24)
* (bluefox) Fix widgets upload

### 0.15.4 (2017-07-19)
* (bluefox) Add swipe

### 0.15.3 (2017-07-12)
* (bluefox) Add full screen widget
* (bluefox) Fix timestamp widget

### 0.15.2 (2017-07-07)
* (bluefox) Fix binding if it has "-" in the OID

### 0.15.1 (2017-06-30)
* (bluefox) Fix error with context menu
* (bluefox) Allow adding of class to view

### 0.15.0 (2017-05-25)
* (bluefox) fix copy of grouped widgets
* (bluefox) fix subscribe if empty states

### 0.14.7 (2017-05-19)
* (bluefox) add templates

### 0.14.6 (2017-05-16)
* (bluefox) Fix error by groups selection
* (apollon77) fix jqui-dialog for auto-open

### 0.14.3 (2017-05-11)
* (bluefox) fix export/import of grouped widgets

### 0.14.2 (2017-04-29)
* (bluefox) Fix install error

### 0.14.1 (2017-04-27)
* (bluefox) move beta to main
* (bluefox) fix choose filter
* (bluefox) fix error if some views do not exist
* (bluefox) fix binding problem, e.g. "a:-45?0" was detected as variable too.
* (bluefox) fix some font sizes
* (bluefox) fix undo
* (bluefox) fix themes change
* (bluefox) optimize load of pages
* (bluefox) check license
* (bluefox) fix basic views 8
* (bluefox) fix time picker if opened in dialog

### 0.14.0 (2017-04-10)
* (bluefox) add mandatory license input

### 0.12.7 (2017-02-09)
* (bluefox) prepare beta

### 0.12.6 (2017-01-29)
* (pmant) fix view copy
* (pmant) Improvements to context menu
* (pmant) usability improvements for both view dropdowns
* (bluefox) small fix of dragging

### 0.12.6 (2017-01-29)
* (pmant) add dropdown menu to views bar
* (pmant) sort widgets widget selector by name
* (bluefox) fix groupAttr in signals and visibility

### 0.12.2 (2016-12-04)
* (bluefox) fix errors with grouping

### 0.12.1 (2016-11-30)
* (bluefox) fix errors with containers

### 0.12.0 (2016-11-24)
* (bluefox) subscribe mode for faster state loading
* (bluefox) add grouping

### 0.10.15 (2016-11-06)
* (bluefox) remove weather-adapter.html
* (bluefox) clean config.js
* (bluefox) remove old widgets
* (bluefox) improve authentication in app
* (bluefox) allow creation of instance from helper widget

### 0.10.14 (2016-10-09)
* (bluefox) fix rendering of widgets
* (bluefox) working on relative positions.
* (bluefox) destroy widgets before views deletion

### 0.10.13 (2016-09-23)
* (bluefox) fixed errors for iPad 1
* (bluefox) start working on relative positions

### 0.10.12 (2016-09-16)
* (bluefox) group specific visibility of widgets and views

### 0.10.11 (2016-09-15)
* (bluefox) fix for iOS 10
* (bluefox) allow disabling of groups for performance

### 0.10.10 (2016-09-14)
* (bluefox) add text2speech widget
* (bluefox) try to fix problem with iOS 10

### 0.10.9 (2016-09-04)
* (bluefox) support of web-sockets force
* (bluefox) destroy unused views after 30 seconds
* (bluefox) do not show middle leading lines if top and bottom are shown
* (bluefox) let timestamp and last-change to show time as interval

### 0.10.7 (2016-07-09)
* (bluefox) add settings to reload vis
* (bluefox) add dark reload screen
* (bluefox) fix reload interval
* (bluefox) export/import
* (bluefox) add global script
* (bluefox) add 'not exist'/'not consist'/'exist' to signal and visibility
* (bluefox) fix OIDs in editor

### 0.10.5 (2016-06-15)
* (bluefox) fix select ID dialog
* (bluefox) add align help lines
* (bluefox) never store data in non-edit mode

### 0.10.4 (2016-06-14)
* (bluefox) fix drag and resize
* (Patrick) fix QuoJS
* (bluefox) support of milliseconds in formatDate
* (bluefox) support of getHistory
* (bluefox) support of show history instances
* (bluefox) grid
* (bluefox) add previews

### 0.10.3 (2016-05-30)
* (bluefox) update canJS
* (pmant) fixes bugs with dialogs on touchscreens
* (bluefox) speedUP show attributes at 300ms
* (bluefox) fix click on widget if signal is active

### 0.10.2 (2016-05-24)
* (bluefox) fix widgets with timestamps

### 0.10.1 (2016-05-23)
* (bluefox) change version

### 0.10.0 (2016-05-23)
* (bluefox) translates
* (bluefox) fix 'no widgets selected'
* (bluefox) change widget icons
* (bluefox) add signals
* (bluefox) add app.css for cordova
* (bluefox) change icons preview
* (bluefox) show properties of widget as icon
* (bluefox) fix error with external commands
* (bluefox) add types icon to preview
* (bluefox) support edit on iPad1
* (bluefox) change security settings

## License
To use this adapter in ioBroker you need to accept the source code license of the adapter. The source code of this adapter is available under the CC BY-NC license.

Additionally, you need a license to use the adapter. The following license editions are available on https://iobroker.net/www/pricing 
* **Community-License: Free for private use!**: Get a free license by registering an account on https://iobroker.net . The license if checked online against the ioBroker license server when the vis adapter is started, so an online connection at this timepoint is required!
* **Private use Offline-License**: For paying a small support fee you can get rid of the required online license check on adapter startup. **Only for Private use!**
* **Commercial License**: When using Vis in a commercial environment or selling Vis as part of ioBroker packages to your customers this license is for you. License check is also not requiring an online connection.

## License
 Copyright (c) 2013-2022 bluefox, https://github.com/GermanBluefox <dogafox@gmail.com>,
 
 Copyright (c) 2013-2014 hobbyquaker, https://github.com/hobbyquaker <hobbyquaker@gmail.com>,
 
 Creative Common Attribution-NonCommercial (CC BY-NC)

 http://creativecommons.org/licenses/by-nc/4.0/

![CC BY-NC License](https://github.com/GermanBluefox/DashUI/raw/master/images/cc-nc-by.png)

Short content:
Licensees may copy, distribute, display and perform the work and make derivative works based on it only if they give the author or licensor the credits in the manner specified by these.
Licensees may copy, distribute, display, and perform the work and make derivative works based on it only for noncommercial purposes.
(Free for non-commercial use).