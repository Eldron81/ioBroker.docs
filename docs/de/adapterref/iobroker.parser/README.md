---
translatedFrom: en
translatedWarning: Wenn Sie dieses Dokument bearbeiten möchten, löschen Sie bitte das Feld "translationsFrom". Andernfalls wird dieses Dokument automatisch erneut übersetzt
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/de/adapterref/iobroker.parser/README.md
title: ioBroker-Parser-Adapter
hash: +XYwQT5Dyh/iO+GD5Kyw1Mv3LLzRKeJbJQ3SAlFwrrk=
---
![Logo](../../../en/adapterref/iobroker.parser/admin/parser.png)

![Anzahl der Installationen](http://iobroker.live/badges/parser-stable.svg)
![NPM-Version](http://img.shields.io/npm/v/iobroker.parser.svg)
![Downloads](https://img.shields.io/npm/dm/iobroker.parser.svg)

# IoBroker-Parser-Adapter
![Testen und freigeben](https://github.com/ioBroker/ioBroker.parser/workflows/Test%20and%20Release/badge.svg) [![Übersetzungsstatus](https://weblate.iobroker.net/widgets/adapters/-/parser/svg-badge.svg)](https://weblate.iobroker.net/engage/adapters/?utm_source=widget)

**Dieser Adapter verwendet Sentry-Bibliotheken, um Ausnahmen und Codefehler automatisch an die Entwickler zu melden.** Weitere Details und Informationen zum Deaktivieren der Fehlerberichterstattung finden Sie unter [Sentry-Plugin-Dokumentation](https://github.com/ioBroker/plugin-sentry#plugin-sentry)! Sentry-Berichte werden ab js-controller 3.0 verwendet.

Dieser Adapter analysiert per URL oder aus einer Datei empfangene Daten mithilfe regulärer Ausdrücke. Für jede Regel, die in den Einstellungen dieses Adapters konfiguriert wird, wird unter `parser.<instance number>` ein Status erstellt und mit den geparsten Informationen gefüllt und aktualisiert.

## Einstellungen
### 1. Standardabfrageintervall
Dieser Standardwert für das Abfrageintervall wird verwendet, wenn für einen Eintrag in der Konfigurationstabelle (Spalte: "Intervall") kein individueller Wert für das Abfrageintervall angegeben ist. Das Intervall wird in Millisekunden angegeben und definiert, wie oft der Link oder die Datei gelesen und die Zustände aktualisiert werden.

**Hinweis:** Verwenden Sie insbesondere für Website-URLs kein zu aggressives Abfrageintervall. Wenn Sie beispielsweise den Kurs Ihrer Aktien von einer bestimmten Website abrufen möchten, sollten Sie wahrscheinlich mit einem Intervall von nur 24 Stunden (= 86400000 ms) auskommen, wenn Sie kein Daytrader sind. Wenn Sie zu oft versuchen, Daten von bestimmten URLs abzurufen, kann die Website Sie sperren und Sie auf eine Server-Blacklist setzen. Verwenden Sie daher das Abfrageintervall mit Bedacht.

### 2. Timeout anfordern
Geben Sie an, wie lange der Adapter bei Websiteabfragen auf eine HTTP-Antwort wartet

### 3. Verzögerung zwischen Anfragen
Geben Sie an, wie lange der Adapter beim Ausführen von Remoteabfragen zwischen HTTP-Anforderungen wartet. Nützlich beim Abrufen von Daten von langsamen Hosts oder über langsame Verbindungen, um eine Überlastung beider zu vermeiden. Null (Standard) bedeutet keine Verzögerung.

Diese Verzögerung gilt pro Host. Wenn Remote-Abfragen so konfiguriert sind, dass sie von mehreren Remote-Hosts abrufen, wird jeder Host parallel abgefragt.

Die Verzögerung ist ein Mindestwert zwischen dem Initiieren jeder Anforderung. D.h. Wenn das Lesen einer Abfrage länger dauert als dieser Verzögerungsparameter, wird die nächste gestartet, sobald der Lesevorgang abgeschlossen ist.

### 4. Akzeptieren Sie ungültige Zertifikate
Geben Sie an, ob selbstsignierte/ungültige SSL/TLS-Zertifikate bei HTTPS-Anforderungen akzeptiert oder abgelehnt werden

### 5. Verwenden Sie einen unsicheren HTTP-Parser
Geben Sie an, dass ein unsicherer HTTP-Parser verwendet werden soll, der ungültige HTTP-Header akzeptiert. Dies kann die Interoperabilität mit nicht konformen HTTP-Implementierungen ermöglichen.
Die Verwendung des unsicheren Parsers sollte vermieden werden.

### 6. Tabelle
Klicken Sie auf die Schaltfläche „Plus“, um der Tabelle einen neuen Eintrag hinzuzufügen.

**Leistungshinweis:** Wenn Sie dieselbe URL oder denselben Dateinamen mehrmals in verschiedene Tabellenzeilen eingeben und die Werte der Spalte "Intervall" gleich sind, wird nur der Inhalt der URL oder des Dateinamens abgerufen ** einmal** und zwischengespeichert, um mehrere Tabellenzeilen zu verarbeiten, die mit URL/Dateiname und Intervall übereinstimmen. Auf diese Weise können Sie mehrere Regex (also mehrere Tabellenzeilen) auf eine einzelne URL oder einen Dateinamen anwenden, ohne die Daten mehrmals von der Quelle abrufen zu müssen.

**Tabellenfelder:**

- ***Name*** - Name des Staates, der unter `parser.<Instanznummer>` erstellt wird. Leerzeichen sind nicht erlaubt. Sie können Punkte "." als Trennzeichen zum Erstellen von Unterordnern. Beispiel: „Shares.Microsoft.Current“ ergibt „parser.<Instanznummer>.Shares.Micosoft.Current“.
- ***URL oder Dateiname*** - entweder eine URL einer Website oder der Pfad zu einer Datei, über die wir Informationen abrufen möchten. Beispiele `https://darksky.net/forecast/48.1371,11.5754/si24/de` (Wetterinfo München), oder `/opt/iobroker/test/testdata.txt` (Datei aus ioBroker).
- ***RegEx*** - Regulärer Ausdruck, wie man Daten aus einem Link extrahiert. Es gibt einen guten Dienst zum Testen von Regula-Ausdrücken: [regex101](https://regex101.com/). Z.B. *temp swip">(-?\d+)˚<* für die obige Zeile.
- ***Item*** (deutsch: "Num") - eine Regex kann mehrere Einträge finden (matchen). Mit dieser Option können Sie festlegen, welche Übereinstimmung ausgewählt werden soll. 0 = erste Übereinstimmung, 1 = zweite Übereinstimmung, 2 = dritte Übereinstimmung usw. Der Standardwert ist 0 (erste Übereinstimmung).
- ***Rolle*** - eine der Rollen:
    - custom - user definiert sich via *admin* die Rolle
    - Temperatur - Der Wert ist die Temperatur
    - value - der Wert ist eine Zahl (z. B. Dimmer)
    - Jalousien - der Wert ist eine Blindposition
    - switch - der Wert ist die Schalterposition (true/false)
    - Schaltfläche - Der Wert ist eine Schaltfläche
    - Indikator - boolescher Indikator
- ***Typ*** – der Variablentyp gemäß dem Pulldown-Menü.
- ***Einheit*** - Optional: Einheit des Werts, der dem Zustandseintrag hinzugefügt wird. Z.B. `°C`, `€`, `GB` usw.
- ***Alt*** - Wenn aktiviert, wird der Status *nicht* aktualisiert, wenn der Wert nicht gelesen oder im angegebenen Datum (URL oder Datei) gefunden werden kann, also behält er in diesem Fall den alten Wert.
- ***Subs*** - Optional: Ersatz-URL oder Dateiname. Dieser Ersatz-URL/Dateiname wird verwendet, wenn der URL/Dateiname der ersten Spalte nicht verfügbar ist.
- ***Factor/Offset*** (nur für "Typ"-Nummern) - ermöglicht die Änderung der abgerufenen Daten vor dem Setzen in den Zustand:
  - *berechneter Wert* = *extrahierter Wert* * Faktor + Offset , um sofort Wertänderungen vorzunehmen
- ***Intervall*** - Abfrageintervall in ms (Millisekunden). Wenn leer oder 0, wird das Standardabfrageintervall verwendet. Weitere Informationen finden Sie oben.

## Beispieleinstellungen
| Name | URL oder Dateiname | RegEx | Rolle | Geben Sie | ein Einheit | Intervall |
|-------------------|:-----------------------------------------------------|:----------------------|--------------|---------|------|----------|
| TemperaturMünchen | `https://darksky.net/forecast/48.1371,11.5754/si24/de` | `temp swip">(-?\d+)˚<` | Temperatur | Nummer | °C | 180000 |
| cloudRunning | `https://iobroker.net/` | `Privacy Notice` | Indikator | boolesch | | 60000 |
| CPUTemperatur | `/sys/devices/virtual/thermal/thermal_zone0/temp` | `(.*)` | Temperatur | Zahl | °C | 30000 |
| stockPreis.Visa | `https://www.finanzen.net/aktien/visa-aktie` | `\d{0,3},\d{2}(?=<span>EUR<\/span>)` | Wert | Nummer | € | 86400000 |
| stockPreis.Visa | `https://www.finanzen.net/aktien/visa-aktie` | `\d{0,3},\d{2}(?= <span>EUR&lt;\/span&gt;)` | Wert | Nummer | € | 86400000 |</span> |

*Hinweis:* Beim Anwenden von Regex auf die abgerufenen URL-/Dateidaten werden alle Zeilenumbrüche durch Leerzeichen ersetzt, um eine mehrzeilige Suche zu ermöglichen.

## Über reguläre Ausdrücke (RegExp)
Reguläre Ausdrücke sind ein leistungsstarkes Werkzeug zum Analysieren und Extrahieren bestimmter Daten aus Zeichenfolgen, und noch wichtiger: Sie ermöglichen das Extrahieren bestimmter Werte/Texte aus einer bestimmten Zeichenfolge (z. B. aus dem HTML einer Webseite oder Text aus einer Datei), indem Regeln angewendet werden .

Für boolesche Typen ist die Regex ziemlich einfach. Bei numerischen Typen sollten Sie die Zahl mit Klammern markieren - "()". Z.B. Um die Zahl aus *Die Temperatur beträgt 5°C* zu extrahieren, sollten Sie den Ausdruck " (\d+)" verwenden.

Weitere Informationen zu RegExp:

  * [MDN/Mozilla-Dokumentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)
  * [regex101: Online-Tool zum Erstellen und Testen regulärer Ausdrücke](https://regex101.com/)

### Beispiele
- *.at* stimmt mit jeder dreistelligen Zeichenfolge überein, die mit „at“ endet, einschließlich „Hut“, „Katze“ und „Fledermaus“.
- *[hc]at* passt auf "Hut" und "Katze".
- *[^b]at* stimmt mit allen Strings überein, die mit .at übereinstimmen, außer "bat".
- *[^hc]at* stimmt mit allen Zeichenfolgen überein, die mit .at übereinstimmen, außer "hat" und "cat".
- *^[hc]at* findet "Hut" und "Katze", aber nur am Anfang des Strings oder der Zeile.
- *[hc]at$* passt auf "Hut" und "Katze", aber nur am Ende des Strings oder der Zeile.
- *\[.\]* entspricht jedem einzelnen Zeichen, das von "[" und "]" umgeben ist, da die Klammern maskiert sind, zum Beispiel: "[a]" und "[b]".
- *s.\** stimmt mit s gefolgt von null oder mehr Zeichen überein, zum Beispiel: "s" und "saw" und "seed".
- *[hc]+at* stimmt mit "hat", "cat", "hhat", "chat", "hcat", "cchchat" usw. überein, aber nicht mit "at".
- *[hc]?at* findet "hat", "cat" und "at".
- *[hc]\*at* passt auf „hat“, „cat“, „hhat“, „chat“, „hcat“, „cchchat“, „at“ und so weiter.
- *Katze|Hund* passt auf "Katze" oder "Hund".
- *(\d+)* - erhält die Nummer aus der Zeichenfolge
- *jetzt (\w+)* später - holt das Wort zwischen "jetzt" und "später"

### Andere nützliche Ausdrücke
- (-?\d+) Zahl abrufen (sowohl negative als auch positive Zahlen)
- [+-]?([0-9]+.?[0-9]|.[0-9]+) erhält eine Zahl mit Dezimalstellen (und . als Dezimaltrennzeichen)
- [+-]?([0-9]+,?[0-9]|,[0-9]+) erhält eine Zahl mit Dezimalstellen (und , als Dezimaltrennzeichen)

## Qualitätscodes
Werte können Quality Codes haben:

- 0 - OK
- 0x82 - Die URL oder Datei kann nicht gelesen werden.
- 0x44 - Zahl oder Stringwert nicht im Text gefunden

## Die Unterstützung
1. Allgemein: [ioBroker-Forum](https://forum.iobroker.net/). Deutschsprachige Nutzer: siehe [ioBroker Forum Thread Parser-Adapter](https://forum.iobroker.net/topic/4494/adapter-parser-regex).
2. Bei Problemen lesen Sie bitte [ioBroker Parser Adapter: GitHub Issues](https://github.com/ioBroker/ioBroker.parser/issues).

<!--

### **IN ARBEIT** -->

## Changelog
### 1.3.1 (2022-11-09)
* (raintonr) added delay option for slow connections
* (bluefox) added compact mode

### 1.2.1 (2022-09-15)
* (Apollon77) Always use raw response and not try to parse it

### 1.2.0 (2022-09-12)
* (Apollon77) Allow to specify if self-signed/invalid SSL certificates are ignored or not (default is to ignore as till now)
* (Apollon77) Allow to specify if an "insecure HTTP parser" is used which also enables HTTP implementations that are not compliant to specifications
* (Apollon77) Allow to specify the HTTP request timeout

### 1.1.8 (2022-06-27)
* (Apollon77) Check that a link is configured

### 1.1.7 (2022-06-16)
* (Apollon77) Fix potential crash cases reported by Sentry

### 1.1.6 (2022-05-28)
* (Apollon77) Set method to "GET" when requesting URLs

### 1.1.5 (2022-04-19)
* (Apollon77) Ignore objects without configuration for parser and log it

### 1.1.4 (2022-03-21)
* (Apollon77) Fix crash case reported by Sentry

### 1.1.3 (2022-03-20)
* (Apollon77) if regex did not match set defined replacement value (or null)

### 1.1.2 (2022-03-09)
* (Apollon77) Fix initialization of new parser objects

### 1.1.1 (2022-03-07)
* IMPORTANT: js-controller 2.0 is required at least now!
* (Apollon77) ignore self signed ssl certificates
* (Apollon77) make sure object changes do not block further updates of values
* (Apollon77) Add Sentry to get crash reports

### 1.0.7 (2018-10-08)
* (bluefox) Comma will be replaced automatically by point for the offset and for the factor

### 1.0.6 (2018-09-22)
* (bluefox) fix parser

### 1.0.5 (2018-08-30)
* (bluefox) Multi-line search allowed

### 1.0.2 (2018-08-06)
* (bluefox) Iterations in regex were corrected

### 1.0.1 (2017-12-10)
* (bluefox) Added additional option: old value

### 1.0.0 (2017-05-19)
* (bluefox) Allow set the number of found item

### 0.2.2 (2017-04-03)
* (Apollon77) fix handling of multiple fields for one URL

### 0.2.1 (2017-02-24)
* (bluefox) fix error with timestamp

### 0.2.0 (2017-02-01)
* (bluefox) Add visual test

### 0.1.1 (2017-01-30)
* (bluefox) move to common group

### 0.0.1 (2017-01-16)
* (bluefox) initial commit

## License
The MIT License (MIT)

Copyright (c) 2017-2022 bluefox <dogafox@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.