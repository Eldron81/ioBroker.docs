---
BADGE-Number of Installations: http://iobroker.live/badges/javascript-stable.svg
BADGE-NPM version: http://img.shields.io/npm/v/iobroker.javascript.svg
BADGE-Downloads: https://img.shields.io/npm/dm/iobroker.javascript.svg
chapters: {"pages":{"de/adapterref/iobroker.javascript/README.md":{"title":{"de":"no title"},"content":"de/adapterref/iobroker.javascript/README.md"},"de/adapterref/iobroker.javascript/blockly.md":{"title":{"de":"Inhalt"},"content":"de/adapterref/iobroker.javascript/blockly.md"}}}
---
**Hinweis: Momentan beinhaltet die Englishe Version viel mehr Information und wir empfehlen die zu lesen**

Der javascript-Adapter dient dazu komfortabel Skripte zu erstellen, editieren und zu verwalten.

## Konfiguration
[Hier ist mehr darüber](https://github.com/ioBroker/ioBroker/wiki/ioBroker-Adapter-javascript#konfiguration)


![Einstellungsmenü Javascript Adapter](img/javascript_Einstellungen-Javascript.png)
   Die eigentliche Konfiguration besteht aus der Eingabe von zusätzlichen zu ladenden npm-Module (durch Komma getrennt), sowie der Geo-Koordinaten, die für diverse Berechnungen benutzt werden sollen. Um die Koordinaten zu erhalten, kann man z.B. _google maps_ ziemlich weit aufzoomen und an der gewünschten Stelle anklicken. Die Koordinaten werden dann angezeigt. Nach dem Speichern muss der Adapter noch über die rote Play-Taste aktiviert werden.

* * *

## Bedienung
Bei der Installation wird ein weiterer Reiter _Scripte_ in der _Admin_-Oberfläche gezeigt. Hier wird ein neuer Ordner angelegt, indem auf das (+) in der Symbolleiste (roter Kreis) geklickt wird.  Ein neues Skript wird über das "leere Blatt"-Icon links davon angelegt. Ein Fenster öffnet sich und fragt den Namen und Speicherort in der Ordnerstruktur ab. 
![Javascript Adapter](img/javascript_Javascript-Adapter.png)


### Ordner- und Dateiliste
Die Ordnerstruktur kann nach eigenem Wunsch angelegt werden. Der Speicherort hat keine Auswirkungen auf die Funktionalität des Skriptes. Neben der Baumstruktur gibt es eine Listenansicht. Ein Suchfeld erleichtert das Wiederfinden von Skripten. Damit ein Skript läuft, muss es links in der Ordnerstruktur durch klick auf den roten _Play_-Knopf aktiviert werden. Zum Stoppen auf den grünen _Pause_-Knopf drücken. Für jedes Skript wird ein neues Objekt angelegt. Es trägt den Skriptnamen mit dem Zusatz `_enabled` und liegt im Ordner `javascript.Instanz.ScriptEnabled`. Das Objekt zeigt mit (`true/false`) an, ob das Skript läuft. Der Zustand kann auch gesetzt werden, um das Skript ein-/auszuschalten. Skripte, die im Ordner _global_ gespeichert wurden, sind globale Skripte. Diese werden intern vor jedes andere Skript kopiert, also vorher abgearbeitet. Somit lassen sich globale Funktionen auf mehrere Skripte anwenden. Variablen in globalen Skripten können in anderen Skripten benutzt werden. Aber Achtung: Jedes Skript hat seinen eigenen Variablen-Raum. Man kann Variablen in globalen Skripten also nicht dazu benutzen um Werte zwischen Skripten auszutauschen. Dazu müssen zwingend Objekte (States) genutzt werden.  

### Editor
Nach dem Anlegen öffnet sich rechts der Editor für _Javascript_. Einige Beispielskripte finden sich [hier](http://www.iobroker.net/docu/?page_id=2786&lang=de).

#### Name
Hat man vorher einen Namen vergeben, wird dieser hier angezeigt und kann geändert werden.

#### Speicherort
In diesem Dropdown werden alle angelegten Ordner angezeigt. Zurzeit sind sie in der chronologischen Folge ihrer Erstellung sortiert.

#### Enginetyp
hier kann ausgewählt werden, ob mit der _javascript_ oder der _coffeescript_ engine gearbeitet werden soll.

#### Log
Rechts unten findet sich das Log-Fenster für die Ausgabe aller das markierte Skript betreffende Logs. Die Logs werden nach dem Abspeichern/Neustart des Skriptes angezeigt.

* * *

## Tipps
### Backup
Um Skripte im Zweifel wiederherstellen zu können, sei die Sicherung per _Copy & Paste_ empfohlen.

### Test-Instanz
Es hat sich bewährt, zum Testen von neuen Skripten, eine weitere Javascript-Instanz anzulegen und das Skript in dieser Instanz zu starten. 
Hinter dem Skriptnamen lässt sich per Dropdown die gewünschte Instanz einstellen. 
Sollte im Skript ein schwerwiegender Fehler sein, beendet sich nur diese zusätzliche Testinstanz, nicht die Produktivinstanz. 

![Instanz Javascript Adapter wählen](img/screen.jpg)

## Changelog
<!--
	### **WORK IN PROGRESS**
-->

### __WORK IN PROGRESS__
* (Apollon77) Prevented duplicate schedule triggering with inaccurate RTC clocks
* (Apollon77) Fixed sendToAsync and sendToHostAsync
* (Apollon77) Added rename/renameFile(Async) methods
* (Apollon77) Deprecated get/setBinaryState(Async) methods and log a message on usage. Use Files instead!
* (Apollon77) Deprecated usage of own states in javascript.X.scriptEnabled/Problem and log a message on usage. Use own states in 0_userdata.0 instead!

### 6.1.4 (2022-11-14)
* (bluefox) Corrected small error in rules
* (bluefox) Tried to fix debug mode

### 6.1.3 (2022-11-03)
* (Apollon77) Prevent adapter crash when script could not be compiled

### 6.1.2 (2022-11-03)
* (bluefox) Added ukrainian translation

### 6.1.0 (2022-11-03)
* (Apollon77) Add a configurable check for number of setStates per Minute to prevent scripts from taking down ioBroker. Default are 1000 setState per minute. Only stops if the number is reached 2 minutes in a row!
* (Apollon77) Add createAlias method to create aliases for states
* (Apollon77) Add setStateDelayed to selector
* (Apollon77) Add options to exec command
* (Apollon77) Fix issues with cancelling schedules when stopping scripts
* (bluefox) Corrected debug mode

### 6.0.3 (2022-09-14)
* (AlCalzone) Downgrade Typescript to prevent errors with global typescript scripts

## License
The MIT License (MIT)

Copyright (c) 2014-2023 bluefox <dogafox@gmail.com>,

Copyright (c) 2014      hobbyquaker