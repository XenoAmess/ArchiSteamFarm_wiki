# Konfiguration

Diese Seite widmet sich der Konfiguration von ASF. Es dient als eine lückenlose Dokumentation des `config` Verzeichnisses, so dass du ASF auf deine Bedürfnisse abstimmen kannst.

- **[Einleitung](#einleitung)**
- **[Web-basierter ConfigGenerator](#web-basierter-configgenerator)**
- **[Manuelle Konfiguration](#manuelle-konfiguration)**
- **[Globale Konfiguration](#globale-konfiguration)**
- **[Bot Konfiguration](#bot-konfiguration)**
- **[Dateistruktur](#dateistruktur)**
- **[JSON-Mapping](#json-mapping)**
- **[Kompatibilitätsmapping](#kompatibilitätsmapping)**
- **[Konfigurations-Kompatibilität](#konfigurations-kompatibilität)**
- **[Automatisches Nachladen](#automatisches-nachladen)**

* * *

## Einleitung

Die ASF-Konfiguration gliedert sich in zwei Hauptteile - die globale (Prozess-)Konfiguration und die Konfiguration jedes einzelnen Bots. Jeder Bot hat seine eigene Bot-Konfigurationsdatei namens `BotName.json` (wobei `BotName` der Name des Bots ist), während die globale ASF (Prozess-)Konfiguration eine einzige Datei namens `ASF.json` ist.

Ein Bot ist ein einzelnes Steam-Konto welches am ASF-Prozess teilnimmt. Um ordnungsgemäß zu funktionieren benötigt ASF mindestens **eine** definierte Bot-Instanz. Es gibt keine prozessbedingte Begrenzung der Bot-Instanzen, so dass du so viele Bots (Steam-Konten) verwenden kannst wie du willst.

ASF verwendet das **[JSON](https://en.wikipedia.org/wiki/JSON)** Format zum Speichern seiner Konfigurationsdateien. Es ist ein benutzerfreundliches, lesbares und sehr universelles Format in dem du das Programm konfigurieren kannst. Keine Sorge, du musst kein JSON können um ASF zu konfigurieren. Ich habe es nur erwähnt, falls du bereits daran denkst ASF-Konfigurationen mit einer Art Bash-Skript massenhaft zu erstellen.

Die Konfiguration kann entweder manuell, durch Erstellung korrekter JSON-Konfigurationen, erfolgen oder durch die Verwendung unseres **[Web-basierten ConfigGenerators](https://justarchinet.github.io/ASF-WebConfigGenerator)**, was viel einfacher und komfortabler sein sollte. Wenn du kein fortgeschrittener Benutzer bist, schlage ich vor, den Konfigurationsgenerator zu verwenden, der im Folgenden beschrieben wird.

* * *

## Web-basierter ConfigGenerator

Der Zweck des **[web-basierten ConfigGenerators](https://justarchinet.github.io/ASF-WebConfigGenerator)** ist es, dir ein benutzerfreundliches Frontend zur Verfügung zu stellen, das zum Erzeugen von ASF-Konfigurationsdateien verwendet wird. Der web-basierte ConfigGenerator ist zu 100% Client-basiert, was bedeutet, dass die von dir eingegebenen Daten nirgendwo hin gesendet, sondern nur lokal verarbeitet werden. Dies garantiert Sicherheit und Zuverlässigkeit, da es sogar **[offline](https://github.com/JustArchiNET/ASF-WebConfigGenerator/tree/master/docs)** funktioniert, wenn du alle Dateien herunterlädst und `index.html` in deinem Lieblingsbrowser öffnest.

Der web-basierte ConfigGenerator wurde unter Chrome und Firefox getestet, sollte aber in allen gängigen JavaScript-fähigen Browsern ordnungsgemäß funktionieren.

Die Verwendung ist denkbar einfach - man wählt über die entsprechenden Registerkarten, ob man eine Konfiguration für `ASF` oder `Bot` erstellen möchte, überprüft ob die gewählte Version der Konfigurationsdatei zur verwendeten ASF-Version passt, füllt dann die entsprechenden Felder aus und klickt abschließend auf die Schaltfläche "Herunterladen". Verschiebe diese Datei in das ASF `config` Verzeichnis und überschreibe bei Bedarf vorhandene Dateien. Wiederhole diese Schritte bei allen zukünftigen Anpassungen. Der Rest dieses Abschnitts erklärt alle zur Verfügung stehenden Konfigurationsmöglichkeiten.

* * *

## Manuelle Konfiguration

Ich empfehle dir unbedingt den web-basierten ConfigGenerator zu verwenden. Solltest du diesen aus irgendeinem Grund nicht verwenden wollen kannst du natürlich auch selbst gültige Konfigurationen erstellen. Sieh dir die folgenden JSON-Beispiele an, um einen guten Start in die richtige Struktur zu erhalten. Du kannst den Inhalt in eine Datei kopieren und als Basis für deine Konfiguration verwenden. Da du nicht unser Frontend verwendest, stelle sicher, dass deine Konfiguration **[gültig](https://jsonlint.com)** ist, da ASF sich weigert sie zu laden wenn sie nicht verarbeitet werden kann. Für eine korrekte JSON-Struktur aller verfügbaren Felder sieh dir den Abschnitt **[JSON-Mapping](#json-mapping)** und die Dokumentation unten an.

* * *

## Globale Konfiguration

Die globale Konfiguration befindet sich in der Datei `ASF.json` und hat folgende Struktur:

```json
{
    "AutoRestart": true,
    "Blacklist": [],
    "CommandPrefix": "!",
    "ConfirmationsLimiterDelay": 10,
    "ConnectionTimeout": 90,
    "CurrentCulture": null,
    "Debug": false,
    "FarmingDelay": 15,
    "GiftsLimiterDelay": 1,
    "Headless": false,
    "IdleFarmingPeriod": 8,
    "InventoryLimiterDelay": 3,
    "IPC": false,
    "IPCPassword": null,
    "LoginLimiterDelay": 10,
    "MaxFarmingTime": 10,
    "MaxTradeHoldDuration": 15,
    "OptimizationMode": 0,
    "Statistics": true,
    "SteamMessagePrefix": "/me ",
    "SteamOwnerID": 0,
    "SteamProtocols": 7,
    "UpdateChannel": 1,
    "UpdatePeriod": 24,
    "WebLimiterDelay": 300,
    "WebProxy": null,
    "WebProxyPassword": null,
    "WebProxyUsername": null
}
```

* * *

Alle Optionen werden nachfolgend erklärt:

### `AutoRestart`

`bool` Typ mit Standardwert von `true`. Diese Eigenschaft legt fest, ob ASF bei Bedarf einen Selbst-Neustart durchführen darf. Es gibt ein paar Fälle, die von ASF einen Neustart erfordern, wie z.B. die ASF-Aktualisierung (durchgeführt mit `UpdatePeriod` oder dem Befehl `update`), sowie das Verändern der `ASF.json` Konfiguration, dem `restart` Befehl und ähnlich. Normalerweise beinhaltet der Neustart zwei Teile - das Erstellen eines neuen Prozesses und das Beenden des aktuellen Prozesses. Die meisten Benutzer sollten damit einverstanden sein und diese Eigenschaft auf dem Standardwert `true` behalten - wenn du ASF durch dein eigenes Skript und/oder mit `dotnet` ausführst, solltest du vielleicht die volle Kontrolle über den Start des Prozesses haben und eine Situation vermeiden, in der ein neuer (neu gestarteter) ASF-Prozess irgendwo im Hintergrund und nicht im Vordergrund des Skripts läuft, der zusammen mit dem alten ASF-Prozess beendet wurde. Dies ist besonders wichtig, wenn man bedenkt, dass der neue Prozess nicht mehr dein direktes "Kind" ist, was dich z.B. nicht in die Lage versetzen würde, die Standardkonsolen-Eingabe dafür zu verwenden.

Wenn das der Fall ist, ist diese Eigenschaft speziell für dich und du kannst sie auf `false` setzen. Bedenke jedoch, dass **du** in diesem Fall für den Neustart des Prozesses verantwortlich bist. Dies ist einigermaßen wichtig, da ASF sich nur beendet, anstatt einen neuen Prozess zu starten (z.B. nach der Aktualisierung), so dass, wenn du keine Logik hinzugefügt hast, ASF einfach aufhört zu laufen, bis du es wieder startest. ASF beendet sich immer mit einem korrekten Fehlercode, der Erfolg (Null) oder Misserfolg (ungleich Null) anzeigt. Auf diese Weise kannst du in deinem Skript eine entsprechende Logik hinzufügen, die einen automatischen Neustart von ASF im Fehlerfall vermeiden sollte oder zumindest zur weiteren Analyse eine lokale Kopie von `log.txt` erstellt. Bedenke auch, dass der Befehl `restart` ASF immer neu startet, unabhängig davon, wie diese Eigenschaft eingestellt ist, da diese Eigenschaft das Standardverhalten definiert, während der Befehl `restart` den Prozess immer neu startet. Wenn du keinen Grund hast, diese Funktion zu deaktivieren, solltest du sie aktiviert lassen.

* * *

### `Blacklist`

`ImmutableHashSet<uint>` Typ mit einem leeren Standardwert. Wie der Name schon verrät, definiert diese globale Konfigurationseigenschaft AppIDs (Spiele), die vom automatischen ASF-Sammel-Prozess vollständig ignoriert werden. Leider liebt Steam es Sommer/Winter-Sale-Abzeichen als "verfügbar für Kartenabgabe" zu kennzeichnen, was den ASF-Prozess verwirrt, da es den Eindruck erweckt, dass es sich um ein gültiges Spiel handelt das gesammelt werden sollte. Wenn es keine schwarze Liste gäbe, würde ASF irgendwann beim Sammeln eines Spieles (das eigentlich kein Spiel ist) "hängen" bleiben und unendlich warten bis die Karten gesammelt wurden, was aber nie passieren wird. Die schwarze Liste von ASF dient dazu, diese Abzeichen als nicht für das Sammeln verfügbar zu kennzeichnen, so dass wir sie bei der Entscheidung, was zu Sammeln ist, stillschweigend ignorieren und nicht in die Falle tappen können.

ASF enthält standardmäßig zwei schwarze Listen - `GlobalBlacklist`, die fest im ASF-Quellcode definiert und nicht editierbar ist, und die normale `Blacklist` die hier definiert ist. Die `GlobalBlacklist` wird zusammen mit der ASF-Version aktualisiert und enthält typischerweise alle "schlechten" AppIDs zum Zeitpunkt der Veröffentlichung, so dass du, wenn die aktuelle Version von ASF verwendet wird, nicht deine eigene `Blacklist` pflegen musst (die hier definiert ist). Der Hauptzweck dieser Eigenschaft ist es, dir die Möglichkeit zu geben, neue, zum Zeitpunkt der ASF-Veröffentlichung unbekannte AppIDs (die nicht gesammelt werden sollten) auf die schwarze Liste zu setzen. Die fest definierte `GlobalBlacklist` wird so schnell wie möglich aktualisiert, daher ist es nicht erforderlich, dass du deine eigene `Blacklist` aktualisierst, wenn du die neueste ASF-Version verwendest, aber ohne `Blacklist` wärst du gezwungen, ASF zu aktualisieren, um "weiterzumachen", wenn Valve ein neues Sale-Abzeichen veröffentlicht - ich möchte dich nicht zwingen, den neuesten ASF-Code zu verwenden, daher ist diese Eigenschaft hier, um es dir zu ermöglichen, ASF selbst zu "reparieren", wenn du aus irgendeinem Grund nicht auf neue fest programmierte `GlobalBlacklist` in der neuen ASF-Version aktualisieren willst oder kannst, aber du willst dein altes ASF am Laufen halten. Wenn du keinen **triftigen** Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

Wenn du stattdessen nach einer Bot-basierten schwarzen Liste suchst, schaue dir die **[Befehle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** `ib`, `ibadd` und `ibrm` an.

* * *

### `CommandPrefix`

`string` Typ mit einem Standardwert von `!`. Diese Eigenschaft spezifiziert das **größensensitive** Präfix, das für **[Befehle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** in ASF verwendet wird. Mit anderen Worten: Du solltest es jedem ASF-Befehl voranstellen, damit ASF dir zuhört. Es ist möglich, diesen Wert auf `null` oder leer zu setzen, damit ASF kein Befehlspräfix verwendet, in diesem Fall gibst du Befehle mit ihren einfachen Bezeichnern ein. Dies kann jedoch die Leistung von ASF beeinträchtigen, da ASF optimiert ist, um Nachrichten nicht weiter zu analysieren, wenn sie nicht mit `CommandPrefix` beginnen - wenn du dich absichtlich dazu entscheidest, sie nicht zu verwenden, wird ASF gezwungen sein, alle Nachrichten zu lesen und darauf zu reagieren, selbst wenn es sich nicht um ASF-Befehle handelt. Daher wird empfohlen, weiterhin irgendein `CommandPrefix` zu verwenden, wie z.B. `/`, wenn du den Standardwert von `!` nicht magst. Aus Konsistenzgründen betrifft `CommandPrefix` den gesamten ASF-Prozess. Wenn du keinen Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `ConfirmationsLimiterDelay`

`byte` Typ mit einem Standardwert von `10`. ASF wird sicherstellen, dass mindestens `ConfirmationsLimiterDelay` Sekunden zwischen zwei aufeinanderfolgenden 2FA-Bestätigungen liegen, die Anfragen abrufen, um eine Auslösung des Ratenlimits zu vermeiden - diese werden von **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** während z.B. des Befehls `2faok` sowie bei verschiedenen handelsbezogenen Operationen nach Bedarf verwendet. Der Standardwert wurde auf Basis unserer Tests festgelegt und sollte nicht verringert werden, wenn du auf keine Probleme stoßen möchtest. Wenn du keinen **triftigen** Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `ConnectionTimeout`

`byte` Typ mit einem Standardwert von `90`. Diese Eigenschaft definiert die Auszeiten für verschiedene Netzwerkaktionen, die von ASF ausgeführt werden, in Sekunden. Im Einzelnen definiert `ConnectionTimeout` die Auszeit in Sekunden für HTTP- und IPC-Anfragen, `ConnectionTimeout / 10` definiert die maximale Anzahl der fehlgeschlagenen Heartbeats, während `ConnectionTimeout / 30` die Anzahl der Minuten definiert, die wir für die initiale Steam-Netzwerkverbindungsanforderung berücksichtigen. Der Standardwert von `90` sollte für die Mehrheit der Benutzer in Ordnung sein, aber wenn du eine eher langsame Netzwerkverbindung oder einen PC hast, solltest du diese Zahl vielleicht um etwas Höheres erhöhen (wie `120`). Bedenke, dass höhere Werte langsame oder sogar unzugängliche Steam-Server nicht auf magische Weise reparieren, also sollten wir nicht unendlich auf etwas warten, das nicht passieren wird, und es einfach später noch einmal versuchen. Eine zu hohe Einstellung dieses Wertes führt zu einer übermäßigen Verzögerung bei der Erfassung von Netzwerkproblemen und zu einer Verringerung der Gesamtleistung. Wenn du diesen Wert zu niedrig einstellst, verringert sich auch die Gesamtstabilität und -leistung, da ASF eine gültige Anfrage abbricht, die noch verarbeitet wird. Daher hat das Setzen dieses Wertes unter dem Standardwert im Allgemeinen keinen Vorteil, da Steam-Server von Zeit zu Zeit sehr langsam sind und mehr Zeit für das Verarbeiten von ASF-Anfragen benötigen könnten. Der Standardwert ist eine ausgewogene Balance zwischen dem Vertrauen, dass unsere Netzwerkverbindung stabil ist, und dem Misstrauen des Steam-Netzwerks, unsere Anfrage innerhalb einer bestimmten Auszeit zu bearbeiten. Wenn du Probleme früher erkennen und die ASF-Wiederverbindung bzw. -Antwort beschleunigen möchtest, sollte der Standardwert dies tun (oder ganz knapp darunter, wie z.B. `60`, wodurch ASF weniger geduldig wird). Wenn du stattdessen bemerkst, dass ASF auf Netzwerkprobleme stößt, wie z.B. fehlgeschlagene Anfragen, Heartbeats, die verloren gehen oder die Verbindung zu Steam unterbrochen wird, könnte es sinnvoll sein, diesen Wert zu erhöhen, wenn du sicher bist, dass es sich um **nicht** um Fehler handelt, die durch dein Netzwerk, sondern durch Steam selbst verursacht werden, da zunehmende Auszeiten ASF mehr "geduldig" machen und sich nicht entscheiden, die Verbindung sofort wieder herzustellen.

Eine Beispielsituation, die eine Erhöhung dieser Eigenschaft erfordern könnte, ist die Möglichkeit, ASF mit einem sehr großen Handelsangebot zu beauftragen, das gut 2+ Minuten dauern kann, um von Steam vollständig akzeptiert und bearbeitet zu werden. Durch die Erhöhung des Standard-Timeout wird ASF geduldiger und wartet länger, bevor es entscheidet, dass der Handel nicht durchläuft und die ursprüngliche Anfrage aufgibt.

Eine weitere Situation könnte durch eine sehr langsame Maschine oder Internetverbindung verursacht werden, die mehr Zeit benötigt, um die zu übertragenden Daten zu verarbeiten. Dies ist eine ziemlich seltene Bedingung, da die CPU/Netzwerkbandbreite fast nie ein Engpass ist, aber dennoch eine erwähnenswerte Möglichkeit.

Kurz gesagt, der Standardwert sollte in den meisten Fällen angemessen sein, aber du kannst ihn bei Bedarf erhöhen. Trotzdem macht es keinen großen Sinn, weit über den Standardwert hinauszugehen, da größere Auszeiten unzugängliche Steam-Server nicht magisch beheben. Wenn du keinen Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `CurrentCulture`

`string` Typ mit einem Standardwert von `null`. Standardmäßig versucht ASF, die Sprache deines Betriebssystems zu verwenden, und wird es vorziehen, übersetzte Zeichenketten in dieser Sprache zu verwenden, falls verfügbar. Dies ist möglich dank unserer Community, die versucht, ASF in allen gängigen Sprachen zu **[übersetzen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization)**. Wenn du aus irgendeinem Grund deine Betriebssystem-Muttersprache nicht verwenden möchtest, kannst du diese Konfigurationseigenschaft verwenden, um eine gültige Sprache auszuwählen, die du stattdessen verwenden möchtest. Für eine Liste aller verfügbaren Sprachen besuche bitte **[MSDN](https://msdn.microsoft.com/en-us/library/cc233982.aspx)** und suche nach `Language tag`. Es ist gut zu erwähnen, dass ASF sowohl spezifische Sprachen wie `en-GB` als auch neutrale Sprachen wie `en` akzeptiert. Die Angabe der aktuellen Sprache kann auch andere sprachspezifische Aspekte beeinflussen, wie z.B. das Währungs-/Datumsformat und ähnliches. Bitte bedenke, dass du möglicherweise zusätzliche Schrift-/Sprachpakete benötigst, um sprachspezifische Zeichen richtig darzustellen, wenn du eine nicht-native Sprache gewählt hast. Normalerweise willst du diese Konfigurationseigenschaft nutzen, wenn du ASF auf Englisch anstelle deiner Muttersprache bevorzugst.

* * *

### `Debug`

`bool` Typ mit einem Standardwert von `false`. Diese Eigenschaft legt fest, ob der Prozess im Debug-Modus laufen soll. Im Debug-Modus erstellt ASF ein spezielles Verzeichnis `debug` neben dem `config` Verzeichnis, das die gesamte Kommunikation zwischen ASF und den Steam-Servern verfolgt. Debug-Informationen können helfen, lästige Probleme im Zusammenhang mit der Vernetzung und dem allgemeinen ASF-Arbeitsablauf zu erkennen. Darüber hinaus werden einige Programmroutinen viel ausführlicher sein, wie z.B. `WebBrowser`, die den genauen Grund für das Scheitern einiger Anfragen angeben - diese Einträge werden in das normale ASF-Protokoll geschrieben. **Du solltest ASF nicht im Debug-Modus ausführen, es sei denn, du wirst von einem Entwickler dazu aufgefordert**. Das Ausführen von ASF im Debug-Modus **verringert die Leistung**, **beeinträchtigt die Stabilität negativ** und ist **weitaus ausführlicher an verschiedenen Stellen**, daher sollte es **nur** gewollt und kurzfristig, für das Debuggen bestimmter Probleme, die Reproduktion des Problems oder das Erhalten von mehr Informationen über eine fehlgeschlagene Anfrage und ähnliches verwendet werden, aber **nicht** für die normale Programmausführung. Du wirst **viele** neue Fehler, Probleme und Ausnahmen sehen - stelle sicher, dass du ein gutes Wissen über ASF, Steam und seine Besonderheiten hast, wenn du dich entscheidest, das alles selbst zu analysieren, da nicht alles relevant ist.

**WARNUNG:** Das Aktivieren dieses Moduses beinhaltet das Protokollieren von **potenziell sensiblen** Informationen wie Anmeldeinformationen und Passwörter, die du für die Anmeldung bei Steam verwendest (aufgrund von Netzwerkprotokollierung). Diese Daten werden sowohl in das Verzeichnis `debug` als auch in die normale `log.txt` Datei geschrieben (diese ist nun absichtlich viel umfangreicher, um diese Information zu protokollieren). Du solltest keine Debug-Inhalte, die von ASF generiert wurden, an einem öffentlichen Ort posten, der ASF-Entwickler sollte dich immer daran erinnern, sie an seine E-Mail oder einen anderen sicheren Ort zu senden. Wir speichern diese sensiblen Details nicht und nutzen sie auch nicht, sie werden als Teil von Debug-Routinen geschrieben, da ihre Anwesenheit für das Problem, das dich betrifft, relevant sein könnte. Wir würden es vorziehen, wenn du das ASF-Logging in keiner Weise ändern würdest, aber wenn du besorgt bist, kannst du diese sensiblen Details entsprechend bearbeiten.

> Beim Editieren werden sensible Details, z.B. durch Sterne, ersetzt. Du solltest darauf verzichten, sensible Zeilen ganz zu entfernen, da ihre reine Existenz relevant sein könnte und erhalten werden sollte.

* * *

### `FarmingDelay`

`byte` Typ mit einem Standardwert von `15`. Damit ASF funktioniert, wird es alle `FarmingDelay` Minuten das aktuell gesammelte Spiel überprüfen, ob es vielleicht schon alle Karten erhalten hat. Wenn du diese Eigenschaft zu niedrig einstellst, kann dies dazu führen, dass eine übermäßige Anzahl von Steam-Anfragen gesendet wird, während eine zu hohe Einstellung dazu führen kann, dass ASF immer noch den vorgegebenen Titel für bis zu `FarmingDelay` Minuten, nachdem es vollständig gesammelt wurde, "sammelt". Der Standardwert sollte für die meisten Benutzer hervorragend sein, aber wenn du viele Bots im Einsatz hast, kannst du ihn auf etwa `30` Minuten erhöhen, um das Senden von Steam-Anfragen zu begrenzen. Es ist gut zu wissen, dass ASF einen ereignisbasierten Mechanismus verwendet und die Spiel-Abzeichen-Seite für jeden gesammelten Steam-Gegenstand überprüft, so dass wir im Allgemeinen **nicht einmal in festen Zeitabständen überprüfen müssen**, aber da wir dem Steam-Netzwerk nicht voll vertrauen - überprüfen wir die Spiel-Abzeichen-Seite trotzdem, wenn wir es nicht überprüft haben, indem die Karte im letzten `FarmingDelay` Minuten gesammelt wurde (falls das Steam-Netzwerk uns nicht über den gesammelten Gegenstand oder dergleichen informiert hat). Angenommen, das Steam-Netzwerk funktioniert ordnungsgemäß, wird die Herabsetzung dieses Wertes **in keiner Weise die Effizienz des Sammelns verbessern**, während **die Erhöhung des Netzwerkaufwandes dies signifikant tut** - es wird empfohlen, es nur (falls erforderlich) vom Standardwert aus auf `15` Minuten zu erhöhen. Wenn du keinen **triftigen** Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `GiftsLimiterDelay`

`byte` Typ mit einem Standardwert von `1`. ASF wird sicherstellen, dass mindestens `GiftsLimiterDelay` Sekunden zwischen zwei aufeinanderfolgenden Anfragen zur Handhabung (Einlösen) von Geschenken/Produktschlüsseln/Lizenzen liegen, um zu vermeiden, dass ein Anfragen-Limit ausgelöst wird. Darüber hinaus wird es auch als globaler Begrenzer für Spiele-Listen-Anfragen verwendet, wie z.B. die vom `owns` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)**. Wenn du keinen **triftigen** Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `Headless`

`bool` Typ mit einem Standardwert von `false`. Diese Eigenschaft legt fest, ob der Prozess im Headless-Modus laufen soll. Im Headless-Modus geht ASF davon aus, dass es auf einem Server oder in einer anderen nicht interaktiven Umgebung läuft, daher wird es nicht versuchen Informationen über die Konsolen-Eingabe zu lesen. Dazu gehören On-Demand-Details (Konto-Anmeldeinformationen wie 2FA-Code, SteamGuard-Code, Passwort oder jede andere Variable, die für den Betrieb von ASF erforderlich ist) sowie alle anderen Konsolen-Eingaben (z.B. interaktive Befehlskonsole). Mit anderen Worten, der `Headless` Modus ist gleichbedeutend mit der schreibgeschützten ASF-Konsole. Diese Einstellung ist vor allem für Benutzer nützlich die ASF auf ihren Servern ausführen, da ASF, anstatt z.B. nach 2FA-Code zu fragen, den Vorgang stillschweigend abbricht, indem es ein Konto stoppt. Wenn du ASF nicht auf einem Server ausführst und vorher bestätigt hast, dass ASF im Nicht-Headless-Modus funktionieren kann, solltest du diese Eigenschaft deaktiviert lassen. Jegliche Benutzerinteraktion wird im Headless-Modus verweigert, und deine Konten werden nicht ausgeführt, wenn sie beim Start **irgendwelche** Konsolen-Eingaben erfordern. Dies ist für Server nützlich, da ASF den Versuch, sich am Konto anzumelden, wenn nach Anmeldeinformationen gefragt wird, abbrechen kann, anstatt (unendlich) darauf zu warten, dass der Benutzer diese bereitstellt. Wenn du diesen Modus aktivierst, kannst du auch den `input` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** verwenden, der als Ersatz für die standardmäßige Konsolen-Eingabe dient. Wenn du dir nicht sicher bist, wie du diese Eigenschaft einstellen sollst, belasse sie bei dem Standardwert `false`.

Wenn du ASF auf dem Server verwendest, solltest du diese Option zusammen mit dem `--process-required` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)** verwenden.

* * *

### `IdleFarmingPeriod`

`byte` Typ mit einem Standardwert von `8`. Wenn ASF nichts zu sammeln hat, wird es regelmäßig alle `IdleFarmingPeriod` Stunden überprüfen, ob das Konto vielleicht einige neue Spiele zum Sammeln hat. Dieses Feature ist nicht erforderlich, wenn es sich um neue Spiele handelt, die wir erhalten, da ASF intelligent genug ist, um in diesem Fall automatisch die Abzeichen-Seiten zu überprüfen. `IdleFarmingPeriod` ist hauptsächlich für Situationen wie alte Spiele gedacht, bei denen wir bereits Karten hinzugefügt haben. In diesem Fall gibt es kein Ereignis, weshalb ASF regelmäßig die Abzeichen-Seiten überprüfen muss, wenn wir dies berücksichtigen wollen. Der Wert von `0` deaktiviert dieses Feature. Siehe auch: `ShutdownOnFarmingFinished`.

* * *

### `InventoryLimiterDelay`

`byte` Typ mit einem Standardwert von `3`. ASF wird sicherstellen, dass mindestens `InventoryLimiterDelay` Sekunden zwischen zwei aufeinanderfolgenden Inventar-Anfragen liegen, um ein Auslösen des Anfrage-Limits zu vermeiden - diese werden zum Abrufen von Steam-Inventaren verwendet, insbesondere während der von dir selbst gewählten Befehle wie `transfer`, sowie in Funktionen wie `MatchActively`. Der Standardwert von `3` wurde basierend auf dem Abrufen von Inventaren von über 100 aufeinanderfolgenden Bot-Instanzen festgelegt und sollte die meisten (wenn nicht alle) Benutzer zufrieden stellen. Du kannst es aber auch verringern oder sogar zu `0` wechseln, wenn du eine sehr geringe Anzahl von Bots hast, so dass ASF die Verzögerung ignoriert und die Steam-Inventare viel schneller plündert. Sei jedoch gewarnt, dass das Setzen eines zu niedrigen Wertes **dazu führt**, dass Steam deine IP vorübergehend blockiert, und das wird dich daran hindern, dein Inventar überhaupt abzurufen. Du musst diesen Wert möglicherweise auch erhöhen, wenn du viele Bots mit vielen Inventar-Anfragen ausführst, obwohl du in diesem Fall wahrscheinlich versuchen solltest, die Anzahl dieser Anfragen zu begrenzen. Wenn du keinen **triftigen** Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `IPC`

`bool` Typ mit einem Standardwert von `false`. Diese Eigenschaft definiert, ob der ASF-**[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)**-Server zusammen mit dem Prozess gestartet werden soll. IPC ermöglicht die prozessübergreifende Kommunikation durch Starten eines lokalen HTTP-Servers. Wenn du den IPC-Server von ASF nicht nutzen willst, dann gibt es keinen Grund für dich, diese Option zu aktivieren.

* * *

### `IPCPassword`

`string` Typ mit einem Standardwert von `null`. Diese Eigenschaft definiert das obligatorische Passwort für jeden API Aufruf über die IPC-Schnittstelle und dient als zusätzliche Sicherheitsmaßnahme. Wenn auf einen nicht-leeren Wert gesetzt, benötigen alle IPC-Anfragen eine zusätzliche `password`-Eigenschaft, die auf das hier angegebene Passwort eingestellt ist. Der Standardwert von `null` überspringt die Notwendigkeit des Passwortes, so dass ASF alle Anfragen akzeptiert. Darüber hinaus ermöglicht die Aktivierung dieser Option auch den eingebauten IPC-Anti-Bruteforce-Mechanismus, der die gegebene `IPAdress` vorübergehend blockiert, nachdem zu viele nicht autorisierte Anfragen in sehr kurzer Zeit gesendet wurden. Wenn du keinen Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `LoginLimiterDelay`

`byte` Typ mit einem Standardwert von `10`. ASF stellt sicher, dass zwischen zwei aufeinanderfolgenden Verbindungsversuchen mindestens `LoginLimiterDelay` Sekunden liegen, um eine Auslösung des Anfrage-Limits zu vermeiden. Der Standardwert von `10` wurde basierend auf der Verbindung von über 100 Bot-Instanzen festgelegt und sollte die meisten (wenn nicht alle) Benutzer zufrieden stellen. Du kannst es jedoch erhöhen/verringern, oder sogar zu `0` wechseln, wenn du eine sehr geringe Anzahl von Bots hast, so dass ASF die Verzögerung ignoriert und sich viel schneller mit Steam verbindet. Aber sei gewarnt, da das Setzen einer zu niedrigen Einstellung, während zu viele Bots gleichzeitig benutzt werden, **garantiert** dazu führt, dass Steam deine IP vorübergehend sperrt. Das wird dich **komplett** daran hindern, dich anzumelden, mit `InvalidPassword/RateLimitExceeded` Fehler - und das betrifft auch deinen normalen Steam-Client, nicht nur ASF. Gleichermaßen, wenn du eine übermäßige Anzahl von Bots verwendest, insbesondere zusammen mit anderen Steam-Clients/Programmen, die die gleiche IP-Adresse verwenden, musst du diesen Wert höchstwahrscheinlich erhöhen, um die Anmeldungen über einen längeren Zeitraum verteilen zu können.

Nebenbei bemerkt, wird dieser Wert auch als Load-Balancing-Puffer in allen ASF-geplanten Aktionen verwendet, wie z.B. Handelsangebote in `SendTradePeriod`. Wenn du keinen **triftigen** Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `MaxFarmingTime`

`byte` Typ mit einem Standardwert von `10`. Wie du wissen solltest, funktioniert Steam nicht immer richtig, manchmal können seltsame Situationen auftreten, wie z.B. dass Steam unsere Spielzeit nicht aufzeichnet, obwohl tatsächlich ein Spiel gespielt wird. ASF erlaubt es, ein einzelnes Spiel im Einzelmodus für maximal `MaxFarmingTime` Stunden zu betreiben und betrachtet es nach diesem Zeitraum als vollständig gesammelt. Dies ist erforderlich, um den Sammel-Prozess nicht einzufrieren, wenn es zu seltsamen Situationen kommt, aber auch, wenn Steam aus irgendeinem Grund ein neues Abzeichen veröffentlicht hat, das ASF daran hindern würde, weiter voranzukommen (siehe: `Blacklist`). Der Standardwert von `10` Stunden sollte ausreichen, um alle Steam-Karten aus einem Spiel zu sammeln. Wenn du diese Eigenschaft zu niedrig einstellst, kann dies dazu führen, dass gültige Spiele übersprungen werden (und ja, es gibt gültige Spiele, die sogar bis zu 9 Stunden zum Sammeln benötigen), während eine zu hohe Einstellung dazu führen kann, dass der Sammel-Prozess eingefroren wird. Bitte beachte, dass diese Eigenschaft nur ein einzelnes Spiel in einer einzigen Sammel-Sitzung betrifft (so dass ASF nach dem Durchlaufen der gesamten Warteschlange zu diesem Titel zurückkehrt), auch basiert sie nicht auf der Gesamtspielzeit, sondern auf der internen ASF-Sammel-Zeit, so dass ASF auch nach einem Neustart zu diesem Titel zurückkehrt. Wenn du keinen **triftigen** Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `MaxTradeHoldDuration`

`byte` Typ mit einem Standardwert von `15`. Diese Eigenschaft definiert die maximale Dauer der Handelssperre in Tagen, die wir bereit sind zu akzeptieren - ASF lehnt Handelsangebote ab, die länger als `MaxTradeHoldDuration` Tage gehalten werden, wie in **[Handeln](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-de-DE)** Abschnitt definiert. Diese Option ist nur für Bots mit `TradingPreferences` von `SteamTradeMatcher` sinnvoll, da sie `Master` / `SteamOwnerID` Handelsangebote und keine Spenden betrifft. Handelssperren sind für alle ärgerlich, und niemand will sich wirklich um sie kümmern. ASF soll nach liberalen Regeln arbeiten und jedem helfen, egal ob er sich in einer Handelssperre befindet oder nicht - deshalb ist diese Option standardmäßig auf `15` gesetzt. Wenn du stattdessen lieber alle von Handelssperren betroffene Handelsangebote ablehnen möchtest, kannst du hier `0` angeben. Bitte bedenke, dass Karten mit kurzer Lebensdauer von dieser Option nicht betroffen sind und für Personen mit Handelssperren automatisch abgelehnt werden, wie im Abschnitt **[Handel](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-de-DE)** beschrieben, so dass es nicht notwendig ist, alle nur deshalb global abzulehnen. Wenn du keinen Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `OptimizationMode`

`byte` Typ mit einem Standardwert von `0`. Diese Eigenschaft definiert den Optimierungsmodus, den ASF während der Laufzeit bevorzugt. Derzeit unterstützt ASF zwei Modi - `0`, der so genannte `MaxPerformance`, und `1`, der so genannte `MinMemoryUsage`. Standardmäßig bevorzugt ASF es, so viele Dinge wie möglich parallel (gleichzeitig) auszuführen, was die Leistung durch Load-Balancing-Arbeiten über alle CPU-Kerne, mehrere CPU-Threads, mehrere Sockel und mehrere Thread-Pool-Aufgaben hinweg erhöht. Zum Beispiel fragt ASF nach deiner ersten Abzeichen-Seite, wenn du nach Spielen zum Sammeln suchst, und sobald die Anfrage eingetroffen ist, liest ASF daraus, wie viele Abzeichen-Seiten du tatsächlich hast, und fordert dann alle gleichzeitig an. Das ist es, was du dir eigentlich **fast immer** wünschst, da der Aufwand in den meisten Fällen minimal ist und die Vorteile des asynchronen ASF-Codes auch auf der ältesten Hardware mit einem einzigen CPU-Kern und stark eingeschränkter Leistung sichtbar sind. Da jedoch viele Aufgaben parallel verarbeitet werden, ist die ASF-Laufzeit für ihre Wartung verantwortlich, z.B. Sockets offen zu halten, Threads am Leben zu erhalten und Aufgaben zu bearbeiten, was von Zeit zu Zeit zu einer erhöhten Speicherauslastung führen kann, und wenn du durch den verfügbaren Speicher extrem eingeschränkt bist, solltest du diese Eigenschaft auf `1` (`MinMemoryUsage`) umschalten, um ASF zu zwingen, so wenig Aufgaben wie möglich zu verwenden und typischerweise possible-to-parallel asynchronen Code synchron zu verwenden. Du solltest erwägen, diese Eigenschaft nur zu ändern, wenn du vorher **[Speichereffizientes Setup](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-de-DE)** gelesen hast und du absichtlich gigantische Leistungssteigerung opfern willst, für eine sehr kleine Verringerung des Speicheraufwands. Normalerweise ist diese Option **viel schlechter** als das, was du mit anderen möglichen Methoden erreichen kannst, z.B. indem du deine ASF-Nutzung einschränkst oder den Garbage Collector der Laufzeit abstimmst, wie in **[Speichereffizientes Setup](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-de-DE)** erklärt. Daher solltest du `MinMemoryUsage` als **letzten Schritt** verwenden, kurz vor der Neukompilierung der Laufzeit, wenn du mit anderen (viel besseren) Optionen keine zufriedenstellende Ergebnisse erzielen konntest. Wenn du keinen **triftigen** Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `Statistics`

`bool` Typ mit Standardwert von `true`. Diese Eigenschaft legt fest, ob ASF die Statistik aktiviert haben soll. Eine detaillierte Erklärung, was genau diese Option bewirkt, findest du im Abschnitt **[Statistiken](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics-de-DE)**. Wenn du keinen Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `SteamMessagePrefix`

`string` Typ mit einem Standardwert von `"/me "`. Diese Eigenschaft definiert ein Präfix, das allen Steam-Nachrichten, die von ASF gesendet werden, vorangestellt wird. Standardmäßig verwendet ASF das Präfix `"/me "`, um Bot-Nachrichten leichter zu unterscheiden, da sie im Steam-Chat in verschiedenen Farben angezeigt werden. Eine weitere erwähnenswerte Eigenschaft ist das Präfix `"/pre "`, das ein ähnliches Ergebnis erzielt, aber eine andere Formatierung verwendet. Du kannst diese Eigenschaft auch auf eine leere Zeichenkette oder `null` setzen, um die Verwendung des Präfixes vollständig zu deaktivieren und alle ASF-Nachrichten auf traditionelle Weise auszugeben. Es ist gut zu wissen, dass diese Eigenschaft nur Steam-Nachrichten betrifft - Antworten, die über andere Kanäle (z.B. IPC) zurückgegeben werden, sind nicht betroffen. Wenn du das standardmäßige ASF-Verhalten nicht anpassen möchtest, ist es eine gute Idee, es bei den Standardeinstellungen zu belassen.

* * *

### `SteamOwnerID`

`ulong` Typ mit einem Standardwert von `0`. Diese Eigenschaft definiert die Steam-ID in 64-Bit-Form des ASF-Prozessinhabers und ist sehr ähnlich der `Master` Berechtigung der gegebenen Bot-Instanz (aber stattdessen global). Du möchtest diese Eigenschaft fast immer auf die ID deines eigenen Steam-Haupt-Kontos setzen. `Master` Berechtigung beinhaltet die volle Kontrolle über seine Bot-Instanz, aber globale Befehle wie `exit`, `restart` oder `update` sind nur für `SteamOwnerID` verfügbar. Dies ist praktisch, da du vielleicht Bots für deine Freunde ausführen möchtest, ohne ihnen zu erlauben, den ASF-Prozess zu steuern, wie z.B. das Beenden über den Befehl `exit`. Der Standardwert von `0` gibt an, dass es keinen Besitzer des ASF-Prozesses gibt, was bedeutet, dass niemand in der Lage sein wird, globale ASF-Befehle auszuführen. Bedenke, dass **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)** Befehle in Verbindung mit der `SteamOwnerID` ausgeführt werden, also musst du hier einen gültigen Wert angeben, wenn du diese verwenden möchtest.

* * *

### `SteamProtocols`

`byte flags` Typ mit einem Standardwert von `7`. Diese Eigenschaft definiert Steam-Protokolle welche ASF beim Verbinden mit Steam-Servern verwendet. Sie sind wie folgt definiert:

| Wert | Name      | Beschreibung                                                                                     |
| ---- | --------- | ------------------------------------------------------------------------------------------------ |
| 0    | None      | Kein Protokoll                                                                                   |
| 1    | TCP       | **[Transmission Control Protocol](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)** |
| 2    | UDP       | **[User Datagram Protocol](https://en.wikipedia.org/wiki/User_Datagram_Protocol)**               |
| 4    | WebSocket | **[WebSocket](https://en.wikipedia.org/wiki/WebSocket)**                                         |

Bitte bedenke, dass diese Eigenschaft das Feld `flags` ist, daher ist es möglich, eine beliebige Kombination von verfügbaren Werten auszuwählen. Schau dir **[JSON-Mapping](#json-mapping)** an, wenn du mehr darüber erfahren möchtest. Das Nicht-Aktivieren eines der Flags führt zur Option `None`, und diese Option ist an sich schon ungültig.

By default ASF will use all available Steam protocols as a measure for fighting with downtimes and other similar Steam issues. Normalerweise möchtest du diese Eigenschaft ändern, wenn du ASF darauf beschränken möchtest, nur ein oder zwei bestimmte Protokolle anstelle aller verfügbaren zu verwenden. Eine solche Maßnahme könnte erforderlich sein, wenn du z.B. nur TCP-Datenverkehr auf deiner Firewall aktivierst und du nicht willst, dass ASF versucht, eine Verbindung über UDP herzustellen. Wenn du jedoch kein bestimmtes Problem debuggst, willst du fast immer sicherstellen, dass ASF jedes Protokoll verwenden kann, das derzeit unterstützt wird, und nicht nur ein oder zwei. Wenn du keinen **triftigen** Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `UpdateChannel`

`byte` Typ mit einem Standardwert von `1`. Diese Eigenschaft definiert den Aktualisierungskanal, der entweder für automatische Aktualisierungen verwendet wird (wenn `UpdatePeriod` größer als `0` ist) oder für Aktualisierungsbenachrichtigungen (anderweitig). Derzeit unterstützt ASF drei Aktualisierungskanäle - `0`, welcher `None` genannt wird, `1`, welcher `Stable` genannt wird, und `2`, welcher `Experimental` genannt wird. Der Kanal `Stable` ist der standardmäßige Veröffentlichungskanal, der von der Mehrheit der Benutzer verwendet werden sollte. `Experimentell` Kanal zusätzlich zu `Stable` Veröffentlichungen, beinhaltet auch **Vorveröffentlichungen** für fortgeschrittene Benutzer und andere Entwickler, um neue Funktionen zu testen, fehlerbehebungen zu bestätigen oder Rückmeldungen über geplante Verbesserungen abzugeben. **Experimentelle Versionen enthalten oft unbehobene Programmfehler, Work-in-Progress-Funktionen oder neu geschriebene Implementierungen**. Wenn du dich nicht als fortgeschrittener Benutzer betrachtest, bleibst du bitte beim Standard-Aktualisierungskanal `1` (Stable). Der Kanal `Experimental` ist für Benutzer gedacht, die wissen, wie man Fehler meldet, Probleme löst und Rückmeldung gibt - es wird keine technische Unterstützung geboten. Sieh dir den **[Veröffentlichungszyklus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-de-DE)** von ASF an, wenn du mehr darüber erfahren möchtest. Du kannst auch `UpdateChannel` auf `0` (`None`) setzen, wenn du alle Versionsüberprüfungen vollständig deaktivieren willst. Wenn du `UpdateChannel` auf `0` stellst, wird die gesamte Funktionalität im Zusammenhang mit Aktualisierungen vollständig deaktiviert, einschließlich des Befehls `update`. Es wird **ausdrücklich** von der Verwendung des `None` Kanals **abgeraten**, weil du dich dadurch allen möglichen Problemen aussetzt (erwähnt in `UpdatePeriod` Beschreibung unten).

**Wenn du nicht weißt was du tust**, empfehlen wir **ausdrücklich** es bei den Standardeinstellungen zu belassen.

* * *

### `UpdatePeriod`

`byte` Typ mit einem Standardwert von `24`. Diese Eigenschaft legt fest, wie oft ASF nach automatischen Aktualisierungen suchen soll. Aktualisierungen sind nicht nur für den Erhalt neuer Funktionen und kritischer Sicherheits-Patches entscheidend, sondern auch für den Erhalt von Fehlerbehebungen, Leistungssteigerungen, Stabilitätsverbesserungen und mehr. Wenn ein Wert größer als `0` eingestellt ist, lädt ASF sich automatisch herunter, ersetzt und startet sich neu (wenn `AutoRestart` es erlaubt), wenn eine neue Aktualisierung verfügbar ist. Um dies zu erreichen, wird ASF alle `UpdatePeriod` Stunden überprüfen, ob eine neue Aktualisierung in unserem GitHub Repository verfügbar ist. Ein Wert von `0` deaktiviert automatische Aktualisierungen, ermöglicht es dir aber trotzdem, den Befehl `update` manuell auszuführen. Du könntest auch daran interessiert sein, den entsprechenden `UpdateChannel` einzustellen, dem `UpdatePeriod` folgen sollte.

Der Aktualisierungsprozess von ASF beinhaltet die Aktualisierung der gesamten Ordnerstruktur, die ASF verwendet, aber ohne deine eigenen Konfigurationen oder Datenbanken im Verzeichnis `config` zu berühren - das bedeutet, dass alle zusätzlichen Dateien, die nicht mit ASF in seinem Verzeichnis zusammenhängen, während des Aktualisierungsvorgangs verloren gehen können. Der Standardwert von `24` ist ein gutes Verhältnis zwischen unnötigen Prüfungen und ASF, das neu genug ist.

Wenn du keinen **triftigen** Grund hast, diese Funktion zu deaktivieren, solltest du die automatischen Aktualisierungen innerhalb einer angemessenen Zeitspanne von `UpdatePeriod` **für dein eigenes Wohl aktivieren**. Dies liegt nicht nur daran, dass wir nichts anderes als die neueste stabile ASF-Version unterstützen, sondern auch daran, dass **wir unsere Sicherheitsgarantie nur für die neueste Version** geben. Wenn du eine veraltete ASF-Version verwendest, dann setzt du dich absichtlich allen möglichen Problemen aus, von kleinen Fehlern über defekte Funktionen bis hin zu **[permanenten Steam-Konto Sperren](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-de-DE#wurde-jemand-daf%C3%BCr-gesperrt)**, also empfehlen wir **dringend**, um zu deinem eigenen Nutzen immer sicherzustellen, dass deine ASF-Version auf dem neuesten Stand ist. Automatische Aktualisierungen ermöglichen es uns, schnell auf alle Arten von Problemen zu reagieren, indem wir problematischen Quelltext deaktivieren oder beheben, bevor er eskalieren kann - wenn du dich dagegen entscheidest, verlierst du alle unsere Sicherheitsgarantien und Risikofolgen durch das Ausführen von Code, der möglicherweise schädlich sein könnte, nicht nur für das Steam-Netzwerk, sondern auch (per Definition) für dein eigenes Steam-Konto.

* * *

### `WebLimiterDelay`

`ushort` Typ mit einem Standardwert von `300`. Diese Eigenschaft definiert in Millisekunden die minimale Verzögerung zwischen dem Senden von zwei aufeinanderfolgenden Anfragen an denselben Steam-Webservice. Eine solche Verzögerung ist erforderlich, da der Dienst **[AkamaiGhost](https://www.akamai.com)**, den Steam intern nutzt, eine Geschwindigkeitsbegrenzung basierend auf der globalen Anzahl der über einen bestimmten Zeitraum gesendeten Anfragen beinhaltet. Unter normalen Umständen ist akamai Block ziemlich schwer zu erreichen, aber unter sehr hohen Arbeitslasten mit einer riesigen laufenden Warteschlange von Anfragen ist es möglich, ihn auszulösen, wenn wir immer wieder zu viele Anfragen über einen zu kurzen Zeitraum senden.

Der Standardwert wurde unter der Annahme festgelegt, dass ASF das einzige Programm ist, das auf die Steam-Webdienste zugreift, insbesondere `steamcommunity.com`, `api.steampowered.com` und `store.steampowered.com`. Wenn du andere Programme verwendest, die Anfragen an dieselben Webservices senden, dann solltest du sicherstellen, dass dein Programm ähnliche Funktionen wie `WebLimiterDelay` enthält und beide auf das Doppelte des Standardwerts setzen, was `600` wäre. Dies garantiert, dass du unter keinen Umständen mehr als 1 Anfrage pro `300` ms senden wirst.

Im Allgemeinen wird das Herabsetzen von `WebLimiterDelay` unter den Standardwert **stark abgeraten**, da es zu verschiedenen IP-bezogenen Sperren führen kann, von denen einige dauerhaft sein können. Der Standardwert ist gut genug, um eine einzelne ASF-Instanz auf dem Server auszuführen und ASF im Normalfall zusammen mit dem ursprünglichen Steam-Client zu verwenden. Es sollte für die meisten Anwendungen zutreffend sein, und du solltest es nur erhöhen (nie senken), wenn du - abgesehen von der Verwendung von ASF - auch ein anderes Programm verwendest, das eine übermäßige Anzahl von Anfragen an dieselben Webdienste senden könnte, die ASF nutzt. Kurz gesagt, die globale Anzahl aller Anfragen, die von einer einzelnen IP an eine einzelne Steam-Domäne gesendet werden, sollte nie 1 Anfrage pro `300` ms überschreiten.

Wenn du keinen Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `WebProxy`

`string` Typ mit einem Standardwert von `null`. Diese Eigenschaft definiert eine Web-Proxy-Adresse, die für alle internen http- und https-Anfragen verwendet wird, die von ASFs `HttpClient` gesendet werden, insbesondere für Dienste wie `github.com`, `steamcommunity.com` und `store.steampowered.com`. Das Proxying von ASF-Anfragen im Allgemeinen hat keine Vorteile, aber es ist äußerst nützlich, um verschiedene Arten von Firewalls zu umgehen, insbesondere die große Firewall von China.

Diese Eigenschaft ist als uri Zeichenfolge definiert:

> Eine URI-Zeichenkette besteht aus einem Schema (http oder https), einem Host und einem optionalen Port. Ein Beispiel für eine komplette uri-Zeichenkette wäre `"http://contoso.com:8080"`.

Wenn dein Proxy eine Benutzer-Authentifizierung erfordert, musst du auch `WebProxyUsername` und/oder `WebProxyPassword` einrichten. Wenn es keinen solchen Bedarf gibt, genügt die Errichtung dieser Eigenschaft allein.

Im Moment verwendet ASF den Web-Proxy nur für `http` und `https` Anfragen, was **nicht** die interne Steam-Netzwerk-Kommunikation innerhalb des internen Steam-Clients von ASF beinhaltet. Es gibt derzeit keine Pläne dies zu unterstützen, hauptsächlich wegen der fehlenden **[SK2](https://github.com/SteamRE/SteamKit/issues/587#issuecomment-413271550)** Funktionalität. Wenn du es brauchst/willst, würde ich vorschlagen, da anzufangen.

Wenn du keinen Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `WebProxyPassword`

`string` Typ mit einem Standardwert von `null`. Diese Eigenschaft definiert das Feld für das Passwort, das bei der Basis-, Digest-, NTLM- und Kerberos-Authentifizierung verwendet wird und von einem Ziel `WebProxy`-Rechner mit Proxy-Funktionalität unterstützt wird. Wenn dein Proxy keine Benutzer-Anmeldeinformationen benötigt, ist es nicht notwendig, dass du hier etwas einträgst. Die Verwendung dieser Option ist nur sinnvoll, wenn auch `WebProxy` verwendet wird, da sie sonst keine Wirkung hat.

Wenn du keinen Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `WebProxyUsername`

`string` Typ mit einem Standardwert von `null`. Diese Eigenschaft definiert das Feld für den Benutzernamen, das bei der Basis-, Digest-, NTLM- und Kerberos-Authentifizierung verwendet wird und von einem Ziel `WebProxy`-Rechner mit Proxy-Funktionalität unterstützt wird. Wenn dein Proxy keine Benutzer-Anmeldeinformationen benötigt, ist es nicht notwendig, dass du hier etwas einträgst. Die Verwendung dieser Option ist nur sinnvoll, wenn auch `WebProxy` verwendet wird, da sie sonst keine Wirkung hat.

Wenn du keinen Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

## Bot Konfiguration

Wie du bereits weißt, sollte jeder Bot eine eigene Konfiguration haben, die auf der folgenden beispielhaften JSON-Struktur basiert. Beginne damit, zu entscheiden, wie du deinen Bot benennen möchtest (z.B. `1.json`, `main.json`, `haupt.json` oder `IrgendwasAnderes.json`) und gehe zur Konfiguration über.

**Hinweis:** Der Bot kann den Namen `ASF` nicht haben (da dieses Schlüsselwort für die globale Konfiguration reserviert ist), ASF ignoriert auch alle Konfigurationsdateien, die mit einem Punkt beginnen.

Die Bot-Konfiguration hat folgende Struktur:

```json
{
    "AcceptGifts": false,
    "AutoSteamSaleEvent": false,
    "BotBehaviour": 0,
    "CustomGamePlayedWhileFarming": null,
    "CustomGamePlayedWhileIdle": null,
    "Enabled": false,
    "FarmingOrders": [],
    "GamesPlayedWhileIdle": [],
    "HoursUntilCardDrops": 3,
    "IdlePriorityQueueOnly": false,
    "IdleRefundableGames": true,
    "LootableTypes": [1, 3, 5],
    "MatchableTypes": [5],
    "OnlineStatus": 1,
    "PasswordFormat": 0,
    "Paused": false,
    "RedeemingPreferences": 0,
    "SendOnFarmingFinished": false,
    "SendTradePeriod": 0,
    "ShutdownOnFarmingFinished": false,
    "SteamLogin": null,
    "SteamMasterClanID": 0,
    "SteamParentalCode": null,
    "SteamPassword": null,
    "SteamTradeToken": null,
    "SteamUserPermissions": {},
    "TradingPreferences": 0,
    "TransferableTypes": [1, 3, 5],
    "UseLoginKeys": true
}
```

* * *

Alle Optionen werden nachfolgend erklärt:

### `AcceptGifts`

`bool` Typ mit einem Standardwert von `false`. Wenn aktiviert, akzeptiert und löst ASF automatisch alle Steam-Geschenke (einschließlich Guthaben-Geschenkgutscheine), die an den Bot geschickt werden. Dazu gehören auch Geschenke, die von anderen Benutzern als denjenigen gesendet werden, die in `SteamUserPermissions` definiert sind. Bedenke, dass Geschenke, die an die E-Mail-Adresse geschickt werden, nicht direkt an den Client weitergeleitet werden, so dass ASF diese ohne deine Hilfe nicht annehmen wird.

Diese Option wird nur für Alternativkonten empfohlen, da es sehr wahrscheinlich ist, dass du nicht automatisch alle Geschenke einlösen möchtest, die an dein Hauptkonto gesendet werden. Wenn du dir nicht sicher bist ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

* * *

### `AutoSteamSaleEvent`

`bool` Typ mit einem Standardwert von `false`. Während der Steam Sommer/Winter-Verkaufsveranstaltungen ist Steam dafür bekannt, dass es dir zusätzliche Karten zur Verfügung stellt, um jeden Tag die Entdeckungsliste zu durchsuchen, sowie durch andere ereignisspezifische Aktivitäten. Wenn diese Option aktiviert ist, überprüft ASF automatisch die Steam Entdeckungsliste alle `8` Stunden (beginnend bei einer Stunde seit Programmstart) und durchläuft sie bei Bedarf. Diese Option wird nicht empfohlen, wenn du diese Aktion selbst durchführen möchtest, und normalerweise sollte sie nur bei Bot-Konten sinnvoll sein. Außerdem musst du sicherstellen, dass dein Konto mindestens das Level `8` hat, wenn du erwartest, diese Karten überhaupt zu erhalten, was direkt mit einer Steam-Anforderung zusammenhängt. Wenn du dir nicht sicher bist ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

Bitte bedenke, dass wir aufgrund von ständigen Steam-Problemen, Änderungen und Problemen **keine Garantie geben, ob diese Funktion ordnungsgemäß funktioniert**, daher ist es durchaus möglich, dass diese Option **überhaupt nicht funktioniert**. Wir akzeptieren **keine** Fehlermeldungen, auch keine Unterstützungsanfragen für diese Option. Es wird ohne jegliche Garantie angeboten, die Nutzung erfolgt auf eigene Gefahr.

* * *

### `BotBehaviour`

`byte flags` Typ mit Standardwert von `0`. Diese Eigenschaft definiert das ASF-Bot-ähnliche Verhalten bei verschiedenen Ereignissen und ist wie folgt definiert:

| Wert | Name                          | Beschreibung                                                                                             |
| ---- | ----------------------------- | -------------------------------------------------------------------------------------------------------- |
| 0    | None                          | Kein spezielles Bot-Verhalten, der am wenigsten invasive Modus, Standard                                 |
| 1    | RejectInvalidFriendInvites    | Führt dazu, dass ASF ungültige Freundschaftseinladungen ablehnt (anstatt sie zu ignorieren)              |
| 2    | RejectInvalidTrades           | Wird ASF dazu veranlassen, ungültige Handelsangebote abzulehnen (anstatt sie zu ignorieren)              |
| 4    | RejectInvalidGroupInvites     | Führt dazu, dass ASF ungültige Gruppeneinladungen ablehnt (anstatt sie zu ignorieren)                    |
| 8    | DismissInventoryNotifications | Veranlasst ASF dazu, alle Inventar-Benachrichtigungen automatisch zu entfernen                           |
| 16   | MarkReceivedMessagesAsRead    | Führt dazu, dass ASF automatisch alle empfangenen Nachrichten als gelesen markiert                       |
| 32   | MarkBotMessagesAsRead         | Will cause ASF to automatically mark messages from other ASF bots (running in the same instance) as read |

Bitte bedenke, dass diese Eigenschaft das Feld `flags` ist, daher ist es möglich, eine beliebige Kombination von verfügbaren Werten auszuwählen. Schau dir **[JSON-Mapping](#json-mapping)** an, wenn du mehr darüber erfahren möchtest. Wenn keines der Flags aktiviert wird, wird die Option `None` verwendet.

Im Allgemeinen möchtest du diese Eigenschaft ändern, wenn du von ASF erwartest, dass er einen bestimmten Grad an Automatisierung im Zusammenhang mit seiner Aktivität durchführt, wie es von einem Bot-Konto erwartet wird, aber nicht von einem primären Konto, das in ASF verwendet wird. Daher ist die Änderung dieser Eigenschaft vor allem für Alternativ-Konten sinnvoll, obwohl du ausgewählte Optionen auch für Hauptkonten verwenden kannst.

Normales (`None`) ASF-Verhalten ist es, nur Dinge zu automatisieren, die der Benutzer wünscht (z.B. Karten sammeln oder `SteamTradeMatcher` Angebote, wenn in `TradingPreferences` eingestellt). Dies ist der am wenigsten eingreifende Modus, und es ist für die Mehrheit der Benutzer von Vorteil, da du die volle Kontrolle über dein Konto hast und du selbst entscheiden kannst, ob du bestimmte Interaktionen außerhalb des Anwendungsbereichs zulässt oder nicht.

Eine ungültige Freundschaftseinladung ist eine Einladung, die nicht vom Benutzer mit `FamilySharing` Berechtigung (definiert in `SteamUserPermissions`) oder höher kommt. ASF ignoriert diese Einladungen im normalen Modus, wie du es erwarten würdest, und gibt dir die freie Wahl, ob du sie annehmen möchtest oder nicht. `RejectInvalidFriendInvites` führt dazu, dass diese Einladungen automatisch abgelehnt werden, was die Option für andere Personen, dich zu ihrer Freundesliste hinzuzufügen, praktisch deaktiviert (da ASF alle diese Anfragen ablehnt, mit Ausnahme der in `SteamUserPermissions` definierten Personen). Wenn du nicht alle Freundschaftseinladungen direkt ablehnen willst, solltest du diese Option nicht aktivieren.

Ein ungültiges Handelsangebot ist ein Angebot, das nicht durch das eingebaute ASF-Modul angenommen wird. Mehr zu diesem Thema findest du im Abschnitt **[Handel](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-de-DE)**, der explizit definiert, welche Arten von Handelsangeboten ASF bereit ist, automatisch zu akzeptieren. Gültige Handelsangebote werden auch durch andere Einstellungen definiert, insbesondere `TradingPreferences`. `RejectInvalidTrades` bewirkt, dass alle ungültigen Handelsangebote abgelehnt, anstatt ignoriert zu werden. Wenn du nicht alle Handelsangebote, die nicht automatisch von ASF angenommen werden, endgültig ablehnen willst, solltest du diese Option nicht aktivieren.

Eine ungültige Gruppeneinladung ist eine Einladung, die nicht aus der Gruppe `SteamMasterClanID` stammt. ASF ignoriert im normalen Modus diese Gruppeneinladungen, wie du es erwarten würdest, und erlaubt es dir, selbst zu entscheiden, ob du einer bestimmten Steam-Gruppe beitreten möchtest oder nicht. `RejectInvalidGroupInvites` führt dazu, dass alle diese Gruppeneinladungen automatisch abgelehnt werden, was es praktisch unmöglich macht, dich in irgendeine andere Gruppe als `SteamMasterClanID` einzuladen. Wenn du nicht alle Gruppeneinladungen direkt ablehnen möchtest, solltest du diese Option nicht aktivieren.

`DismissInventoryNotifications` is extremely useful when you start getting annoyed by constant Steam notification about receiving new items. ASF kann die Benachrichtigung selbst nicht loswerden, da sie in deinem Steam-Client integriert ist, aber es ist in der Lage, die Benachrichtigung nach Erhalt automatisch zu löschen, was dazu führt, dass keine Benachrichtigung über "neue Gegenstände im Inventar" mehr auftritt. Wenn du erwartest, alle erhaltenen Gegenstände selbst zu überprüfen (insbesondere Karten, die mit ASF gesammelt wurden), dann solltest du diese Option natürlich nicht aktivieren. Wenn du anfängst verrückt zu werden, denk daran, dass dies als Option angeboten wird.

`MarkReceivedMessagesAsRead` will automatically mark **all** messages being received by the account on which ASF is running, both private and group, as read. Dies sollte typischerweise nur von Alt-Konten verwendet werden, um Benachrichtigungen über "neue Nachrichten" zu löschen, die z.B. von dir während der Ausführung von ASF-Befehlen kommen. Wir empfehlen diese Option nicht für Hauptkonten, es sei denn, du möchtest dich von jeder Art von Benachrichtigungen über neue Nachrichten befreien, **einschließlich** derjenigen, die während deines Offline-Betriebs passiert sind, vorausgesetzt, dass ASF immer noch offen gelassen wurde, als es sie ablehnte.

`MarkBotMessagesAsRead` works in a similar manner by marking only bot messages as read. However, keep in mind that when using that option on group chats with your bots and other people, Steam implementation of acknowledging chat message **also** acknowledges all messages that happened **before** the one you decided to mark, so if by any chance you don't want to miss unrelated message that happened in-between, you typically want to avoid using this feature. Natürlich ist es ebenfalls riskant, wenn mehrere Primärkonten (z.B. von unterschiedlichen Nutzern) in der Selben ASF-Instanz laufen, besonders weil Sie die out-of-ASF Nachrichten verpassen könnten.

Wenn du dir nicht sicher bist, wie du diese Option konfigurieren sollst, ist es am besten, sie auf dem Standardwert zu belassen.

* * *

### `CustomGamePlayedWhileFarming`

`string` Typ mit einem Standardwert von `null`. Wenn ASF sammelt, kann es sich als "Spielt ein Steam fremdes Spiel: `CustomGamePlayedWhileFarming`" anzeigen, anstatt des aktuellen Spiels was gesammelt wird. Dies kann nützlich sein, wenn du deine Freunde darüber informieren möchtest, dass du am Sammeln bist, aber den `OnlineStatus` nicht auf `Offline` ändern möchtest. Bitte bedenke, dass ASF die tatsächliche Anzeige-Reihenfolge des Steam-Netzwerks nicht garantieren kann, daher ist dies nur ein Vorschlag, der richtig angezeigt werden kann oder auch nicht. Der Standardwert von `null` deaktiviert dieses Feature.

ASF provides a few special variables that you can optionally use in your text. `{0}` will be replaced by ASF with `AppID` of currently farmed game(s), while `{1}` will be replaced by ASF with `GameName` of currently farmed game(s).

* * *

### `CustomGamePlayedWhileIdle`

`string` Typ mit einem Standardwert von `null`. Ähnlich wie `CustomGamePlayedWhileFarming`, aber für die Situation, in der ASF nichts zu tun hat (da das Konto vollständig gesammelt wurde). Der Standardwert von `null` deaktiviert dieses Feature.

* * *

### `Enabled`

`bool` Typ mit einem Standardwert von `false`. Diese Eigenschaft definiert, ob der Bot aktiviert ist. Eine aktivierte Bot-Instanz (`true`) wird automatisch beim Starten von ASF gestartet, während eine deaktivierte Bot-Instanz (`false`) manuell gestartet werden muss. Standardmäßig ist jeder Bot deaktiviert, so dass du diese Eigenschaft wahrscheinlich auf `true` für alle deine Bots umschalten möchtest, die automatisch gestartet werden sollen.

* * *

### `FarmingOrders`

`ImmutableHashSet<byte>` Typ mit einem leeren Standardwert. Diese Eigenschaft definiert die von ASF verwendete **bevorzugte** Sammel-Reihenfolge für ein gegebenes Bot-Konto. Derzeit sind folgende Sammel-Reihenfolgen verfügbar:

| Wert | Name                      | Beschreibung                                                                                                       |
| ---- | ------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| 0    | Unordered                 | Keine Sortierung, leichte Verbesserung der CPU-Leistung                                                            |
| 1    | AppIDsAscending           | Versuche Spiele mit den niedrigsten `appIDs` zuerst zu sammeln                                                     |
| 2    | AppIDsDescending          | Versuche Spiele mit den höchsten `appIDs` zuerst zu sammeln                                                        |
| 3    | CardDropsAscending        | Versuche Spiele mit der niedrigsten Anzahl an verbleibenden Karten zuerst zu sammeln                               |
| 4    | CardDropsDescending       | Versuche Spiele mit der höchsten Anzahl an verbleibenden Karten zuerst zu sammeln                                  |
| 5    | HoursAscending            | Versuche Spiele mit der niedrigsten Anzahl an gespielten Stunden zuerst zu sammeln                                 |
| 6    | HoursDescending           | Versuche Spiele mit der höchsten Anzahl an gespielten Stunden zuerst zu sammeln                                    |
| 7    | NamesAscending            | Versuche Spiele in alphabetischer Reihenfolge zu sammeln, beginnend mit A                                          |
| 8    | NamesDescending           | Versuche Spiele in umgekehrter alphabetischer Reihenfolge zu sammeln, beginnend mit Z                              |
| 9    | Random                    | Versuche Spiele in einer komplett zufälligen Reihenfolge zu sammeln (unterschiedlich bei jedem Lauf des Programms) |
| 10   | BadgeLevelsAscending      | Versuche Spiele mit den niedrigsten Abzeichen-Level zuerst zu sammeln                                              |
| 11   | BadgeLevelsDescending     | Versuche Spiele mit den höchsten Abzeichen-Level zuerst zu sammeln                                                 |
| 12   | RedeemDateTimesAscending  | Versuche die ältesten Spiele auf unserem Konto zuerst zu sammeln                                                   |
| 13   | RedeemDateTimesDescending | Versuche die neuesten Spiele auf unserem Konto zuerst zu sammeln                                                   |
| 14   | MarketableAscending       | Versuche Spiele mit nicht marktfähigen Karten zuerst zu sammeln                                                    |
| 15   | MarketableDescending      | Versuche Spiele mit marktfähigen Karten zuerst zu sammeln                                                          |

Da es sich bei dieser Eigenschaft um ein Array handelt, kannst du mehrere verschiedene Einstellungen in einer festen Reihenfolge verwenden. Du kannst zum Beispiel Werte von `15`, `11` und `7` einbeziehen, um zuerst nach marktfähigen Spielen, dann nach denen mit dem höchsten Abzeichen-Level und schließlich alphabetisch zu sortieren. Wie du erraten kannst, ist die Reihenfolge tatsächlich wichtig, denn die umgekehrte Reihenfolge (`7`, `11` und `15`) erreicht etwas ganz anderes. Die Mehrheit der Benutzer wird wahrscheinlich nur eine Reihenfolge von allen verwenden, aber falls du möchtest, kannst du auch weiter nach zusätzlichen Parametern sortieren.

Achte auch darauf, dass das Wort "versuchen" in allen obigen Beschreibungen vorkommt - die tatsächliche ASF-Reihenfolge wird stark von ausgewählten **[Karten-Sammel-Algorithmus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE)** beeinflusst und die Sortierung wirkt sich nur auf Ergebnisse aus, die ASF als gleich leistungsmäßig betrachtet. So sollte beispielsweise in `Simple` der ausgewählte `FarmingOrders` in der aktuellen Sammel-Sitzung vollständig berücksichtigt werden (da jedes Spiel den gleichen Leistungswert hat), während in `Complex` der Algorithmus die tatsächliche Reihenfolge zuerst nach Stunden beeinflusst und dann nach der gewählten `FarmingOrders` sortiert wird. Dies führt zu unterschiedlichen Ergebnissen, da Spiele mit vorhandener Spielzeit eine Priorität gegenüber anderen haben, so dass ASF effektiv Spiele bevorzugt, die bereits die erforderlichen `HoursUntilCardDrops` durchlaufen haben. Erst dann werden diese Spiele nach dem von dir gewählten `FarmingOrders` weiter sortiert. Ebenso, sobald ASF keine bereits angestoßenen Spiele mehr hat, wird die verbleibende Warteschlange zuerst nach Stunden sortiert (da dies die Zeit, die für das Anstoßen eines der verbleibenden Titel benötigt wird, auf `HoursUntilCardDrops` verringert). Daher ist diese Konfigurationseigenschaft nur eine **Empfehlung**, die ASF zu respektieren versucht, solange sie die Leistung nicht negativ beeinflusst (in diesem Fall wird ASF immer die Sammel-Leistung gegenüber `FarmingOrders` bevorzugen).

Es gibt auch eine priorisierte Sammel-Warteschlange, die über die `iq` **[Befehle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** zugänglich ist. Wenn es verwendet wird, wird die tatsächliche Sammel-Reihenfolge zuerst nach Leistung, dann nach Sammel-Warteschlange und schließlich nach deinen `FarmingOrders` sortiert.

* * *

### `GamesPlayedWhileIdle`

`ImmutableHashSet<uint>` Typ mit einem leeren Standardwert. Wenn ASF nichts zu sammeln hat, kann es stattdessen deine angegebenen Steam Spiele (`appIDs`) spielen. Das Spielen von Spielen auf diese Weise erhöht deine "gespielten Stunden" dieser Spiele, aber nichts anderes als das. In order for this feature to work properly, your Steam account **must** own a valid license to all the `appIDs` that you specify here, this includes F2P games as well. Dieses Feature kann gleichzeitig mit `CustomGamePlayedWhileIdle` aktiviert werden, um die ausgewählten Spiele zu spielen, während der benutzerdefinierte Status im Steam-Netzwerk angezeigt wird, aber in diesem Fall, wie in `CustomGamePlayedWhileFarming`, ist die tatsächliche Anzeige-Reihenfolge nicht garantiert. Bitte beachte, dass Steam insgesamt ASF nur bis zu `32` `appIDs` spielen lässt, daher kannst du nur so viele Spiele in diese Eigenschaft aufnehmen.

* * *

### `HoursUntilCardDrops`

`byte` Typ mit einem Standardwert von `3`. Diese Eigenschaft legt fest, ob und wenn ja, für wie viele Anfangsstunden das Konto Kartendrops eingeschränkt hat. Eingeschränkte Kartendrops bedeuten, dass das Konto keine Karten von einem bestimmten Spiel erhält, bis das Spiel mindestens `HoursUntilCardDrops` Stunden lang gespielt wurde. Leider gibt es keinen magischen Weg, das zu erkennen, also verlässt sich ASF auf dich. Diese Eigenschaft wirkt sich auf den **[Karten-Sammel-Algorithmus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE)** aus, der verwendet wird. Wenn du diese Eigenschaft richtig einstellst, maximierst du den Gewinn und minimierst die Zeit, die für die Bearbeitung der Karten benötigt wird. Bedenke, dass es keine offensichtliche Antwort gibt, ob du den einen oder anderen Wert verwenden solltest, da es völlig von deinem Konto abhängt. Es scheint, dass ältere Konten, die nie um Rückerstattung gebeten haben, unbeschränkte Kartendrops haben, also solltest du einen Wert von `0` verwenden, während neue Konten und diejenigen, die um Rückerstattung gebeten haben, beschränkte Kartendrops mit einem Wert von `3` haben. Dies ist jedoch nur eine Theorie und sollte nicht als Tatsache betrachtet werden. Der Standardwert für diese Eigenschaft wurde basierend auf einem "kleinerem Übel" und der Mehrheit der Anwendungsfälle festgelegt.

* * *

### `IdlePriorityQueueOnly`

`bool` Typ mit einem Standardwert von `false`. Diese Eigenschaft legt fest, ob ASF für das automatische Sammeln nur Anwendungen berücksichtigen soll, die du selbst zur priorisierten Sammel-Warteschlange hinzugefügt hast, die mit `iq` **[Befehlen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** verfügbar ist. Wenn diese Option aktiviert ist, überspringt ASF alle `appIDs`, die auf der Liste fehlen, was es dir effektiv ermöglicht, Spiele für das automatische ASF-Sammeln auszuwählen. Bedenke, dass, wenn du keine Spiele zur Warteschlange hinzugefügt hast, ASF so tut, als gäbe es für dein Konto nichts zu tun. Wenn du dir nicht sicher bist ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

* * *

### `IdleRefundableGames`

`bool` Typ mit einem Standardwert von `true`. Diese Eigenschaft legt fest, ob es ASF erlaubt ist, Spiele zu sammeln, die noch erstattungsfähig sind. Ein rückerstattungsfähiges Spiel ist ein Spiel, das du in den letzten 2 Wochen über den Steam-Shop gekauft hast und das wir noch nicht länger als 2 Stunden gespielt haben, wie auf der **[Steam-Rückerstattungen](https://store.steampowered.com/steam_refunds)** Seite angegeben. Standardmäßig, wenn diese Option auf `true` gesetzt ist, ignoriert ASF die Steam-Rückerstattungen vollständig und Sammelt alles, wie es die meisten Benutzer erwarten würden. Du kannst diese Option jedoch auf `false` setzen, wenn du sicherstellen möchtest, dass ASF keines deiner rückerstattungsfähigen Spiele zu früh sammelt, so dass du diese Spiele selbst bewerten und bei Bedarf zurückerstatten kannst, ohne dir Sorgen zu machen, dass ASF die Spielzeit negativ beeinflusst. Bitte bedenke, dass, wenn du diese Option deaktivierst, Spiele, die du im Steam-Shop gekauft hast, bis zu 14 Tage nach dem Einlösedatum nicht von ASF gesammelt werden, was als nichts zu sammeln angezeigt wird, wenn dein Konto nichts anderes besitzt. Wenn du dir nicht sicher bist, ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `true`.

* * *

### `LootableTypes`

`ImmutableHashSet<byte>` Typ mit einem Standardwert von `1, 3, 5` Steam-Gegenstands-Typen. Diese Eigenschaft definiert das ASF-Verhalten beim Plündern - sowohl manuell, durch Verwendung eines **[Befehls](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)**, als auch automatisch über eine oder mehrere Konfigurationseigenschaften. ASF wird sicherstellen, dass nur Gegenstände von `LootableTypes` in ein Handelsangebot aufgenommen werden, daher kannst du mit dieser Eigenschaft wählen, was du in einem Handelsangebot erhalten möchtest, das an dich gesendet wird.

| Wert | Name                  | Beschreibung                                                                                           |
| ---- | --------------------- | ------------------------------------------------------------------------------------------------------ |
| 0    | Unknown               | Jeder Typ, der nicht in eine der folgenden Kategorien passt                                            |
| 1    | BoosterPack           | Booster Pack mit 3 zufälligen Karten aus einem Spiel                                                   |
| 2    | Emoticon              | Emoticon zur Verwendung im Steam-Chat                                                                  |
| 3    | FoilTradingCard       | Glanz-Variante von `TradingCard`                                                                       |
| 4    | ProfileBackground     | Profilhintergrund zur Verwendung in deinem Steam-Profil                                                |
| 5    | TradingCard           | Steam-Sammel-Karte, die für die Herstellung von Abzeichen (Nicht-Glanz) verwendet werden               |
| 6    | SteamGems             | Steam-Edelsteine, die für die Herstellung von Booster Packs verwendet werden, inklusive Edelsteinsäcke |
| 7    | SaleItem              | Spezialgegenstände, die während des Steam-Sales vergeben werden                                        |
| 8    | Consumable            | Spezielle Verbrauchsgegenstände, die nach dem Gebrauch wieder verschwinden                             |
| 9    | ProfileModifier       | Spezialgegenstände, welche das Aussehen des Steam-Profils verändern können                             |
| 10   | Aufkleber             | Spezialgegenstände, welche im Steam-Chat verwendet werden können                                       |
| 11   | ChatEffect            | Spezialgegenstände, welche im Steam-Chat verwendet werden können                                       |
| 12   | MiniProfilhintergrund | Besonderer Hintergrund für Steam Profile                                                               |

Bitte bedenke, dass ASF unabhängig von den obigen Einstellungen nur nach Steam (`appID` von 753) Community (`contextID` von 6) Gegenständen fragt, so dass alle Spiel-Gegenstände und Geschenke und dergleichen per Definition aus dem Handelsangebot ausgeschlossen sind.

Die Standard-ASF-Einstellung basiert auf der gebräuchlichsten Verwendung des Bot, wobei nur Boosterpacks und Trading-Karten (einschließlich Folienkarten) geöffnet werden. Die hier definierte Eigenschaft ermöglicht es dir, dieses Verhalten so zu verändern, dass es dich zufrieden stellt. Bitte bedenke, dass alle nicht oben definierten Typen als `Unknown` Typ angezeigt werden, was besonders wichtig ist, wenn Valve einen neuen Steam-Gegenstand veröffentlicht, der ebenfalls von ASF als `Unknown` markiert wird, bis er hier (in der zukünftigen Version) hinzugefügt wird. Deshalb ist es im Allgemeinen nicht empfehlenswert, `Unknown` in deinen `LootableTypes` einzugeben, es sei denn, du weißt, was du tust, und du verstehst auch, dass ASF dein gesamtes Inventar in einem Handelsangebot versenden wird, wenn das Steam-Netzwerk wieder unterbrochen wird und alle deine Gegenstände als `Unknown` meldet. My strong suggestion is to not include `Unknown` type in the `LootableTypes`, even if you expect to loot everything (else).

* * *

### `MatchableTypes`

`ImmutableHashSet<byte>` Typ mit einem Standardwert von `5` Steam-Gegenstands-Typen. Diese Eigenschaft definiert, welche Steam Gegenstands-Typen angepasst werden dürfen, wenn die Option `SteamTradeMatcher` in `TradingPreferences` aktiviert ist. Die Typen sind wie folgt definiert:

| Wert | Name                  | Beschreibung                                                                                  |
| ---- | --------------------- | --------------------------------------------------------------------------------------------- |
| 0    | Unknown               | Jeder Typ, der nicht in eine der folgenden Kategorien passt                                   |
| 1    | BoosterPack           | Booster Pack mit 3 zufälligen Karten aus einem Spiel                                          |
| 2    | Emoticon              | Emoticon zur Verwendung im Steam-Chat                                                         |
| 3    | FoilTradingCard       | Folienvariante von `TradingCard`                                                              |
| 4    | ProfileBackground     | Profilhintergrund zur Verwendung in deinem Steam-Profil                                       |
| 5    | TradingCard           | Steam-Karte, die für die Herstellung von Abzeichen (Nicht-Folie) verwendet werden             |
| 6    | SteamGems             | Steam-Edelsteine, die für die Herstellung von Booster Packs verwendet werden, inklusive Säcke |
| 7    | SaleItem              | Spezialgegenstände, die während des Steam-Sales vergeben werden                               |
| 8    | Consumable            | Spezielle Verbrauchsgegenstände, die nach dem Gebrauch wieder verschwinden                    |
| 9    | ProfileModifier       | Spezialgegenstände, welche das Aussehen des Steam-Profils verändern können                    |
| 10   | Aufkleber             | Spezialgegenstände, welche im Steam-Chat verwendet werden können                              |
| 11   | ChatEffect            | Spezialgegenstände, welche im Steam-Chat verwendet werden können                              |
| 12   | MiniProfilhintergrund | Besonderer Hintergrund für Steam Profile                                                      |

Natürlich beinhalten die Typen, die du für diese Eigenschaft verwenden solltest, typischerweise nur `2`, `3`, `4` und `5`, da nur diese Typen von STM unterstützt werden. ASF beinhaltet die passende Logik, um die Seltenheit der Gegenstände zu ermitteln, daher ist es auch sicher, Emoticons oder Hintergründe zu vergleichen, da ASF nur die Gegenstände aus dem gleichen Spiel und Typ, die auch die gleiche Seltenheit aufweisen, als fair erachten wird.

Bitte bedenke, dass **ASF kein Handels-Bot** ist und **sich NICHT um den Marktpreis schert.**. Wenn du Gegenstände der gleichen Seltenheit aus dem gleichen Set nicht als preislich gleichwertig betrachtest, dann ist diese Option NICHT für dich. Bitte überlege zweimal, ob du diese Erklärung verstehst und damit einverstanden bist, bevor du dich entscheidest, diese Einstellung zu ändern.

Wenn du nicht weißt, was du tust, solltest du es bei dem Standardwert `5` belassen.

* * *

### `OnlineStatus`

`byte` Typ mit einem Standardwert von `1`. Diese Eigenschaft gibt den Status der Steam-Community an, mit dem der Bot nach der Anmeldung im Steam-Netzwerk angekündigt wird. Derzeit kannst du einen der folgenden Stati wählen:

| Wert | Name           |
| ---- | -------------- |
| 0    | Offline        |
| 1    | Online         |
| 2    | Busy           |
| 3    | Away           |
| 4    | Snooze         |
| 5    | LookingToTrade |
| 6    | LookingToPlay  |
| 7    | Invisible      |

`Offline` Status ist extrem nützlich für primäre Konten. Wie du wissen solltest, zeigt das Sammeln eines Spiels tatsächlich deinen Steam-Status als "Spielt: XXX" an, was für deine Freunde irreführend sein kann und sie verwirrt, dass du ein Spiel spielst, während du es eigentlich nur zum Sammeln nutzt. Die Verwendung von `Offline` Status löst dieses Problem - Dein Konto wird nie als "im Spiel" angezeigt, wenn du Steam-Karten mit ASF sammelst. Dies ist möglich dank der Tatsache, dass sich ASF nicht in Steam Community anmelden muss, um richtig zu funktionieren, also spielen wir tatsächlich diese Spiele, sind mit dem Steam-Netzwerk verbunden, aber ohne unsere Online-Präsenz überhaupt bekannt zu geben. Bedenke, dass gespielte Spiele im Offline-Status immer noch für deine Spielzeit zählen und in deinem Profil als "kürzlich gespielt" angezeigt werden.

Darüber hinaus ist diese Funktion auch wichtig, wenn du Benachrichtigungen und ungelesene Nachrichten erhalten möchtest, während ASF läuft, ohne den Steam-Client gleichzeitig offen zu halten. Dies liegt daran, dass ASF selbst als Steam-Client fungiert, und ob ASF es möchte oder nicht, Steam sendet all diese Nachrichten und andere Ereignisse an ihn. Dies ist kein Problem, wenn du sowohl ASF als auch deinen eigenen Steam-Client laufen hast, da beide Clients genau die gleichen Ereignisse empfangen. Wenn jedoch nur ASF läuft, könnte das Steam-Netzwerk bestimmte Ereignisse und Nachrichten als "zugestellt" markieren, obwohl dein herkömmlicher Steam-Client sie nicht erhält, weil er nicht anwesend ist. Der Offline-Status löst auch dieses Problem, da ASF in diesem Fall nie für Community-Events in Betracht gezogen wird, so dass alle ungelesenen Nachrichten und andere Ereignisse bei deiner Rückkehr ordnungsgemäß als ungelesen markiert werden.

Es ist wichtig zu beachten, dass ASF, das im `Offline` Modus läuft, **nicht** in der Lage sein wird, Befehle auf herkömmliche Weise im Steam-Chat zu empfangen, da der Chat sowie die gesamte Community-Präsenz in der Tat völlig offline ist. Eine Lösung für dieses Problem ist die Verwendung des Modus `Invisible`, der auf ähnliche Weise funktioniert (nicht den Status offenbart), aber die Fähigkeit behält, Nachrichten zu empfangen und zu beantworten (also auch das Potenzial, Benachrichtigungen und ungelesene Nachrichten wie oben beschrieben zu verwerfen). Der Modus `Invisible` ist am sinnvollsten bei Alternativ-Konten, die du nicht anzeigen möchtest (statusmäßig), aber trotzdem in der Lage bist, Befehle zu senden.

Es gibt jedoch einen Haken beim `Invisible` Modus - er funktioniert nicht gut mit primären Konten. Dies liegt daran, dass **jegliche** Steam Session, die derzeit online ist den Status **anzeigt**, auch wenn ASF die selbst nicht tut. Dies wird durch die Strombegrenzung/Fehler des Steam-Netzwerks verursacht, die auf der ASF-Seite nicht behoben werden können, so dass du, wenn du den `Invisible`-Modus verwenden möchtest, auch sicherstellen musst, dass **alle** andere Sitzungen zum gleichen Konto auch den `Invisible`-Modus verwenden. Dies wird der Fall sein bei Alternativ-Konten, bei denen ASF hoffentlich die einzige aktive Sitzung ist, aber bei primären Konten wirst du es fast immer vorziehen, deine Freunden als `Online` anzuzeigen, indem du nur ASF-Aktivität versteckst, und in diesem Fall wird der `Invisible`-Modus für dich völlig nutzlos sein (wir empfehlen, stattdessen den `Offline`-Modus zu verwenden). Hoffentlich wird diese Beschränkung/Fehler in Zukunft von Valve behoben, aber ich würde nicht erwarten, dass das irgendwann bald passieren wird...

Wenn du dir nicht sicher bist, wie du diese Eigenschaft einrichten sollst, wird empfohlen, einen Wert von `0` (`Offline`) für primäre Konten und den Standard `1` (`Online`) anderweitig zu verwenden.

* * *

### `PasswordFormat`

`byte` Typ mit einem Standardwert von `0`. Diese Eigenschaft definiert das Format der Eigenschaft `SteamPassword` und unterstützt derzeit - `0` für `PlainText`, `1` für `AES` und `2` für `ProtectedDataForCurrentUser`. Bitte lies den Abschnitt **[Sicherheit](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-de-DE)**, wenn du mehr erfahren möchtest, da du sicherstellen musst, dass die Eigenschaft `SteamPassword` tatsächlich ein Passwort im passenden `PasswordFormat` enthält. Mit anderen Worten, wenn du `PasswordFormat` änderst, dann sollte dein `SteamPassword` **bereits** in diesem Format sein und nicht nur darauf abzielen, es zu sein. Wenn du nicht weißt, was du tust, solltest du es bei dem Standardwert `0` belassen.

* * *

### `Paused`

`bool` Typ mit einem Standardwert von `false`. Diese Eigenschaft definiert den Anfangszustand des `CardsFarmer` Moduls. Mit dem Standardwert `false` startet der Bot automatisch das Sammeln, wenn er gestartet wird, entweder wegen `Enabled` oder wegen dem `start` Befehl. Das Umschalten dieser Eigenschaft auf `true` sollte nur dann erfolgen, wenn du manuell den `resume` Befehl verwenden willst um den automatischen Sammel-Prozess wieder zu starten, z.B. weil du `play` ständig verwenden willst und niemals automatisch `CardsFarmer` Modul verwendest - das funktioniert genau wie der `pause` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)**. Wenn du dir nicht sicher bist ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

* * *

### `RedeemingPreferences`

`byte flags` Typ mit einem Standardwert von `0`. Diese Eigenschaft definiert das ASF-Verhalten beim Einlösen von Produktschlüsseln und ist wie folgt definiert:

| Wert | Name                               | Beschreibung                                                                                                                    |
| ---- | ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| 0    | None                               | Keine speziellen Einlöse-Einstellungen, Standard                                                                                |
| 1    | Forwarding                         | Leite Produktschlüssel, die nicht zum Einlösen verfügbar sind, zu anderen Bots weiter                                           |
| 2    | Distributing                       | Verteile alle Produktschlüssel auf sich und andere Bots                                                                         |
| 4    | KeepMissingGames                   | Bewahre Produktschlüssel für (potenziell) fehlende Spiele beim Weiterleiten auf und lasse sie ungenutzt                         |
| 8    | AssumeWalletKeyOnBadActivationCode | Assume that `BadActivationCode` keys are equal to `CannotRedeemCodeFromClient`, and therefore try to redeem them as wallet keys |

Bitte bedenke, dass diese Eigenschaft das Feld `flags` ist, daher ist es möglich, eine beliebige Kombination von verfügbaren Werten auszuwählen. Schau dir **[JSON-Mapping](#json-mapping)** an, wenn du mehr darüber erfahren möchtest. Wenn keines der Flags aktiviert wird, wird die Option `None` verwendet.

`Forwarding` veranlasst den Bot, einen Produktschlüssel, der nicht eingelöst werden kann, an einen anderen verbundenen und angemeldeten Bot weiterzuleiten, dem dieses bestimmte Spiel fehlt (wenn es möglich ist, dies zu überprüfen). Die häufigste Situation ist die Weiterleitung von `AlreadyPurchased` Spiel an einen anderen Bot, dem dieses Spiel fehlt, aber diese Option deckt auch andere Szenarien ab, wie `DoesNotOwnRequiredApp`, `RateLimited` oder `RestrictedCountry`.

`Distributing` veranlasst den Bot, alle empfangenen Produktschlüssel unter sich und anderen Bots zu verteilen. Das bedeutet, dass jeder Bot einen einzigen Produktschlüssel aus der Charge erhält. Normalerweise wird dies nur verwendet, wenn du viele Produktschlüssel für das gleiche Spiel einlöst und du sie gleichmäßig auf deine Bots verteilen möchtest, im Gegensatz zum Einlösen von Produktschlüsseln für verschiedene Spiele. Diese Funktion ist nicht sinnvoll, wenn du nur einen Produktschlüssel in einer einzigen `redeem` Aktion einlöst (da es keine zusätzlichen Produktschlüssel gibt, die verteilt werden müssen).

`KeepMissingGames` veranlasst den Bot, `Forwarding` zu überspringen, wenn wir nicht sicher sein können, ob der Produktschlüssel, der eingelöst wird, tatsächlich unserem Bot gehört oder nicht. Dies bedeutet im Grunde, dass `Forwarding` **nur** für `AlreadyPurchased` Produktschlüssel gilt, anstatt auch andere Fälle wie `DoesNotOwnRequiredApp`, `RateLimited` oder `RestrictedCountry` zu behandeln. Normalerweise solltest du diese Option für das primäre Konto verwenden, um sicherzustellen, dass Produktschlüssel, die auf dem primären Konto eingelöst werden, nicht weitergereicht werden, wenn dein Bot beispielsweise vorübergehend `RateLimited` wird. Wie Sie der Beschreibung entnehmen können, hat dieses Feld absolut keine Wirkung, wenn `Forwarding` nicht aktiviert ist.

`AssumeWalletKeyOnBadActivationCode` will cause `BadActivationCode` keys to be treated as `CannotRedeemCodeFromClient`, and therefore result in ASF trying to redeem them as wallet keys. This is needed because Steam might announce wallet keys as `BadActivationCode` (and not `CannotRedeemCodeFromClient` as it used to), resulting in ASF never attempting to redeem them. However, we recommend **against** using this preference, as it'll result in ASF trying to redeem every invalid key as a wallet code, resulting in excessive amount of (potentially invalid) requests sent to the Steam service, with all the potential consequences. Instead, we recommend to use `ForceAssumeWalletKey` **[`redeem^`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands#redeem-modes)** mode while knowingly redeeming wallet keys, which will enable the needed workaround only when it's required, on as-needed basis.

Wenn du sowohl `Forwarding` als auch `Distributing` aktivierst, wird zusätzlich zur Weiterleitung eine Verteilungsfunktion hinzugefügt, wodurch ASF versucht, einen Produktschlüssel auf allen Bots zuerst einzulösen (Weiterleitung), bevor es zum nächsten geht (Verteilung). Normalerweise möchtest du diese Option nur verwenden, wenn du `Forwarding` möchtest, aber mit geändertem Verhalten beim Einschalten des verwendeten Bot auf Produktschlüssel, anstatt immer mit jedem Produktschlüssel in Reihenfolge zu gehen (das wäre `Forwarding` allein). Dieses Verhalten kann von Vorteil sein, wenn du weißt, dass die Mehrheit oder sogar alle deine Produktschlüssel an das gleiche Spiel gebunden sind, denn in dieser Situation würde `Forwarding` allein versuchen, alles auf einem Bot einzulösen (was Sinn macht, wenn deine Produktschlüssel für einzigartige Spiele sind), und `Forwarding` + `Distributing` schaltet den Bot auf den nächsten Produktschlüssel um, "verteilt" die Aufgabe, einen neuen Produktschlüssel auf einen anderen als den ursprünglichen einzulösen (was sinnvoll ist, wenn die Produktschlüssel für das gleiche Spiel sind, überspringt einen sinnlosen Versuch pro Produktschlüssel).

Die tatsächliche Reihenfolge der Bots für alle Einlöseszenarien ist alphabetisch, mit Ausnahme von Bots, die nicht verfügbar sind (nicht verbunden, gestoppt oder ähnliches). Bitte bedenke, dass es ein stündliches Limit für Einlösungsversuche pro IP und pro Konto gibt, und jeder Einlösungsversuch, der nicht mit `OK` endete, trägt zu fehlgeschlagenen Versuchen bei. ASF wird sein Bestes tun, um die Anzahl der `AlreadyPurchased` Fehler zu minimieren, z.B. indem es versucht zu vermeiden, einen Produktschlüssel an einen anderen Bot weiterzuleiten, der bereits dieses bestimmte Spiel besitzt, aber es ist nicht immer garantiert, dass es funktioniert, aufgrund dessen, wie Steam mit Lizenzen umgeht. Die Verwendung von Einlöse-Flags wie `Forwarding` oder `Distributing` erhöht immer die Wahrscheinlichkeit, dass du `RateLimited` erreichst.

Bedenke auch, dass du keine Produktschlüssel an Bots weiterleiten oder verteilen kannst, auf die du keinen Zugriff hast. Dies sollte offensichtlich sein, aber stelle sicher, dass du mindestens `Operator` aller Bots bist, die du in deinem Einlösungsprozess einbeziehen möchtest, z.B. mit `status ASF` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)**.

* * *

### `SendOnFarmingFinished`

`bool` Typ mit einem Standardwert von `false`. Wenn ASF mit dem Sammeln des angegebenen Kontos fertig ist, kann es automatisch ein Steam-Handelsangebot mit allem, was bis zu diesem Punkt gesammelt wurde, an den Benutzer mit der Berechtigung `Master` senden, was sehr praktisch ist, wenn du dich nicht selbst mit den Handelsangeboten beschäftigen möchtest. Diese Option funktioniert genauso wie der Befehl `loot`, deshalb denken Sie daran, dass Sie einen Benutzer mit der Berechtigung `Master` benötigen, Sie können auch einen gültigen `SteamTradeToken` benötigen, sowie ein Konto, das überhaupt zum Handel zugelassen ist. Zusätzlich zum Einleiten von `loot` nach Beendigung des Sammelns initiiert ASF auch `loot` bei jeder Benachrichtigung über neue Gegenstände (wenn nicht am Sammeln) und nach Abschluss jedes Handelsangebotes, der zu neuen Gegenständen führt (immer), wenn diese Option aktiv ist. Dies ist besonders nützlich, um von anderen Personen erhaltene Gegenstände auf unser Konto "weiterzuleiten".

Normalerweise solltest du **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** zusammen mit dieser Funktion verwenden, obwohl es keine Voraussetzung ist, wenn du beabsichtigst, manuell und rechtzeitig zu bestätigen. Wenn du dir nicht sicher bist, wie du diese Eigenschaft einstellen sollst, belasse sie bei dem Standardwert `false`.

* * *

### `SendTradePeriod`

`byte` Typ mit einem Standardwert von `0`. Diese Eigenschaft funktioniert sehr ähnlich wie `SendOnFarmingFinished` Eigenschaft, mit einem Unterschied - anstatt Handelsangebote zu senden, wenn das Sammeln abgeschlossen ist, können wir sie auch alle `SendTradePeriod` Stunden senden, unabhängig davon, wie viel wir noch zu sammeln haben. Dies ist nützlich, wenn du den normalen `loot` Befehl auf deinen Alternativ-Konten ausführen willst, anstatt darauf zu warten, dass sie das Sammeln beenden. Der Standardwert von `0` deaktiviert diese Funktion, wenn du möchtest, dass dein Bot dir z.B. jeden Tag Handelsangebote sendet, solltest du `24` hier eintragen.

Normalerweise solltest du **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** zusammen mit dieser Funktion verwenden, obwohl es keine Voraussetzung ist, wenn du beabsichtigst, manuell und rechtzeitig zu bestätigen. Wenn du dir nicht sicher bist, wie du diese Eigenschaft einstellen sollst, belasse sie bei dem Standardwert `0`.

* * *

### `ShutdownOnFarmingFinished`

`bool` Typ mit einem Standardwert von `false`. ASF "belegt" ein Konto für die gesamte Zeit des aktiven Prozesses. Wenn ein Konto mit dem Sammeln fertig ist, überprüft ASF regelmäßig (jede `IdleFarmingPeriode` Stunden), ob in der Zwischenzeit vielleicht einige neue Spiele mit Steam-Karten hinzugefügt wurden, damit es das Sammeln dieses Kontos fortsetzen kann, ohne den Prozess neu starten zu müssen. Dies ist für die Mehrheit der Menschen nützlich, da ASF bei Bedarf automatisch das Sammeln wieder aufnehmen kann. Jedoch kannst du den Prozess tatsächlich stoppen wollen, wenn das angegebene Konto vollständig gesammelt ist, du kannst das erreichen, indem du diese Eigenschaft auf `true` setzt. Wenn aktiviert, fährt ASF mit der Abmeldung fort, wenn das Konto vollständig gesammelt ist, was bedeutet, dass es nicht mehr regelmäßig überprüft oder belegt wird. Du solltest selbst entscheiden, ob du es vorziehst, dass ASF die ganze Zeit an einer bestimmten Bot-Instanz arbeitet, oder ob ASF sie vielleicht stoppen sollte, wenn das Sammeln abgeschlossen ist. Wenn alle Konten gestoppt sind und der Prozess nicht in `--process-required` **[Modus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments-de-DE)** läuft, wird ASF auch heruntergefahren, wodurch deine Maschine in den Ruhemodus geht und du andere Aktionen planen kannst wie z.B. Schlafmodus oder Herunterfahren sobald die letzte Karte gesammelt wurde.

Wenn du dir nicht sicher bist, wie du diese Eigenschaft einstellen sollst, belasse sie bei dem Standardwert `false`.

* * *

### `SteamLogin`

`string` Typ mit einem Standardwert von `null`. Diese Eigenschaft definiert dein Steam-Login - das, mit dem du dich bei Steam anmeldest. Zusätzlich zur Definition des Steam-Login kannst du hier auch den Standardwert `null` beibehalten, wenn du dein Steam-Login bei jedem ASF-Start eingeben möchtest, anstatt es in die Konfiguration einzutragen. Dies kann für dich nützlich sein, wenn du sensible Daten nicht in der Konfigurationsdatei speichern möchtest.

* * *

### `SteamMasterClanID`

`ulong` Typ mit einem Standardwert von `0`. Diese Eigenschaft definiert die steamID der Steam-Gruppe, der der Bot automatisch beitreten soll, einschließlich des Gruppen-Chats. Du kannst die steamID deiner Gruppe überprüfen, indem du zu ihrer **[Seite](https://steamcommunity.com/groups/archiasf)** navigierst und dann `/memberslistxml?xml=1` am Ende des Links hinzufügst, so dass der Link wie **[dieser](https://steamcommunity.com/groups/archiasf/memberslistxml?xml=1)** aussehen wird. Dann kannst du die steamID deiner Gruppe aus dem Ergebnis erhalten, sie befindet sich im `<groupID64>` Tag. Im obigen Beispiel wäre es `103582791440160998`. Zusätzlich zu dem Versuch, einer bestimmten Gruppe beim Start beizutreten, akzeptiert der Bot auch automatisch Gruppeneinladungen zu dieser Gruppe, so dass du deinen Bot manuell einladen kannst, wenn deine Gruppe eine private Mitgliedschaft hat. Wenn du keine Gruppe für deine Bots hast, solltest du diese Eigenschaft mit dem Standardwert `0` behalten.

* * *

### `SteamParentalCode`

`string` Typ mit einem Standardwert von `null`. Diese Eigenschaft definiert deinen Steam-Familienansicht-Code. ASF requires an access to resources protected by steam parental, therefore if you use that feature, you should provide ASF with parental unlock PIN, so it can operate normally. Der Standardwert von `null` bedeutet, dass für die Freischaltung dieses Kontos kein Steam-Familienansicht-Code erforderlich ist, und das ist wahrscheinlich das, was du willst, wenn du nicht die Steam-Elternfunktionalität verwendest. In addition to defining steam parental PIN here, you may also use value of `0` if you want to enter your steam parental PIN on each ASF startup, when needed, instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

In limited circumstances, ASF is also able to generate a valid Steam parental code itself, although that requires excessive amount of OS resources and additional time to complete, not to mention that it's not guaranteed to succeed, therefore we recommend to not rely on that feature and instead put valid `SteamParentalCode` in the config for ASF to use.

* * *

### `SteamPassword`

`string` Typ mit einem Standardwert von `null`. Diese Eigenschaft definiert dein Steam-Passwort - das, mit dem du dich bei Steam anmeldest. Zusätzlich zur Definition des Steam-Passworts kannst du hier auch den Standardwert `null` beibehalten, wenn du dein Steam-Passwort bei jedem ASF-Start eingeben möchtest, anstatt es in die Konfiguration einzutragen. Dies kann für dich nützlich sein, wenn du sensible Daten nicht in der Konfigurationsdatei speichern möchtest.

* * *

### `SteamTradeToken`

`string` Typ mit einem Standardwert von `null`. Wenn du deinen Bot auf deiner Freundesliste hast, dann kann der Bot dir sofort ein Handelsangebot schicken, ohne sich um den Handels-Code zu kümmern, deshalb kannst du diese Eigenschaft auf dem Standardwert von `null` belassen. Wenn du dich jedoch dazu entscheidest, deinen Bot NICHT auf deiner Freundesliste zu haben, dann musst du einen Handels-Code dessen Benutzers generieren und eintragen an den dieser Bot Handelsangebote senden möchte. Mit anderen Worten, diese Eigenschaft sollte mit dem Handels-Code des Kontos gefüllt werden, das mit `Master` Berechtigung in `SteamUserPermissions` von **dieser** Bot-Instanz definiert ist.

Um deinen Code zu finden, navigiere als angemeldeter Benutzer mit der Berechtigung `Master` **[hier](https://steamcommunity.com/my/tradeoffers/privacy)** und schau dir deine Handels URL an. Der Code, nach dem wir suchen, besteht aus 8 Zeichen nach `&token=` Teil in deiner Handels-URL. Du solltest diese 8 Zeichen kopieren und hier einfügen, als `SteamTradeToken`. Füge nicht die gesamte Handels URL ein, auch nicht den `&token=` Teil, nur den Code selbst (8 Zeichen).

* * *

### `SteamUserPermissions`

` ImmutableDictionary<ulong, byte>` Typ mit einem leeren Standardwert. Diese Eigenschaft ist eine Dictionary-Eigenschaft, die den Steam-Benutzer, der durch seine 64-Bit-Steam-ID identifiziert wurde, auf die Nummer `byte` umbildet, die seine Berechtigung in ASF-Instanz angibt. Die derzeit verfügbaren Bot-Berechtigungen in ASF sind definiert als:

| Wert | Name          | Beschreibung                                                                                                                                                                                                                                    |
| ---- | ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | None          | Keine spezielle Berechtigung, dies ist hauptsächlich ein Referenzwert, der den in diesem Verzeichnis fehlenden Steam-IDs zugeordnet wird - es ist nicht notwendig, irgendjemanden mit dieser Berechtigung zu definieren                         |
| 1    | FamilySharing | Bietet einen minimalen Zugriff für Familienmitglieder. Auch hier handelt es sich hauptsächlich um einen Referenzwert, da ASF in der Lage ist, Steam-IDs, die wir für die Nutzung unserer Bibliothek freigegeben haben, automatisch zu entdecken |
| 2    | Operator      | Bietet grundlegenden Zugriff auf gegebene Bot-Instanzen, hauptsächlich das Hinzufügen von Lizenzen und das Einlösen von Produktschlüsseln                                                                                                       |
| 3    | Master        | Bietet vollen Zugriff auf die gegebene Bot-Instanz                                                                                                                                                                                              |

Kurz gesagt, diese Eigenschaft ermöglicht es dir, Berechtigungen für bestimmte Benutzer zu verwalten. Berechtigungen sind vor allem für den Zugriff auf ASF **[Befehle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** wichtig, aber auch für die Aktivierung vieler ASF-Funktionen, wie z.B. die Annahme von Handelsangeboten. Zum Beispiel könntest du dein eigenes Konto als `Master` einrichten und `Operator` Zugang zu 2-3 deiner Freunde geben, damit sie leicht Produktschlüssel für deinen Bot mit ASF einlösen können, während sie **nicht** berechtigt sind, z.B. ihn zu stoppen. Dank dessen kannst du bestimmten Benutzern einfach Berechtigungen zuweisen und sie deinen Bot verwenden lassen.

Wir empfehlen, genau einen Benutzer als `Master` und eine beliebige Anzahl von `Operators` und darunter einzutragen. Während es technisch möglich ist, mehrere `Masters` einzustellen und ASF wird korrekt mit ihnen arbeiten, z.B. indem es alle ihre an den Bot gesendeten Handelsangebote akzeptiert, wird ASF nur einen von ihnen (mit der niedrigsten Steam-ID) für jede Aktion verwenden, die ein einzelnes Ziel erfordert, z.B. eine `loot` Anfrage, also auch Eigenschaften wie `SendOnFarmingFinished` oder `SendTradePeriod`. Wenn du diese Einschränkungen perfekt verstehst, insbesondere die Tatsache, dass die `loot` Anfrage immer Elemente an den `Master` mit der niedrigsten Steam-ID sendet, unabhängig von dem `Master`, der den Befehl tatsächlich ausgeführt hat, dann kannst du hier mehrere Benutzer mit der Berechtigung `Master` definieren, aber wir empfehlen trotzdem ein einziges Master-Schema - mehrere Master-Schemata sind von einem Setup abzuraten, das nicht unterstützt wird.

Es ist schön zu beachten, dass es noch eine weitere zusätzliche `Owner` Berechtigung gibt, die als `SteamOwnerID` globale Konfigurationseigenschaft deklariert ist. Du kannst hier niemandem die Berechtigung `Owner` zuweisen, da die Eigenschaft `SteamUserPermissions` nur Berechtigungen definiert, die sich auf die Bot-Instanz beziehen, und nicht ASF als Prozess. Für Bot-bezogene Aufgaben wird die `SteamOwnerID` genauso behandelt wie `Master`, so dass die Angabe deiner `SteamOwnerID` hier nicht erforderlich ist.

* * *

### `TradingPreferences`

`byte flags` Typ mit einem Standardwert von `0`. Diese Eigenschaft definiert das ASF-Verhalten beim Handeln und ist wie folgt definiert:

| Wert | Name                | Beschreibung                                                                                                                                                                                                                     |
| ---- | ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | None                | Keine besonderen Handelspräferenzen, Standard                                                                                                                                                                                    |
| 1    | AcceptDonations     | Akzeptiert Handelsangebote, bei denen wir nichts verlieren                                                                                                                                                                       |
| 2    | SteamTradeMatcher   | Nimmt passiv an **[STM](https://www.steamtradematcher.com)**-artigen Handelsangeboten teil. Besuche **[Handeln](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-de-DE#steamtradematcher)** für weitere Informationen |
| 4    | MatchEverything     | Erfordert das Setzen von `SteamTradeMatcher` und akzeptiert dabei neben guten und neutralen auch schlechte Handelsangebote                                                                                                       |
| 8    | DontAcceptBotTrades | Akzeptiert keine automatischen `loot` Handelsangebote von anderen Bot-Instanzen                                                                                                                                                  |
| 16   | MatchActively       | Nimmt aktiv an **[STM](https://www.steamtradematcher.com)**-artigen Handelsangeboten teil. Besuche **[Handeln](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-de-DE#matchactively)** für weitere Informationen      |

Bitte bedenke, dass diese Eigenschaft das Feld `flags` ist, daher ist es möglich, eine beliebige Kombination von verfügbaren Werten auszuwählen. Schau dir **[JSON-Mapping](#json-mapping)** an, wenn du mehr darüber erfahren möchtest. Wenn keines der Flags aktiviert wird, wird die Option `None` verwendet.

Für weitere Erläuterungen zur ASF-Handelslogik und zur Beschreibung jedes verfügbaren Flags besuche bitte den Abschnitt **[Handeln](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-de-DE)**.

* * *

### `TransferableTypes`

`ImmutableHashSet<byte>` Typ mit einem Standardwert von `1, 3, 5` Steam-Gegenstands-Typen. Diese Eigenschaft definiert, welche Steam-Gegenstands-Typen für die Übertragung zwischen Bots während `transfer` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** berücksichtigt werden. ASF wird sicherstellen, dass nur Gegenstände von `TransferableTypes` in ein Handelsangebot aufgenommen werden, daher kannst du mit dieser Eigenschaft wählen, was du in einem Handelsangebot erhalten möchtest, das an einen deiner Bots gesendet wird.

| Wert | Name                  | Beschreibung                                                                                  |
| ---- | --------------------- | --------------------------------------------------------------------------------------------- |
| 0    | Unknown               | Jeder Typ, der nicht in eine der folgenden Kategorien passt                                   |
| 1    | BoosterPack           | Booster Pack mit 3 zufälligen Karten aus einem Spiel                                          |
| 2    | Emoticon              | Emoticon zur Verwendung im Steam-Chat                                                         |
| 3    | FoilTradingCard       | Folienvariante von `TradingCard`                                                              |
| 4    | ProfileBackground     | Profilhintergrund zur Verwendung in deinem Steam-Profil                                       |
| 5    | TradingCard           | Steam-Karte, die für die Herstellung von Abzeichen (Nicht-Folie) verwendet werden             |
| 6    | SteamGems             | Steam-Edelsteine, die für die Herstellung von Booster Packs verwendet werden, inklusive Säcke |
| 7    | SaleItem              | Spezialgegenstände, die während des Steam-Sales vergeben werden                               |
| 8    | Consumable            | Spezielle Verbrauchsgegenstände, die nach dem Gebrauch wieder verschwinden                    |
| 9    | ProfileModifier       | Spezialgegenstände, welche das Aussehen des Steam-Profils verändern können                    |
| 10   | Aufkleber             | Spezialgegenstände, welche im Steam-Chat verwendet werden können                              |
| 11   | ChatEffect            | Spezialgegenstände, welche im Steam-Chat verwendet werden können                              |
| 12   | MiniProfilhintergrund | Besonderer Hintergrund für Steam Profile                                                      |

Bitte bedenke, dass ASF unabhängig von den obigen Einstellungen nur nach Steam (`appID` von 753) Community (`contextID` von 6) Gegenständen fragt, so dass alle Spiel-Gegenstände und Geschenke und dergleichen per Definition aus dem Handelsangebot ausgeschlossen sind.

Die Standard-ASF-Einstellung basiert auf der gebräuchlichsten Verwendung des Bot, wobei nur Booster Packs und Trading Cards (einschließlich Folienkarten) gehandelt werden. Die hier definierte Eigenschaft ermöglicht es dir, dieses Verhalten so zu verändern, dass es dich zufrieden stellt. Bitte bedenke, dass alle nicht oben definierten Typen als `Unknown` Typ angezeigt werden, was besonders wichtig ist, wenn Valve einen neuen Steam-Gegenstand veröffentlicht, der ebenfalls von ASF als `Unknown` markiert wird, bis er hier (in der zukünftigen Version) hinzugefügt wird. Deshalb ist es im Allgemeinen nicht empfehlenswert, `Unknown` in deinen `TransferableTypes` einzugeben, es sei denn, du weißt, was du tust, und du verstehst auch, dass ASF dein gesamtes Inventar in einem Handelsangebot versenden wird, wenn das Steam-Netzwerk wieder unterbrochen wird und alle deine Gegenstände als `Unknown` meldet. Mein nachdrücklicher Vorschlag ist, `Unknown` nicht in das Feld `TransferableTypes` einzutragen, selbst wenn du alles übertragen möchtest.

* * *

### `UseLoginKeys`

`bool` Typ mit einem Standardwert von `true`. Diese Eigenschaft legt fest, ob ASF den Anmelde-Schlüssel-Mechanismus für dieses Steam-Konto verwenden soll. Der Anmelde-Schlüssel-Mechanismus funktioniert sehr ähnlich wie die "Remember me"-Option des offiziellen Steam-Clients, die es ASF ermöglicht, einen temporären, einmaligen Anmelde-Schlüssel für den nächsten Anmeldeversuch zu speichern und zu verwenden, wodurch die Angabe von Passwort, Steam Guard oder 2FA-Code übergangen wird, solange unser Anmelde-Schlüssel gültig ist. Der Anmelde-Schlüssel wird in der Datei `BotName.db` gespeichert und automatisch aktualisiert. Aus diesem Grund musst du kein Passwort/SteamGuard/2FA-Code angeben, nachdem du dich nur einmal erfolgreich mit ASF angemeldet hast.

Die Anmelde-Schlüssel werden standardmäßig für deine Bequemlichkeit verwendet, so dass du nicht bei jedem Anmeldevorgang `SteamPassword`, SteamGuard oder 2FA-Code (wenn du nicht **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** verwendest) eingeben musst. Es ist auch eine überlegene Alternative, da der Anmelde-Schlüssel nur einmalig verwendet werden kann und dein ursprüngliches Passwort in keiner Weise offenbart. Genau die gleiche Methode wird von deinem ursprünglichen Steam-Client verwendet, der deinen Konto-Namen und Anmelde-Schlüssel für deinen nächsten Anmeldeversuch speichert, was praktisch die gleiche ist wie die Verwendung von `SteamLogin` mit `UseLoginKeys` und leerem `SteamPassword` in ASF.

Einige Leute sind jedoch vielleicht sogar über dieses kleine Detail besorgt, deshalb steht dir diese Option hier zur Verfügung, wenn du sicherstellen möchtest, dass ASF keine Art von Daten speichert, die es ermöglichen würden, die vorherige Sitzung nach dem Schließen wieder aufzunehmen, was dazu führt, dass eine vollständige Authentifizierung bei jedem Anmeldeversuch obligatorisch ist. Das Deaktivieren dieser Option funktioniert genau so, wie wenn du "Remember me" im offiziellen Steam-Client nicht aktivierst. Wenn du nicht weißt was du tust, solltest du es bei dem Standardwert `true` belassen.

* * *

## Dateistruktur

ASF benutzt eine recht einfache Dateistruktur.

    ├── config
    │     ├── ASF.json
    │     ├── ASF.db
    │     ├── Bot1.json
    │     ├── Bot1.db
    │     ├── Bot1.bin
    │     ├── Bot2.json
    │     ├── Bot2.db
    │     ├── Bot2.bin
    │     └── ...
    ├── ArchiSteamFarm.dll
    ├── log.txt
    └── ...
    

Um ASF an einen neuen Ort zu verschieben, z.B. auf einen anderen PC, genügt es, das Verzeichnis `config` allein zu verschieben/kopieren, und das ist die empfohlene Art und Weise, jede Form von "ASF-Backups" durchzuführen, da man immer den verbleibenden (Programm-)Teil von GitHub herunterladen kann, ohne zu riskieren, interne ASF-Dateien zu beschädigen, z.B. durch ein fehlerhaftes Backup.

Die `log.txt` Datei enthält das Protokoll, das durch deinen letzten ASF-Lauf erzeugt wurde. Diese Datei enthält keine sensiblen Informationen und ist äußerst nützlich, wenn es um Probleme, Abstürze oder einfach nur als Information an dich geht, was im letzten ASF-Lauf passiert ist. Wir werden sehr oft nach dieser Datei fragen, wenn du auf Probleme oder Fehler stößt. ASF verwaltet diese Datei automatisch für dich, aber du kannst die ASF **[Protokollierung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Logging-de-DE)**s-Modul weiter optimieren, wenn du ein fortgeschrittener Benutzer bist.

`config` Verzeichnis ist der Ort, der die Konfiguration für ASF enthält, einschließlich aller seiner Bots.

`ASF.json` ist eine globale ASF-Konfigurationsdatei. Diese Konfiguration wird verwendet, um anzugeben, wie sich ASF als Prozess verhält, was alle Bots und das Programm selbst betrifft. Dort findest du globale Eigenschaften wie ASF-Prozesseigentümer, automatische Aktualisierungen oder Debugging.

`BotName.json` ist eine Konfiguration der gegebenen Bot-Instanz. Diese Konfiguration wird verwendet, um das Verhalten einer gegebenen Bot-Instanz festzulegen, daher sind diese Einstellungen nur für diesen Bot spezifisch und nicht für andere bestimmt. Dies ermöglicht es dir, Bots mit verschiedenen Einstellungen zu konfigurieren, und nicht unbedingt alle funktionieren auf die gleiche Weise. Jeder Bot wird mit einer eindeutigen Kennzeichnung benannt, die du anstelle von `BotName` wählst.

Neben den Konfigurationsdateien verwendet ASF auch das Verzeichnis `config` zum Speichern von Datenbanken.

`ASF.db` ist eine globale ASF-Datenbankdatei. Es fungiert als globaler dauerhafter Speicher und wird zum Speichern verschiedener Informationen im Zusammenhang mit dem ASF-Prozess verwendet, wie z.B. IPs von lokalen Steam-Servern. **Du solltest diese Datei nicht bearbeiten**.

`BotName.db` ist eine Datenbank der jeweiligen Bot-Instanz. Diese Datei wird verwendet, um wichtige Daten der jeweiligen Bot-Instanz im dauerhaften Speicher zu speichern, wie z.B. Anmelde-Schlüssel oder ASF 2FA. **Du solltest diese Datei nicht bearbeiten**.

`BotName.bin` ist eine spezielle Datei der jeweiligen Bot-Instanz, die Informationen über den Steam-Sentry-Hash enthält. Der Sentry-Hash wird für die Authentifizierung mit dem `SteamGuard` Mechanismus verwendet, sehr ähnlich Steam-Datei `ssfn`. **Du solltest diese Datei nicht bearbeiten**.

`BotName.keys` ist eine spezielle Datei, die zum Importieren von Produkt-Schlüsseln in den **[Hintergrundproduktschlüsselaktivierer ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-de-DE)** verwendet werden kann. Es ist nicht zwingend erforderlich und wird nicht generiert, aber von ASF anerkannt. Diese Datei wird nach dem erfolgreichen Import der Produkt-Schlüssel automatisch gelöscht.

`BotName.maFile` ist eine spezielle Datei, die für den Import von **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** verwendet werden kann. Es ist nicht zwingend erforderlich und wird nicht generiert, aber von ASF erkannt, wenn dein `BotName` ASF 2FA noch nicht verwendet. Diese Datei wird nach erfolgreichem Import von ASF 2FA automatisch gelöscht.

* * *

## JSON-Mapping

Jede Konfigurationseigenschaft hat ihren Typ. Der Typ der Eigenschaft definiert Werte, die für sie gültig sind. Du kannst nur Werte verwenden, die für einen bestimmten Typ gültig sind - wenn du einen ungültigen Wert verwendest, dann kann ASF deine Konfiguration nicht verarbeiten.

**Wir empfehlen dringend, den ConfigGenerator für die Generierung von Konfigurationen zu verwenden** - er handhabt den größten Teil des Low-Level-Sachen (wie z.B. die Typ-Validierung) für dich, so dass du nur die richtigen Werte eingeben musst, und du musst auch die unten angegebenen Variablentypen nicht verstehen. Dieser Abschnitt ist hauptsächlich für Personen gedacht, die Konfigurationen manuell erstellen/bearbeiten, damit sie wissen, welche Werte sie verwenden können.

Die von ASF verwendeten Typen sind native C#-Typen, die im Folgenden aufgeführt werden:

* * *

`bool` - Boolean-Typ der nur die Werte `true` und `false` akzeptiert.

Beispiel: `"Enabled": true`

* * *

`byte` - Unsignierter Byte-Typ der nur ganze Zahlen von `0` bis `255` (einschließlich) akzeptiert.

Beispiel: `"ConnectionTimeout": 60`

* * *

`ushort` - Unsignierter Short-Typ der nur ganze Zahlen von `0` bis `65535` (einschließlich) akzeptiert.

Beispiel: `"WebLimiterDelay": 300`

* * *

`uint` - Unsignierter Integer-Typ der nur ganze Zahlen von `0` bis `4294967295` (einschließlich) akzeptiert.

* * *

`ulong` - Unsignierter Long-Integer-Typ der nur ganze Zahlen von `0` bis `18446744073709551615` (einschließlich) akzeptiert.

Beispiel: `"SteamMasterClanID": 103582791440160998`

* * *

`string` - String-Typ der jede beliebige Zeichenfolge akzeptiert, einschließlich einer leeren Zeichenfolge wie `""` und `null`. Leere Sequenz und `null` Wert werden von ASF gleich behandelt, so dass du es dir aussuchen kannst, welche du verwenden möchtest (wir bleiben bei `null`).

Beispiele: `"SteamLogin": null`, `"SteamLogin": ""`, `"SteamLogin": "MeinLogin"`

* * *

`ImmutableHashSet<valueType>` - Unveränderliche Sammlung (Set) von eindeutigen Werten in gegebenen `valueType`. In JSON ist es definiert als Array von Elementen in gegebenem `valueType`. ASF verwendet `HashSet`, um anzugeben, dass eine angegebene Eigenschaft nur für eindeutige Werte Sinn macht, daher ignoriert es bewusst alle potenziellen Duplikate während des Parsen (falls du sie trotzdem angegeben hast).

Beispiel für `ImmutableHashSet<uint>`: `"Blacklist": [267420, 303700, 335590]`

* * *

`ImmutableDictionary<keyType, valueType>` - Unveränderliches Wörterbuch (Map) das einen in seinem `keyType` angegebenen einzigartigen Schlüssel auf den in seinem `valueType` angegebenen Wert abbildet. In JSON wird es als Objekt mit Schlüssel-Wert-Paaren definiert. Bedenke, dass `keyType` in diesem Fall immer angegeben wird, auch wenn es sich um einen Wert-Typ wie `ulong` handelt. Es gibt auch eine strenge Anforderung, dass der Schlüssel auf der gesamten Karte eindeutig ist, diesmal ebenfalls von JSON durchgesetzt.

Beispiel für `ImmutableDictionary<ulong, byte>`: `"SteamUserPermissions": { "76561198174813138": 3, "76561198174813137": 1 }`

* * *

`flags` - Das Attribut Flags kombiniert mehrere verschiedene Eigenschaften zu einem Endwert, indem es bitweise Operationen anwendet. Auf diese Weise kannst du jede mögliche Kombination aus verschiedenen zulässigen Werten gleichzeitig wählen. Der Endwert wird als Summe aller Werte der aktivierten Optionen berechnet.

Zum Beispiel, wenn folgende Werte angegeben werden:

| Wert | Name |
| ---- | ---- |
| 0    | None |
| 1    | A    |
| 2    | B    |
| 4    | C    |

Die Verwendung von `B + C` würde zu einem Wert von `6` führen, die Verwendung von `A + C` würde zu einem Wert von `5` führen, die Verwendung von `C` würde zu einem Wert von `4`führen und so weiter. Dies ermöglicht es dir, jede mögliche Kombination von aktivierten Werten zu erstellen - wenn du dich dazu entschieden hast, alle zu aktivieren, indem du `None + A + B + C` wählst, bekommst du den Wert von `7`. Außerdem ist zu beachten, dass das Flag mit dem Wert `0` per Definition in allen anderen verfügbaren Kombinationen aktiviert ist, daher ist es sehr oft ein Flag, das nichts Bestimmtes aktiviert (wie `None`).

Wie du sehen kannst, haben wir im obigen Beispiel 3 verfügbare Flags zum Ein- und Ausschalten (`A`, `B`, `C`), und `8` mögliche Werte insgesamt:

- `None -> 0`
- `A -> 1`
- `B -> 2`
- `A+B -> 3`
- `C -> 4`
- `A+C -> 5`
- `B+C -> 6`
- `A+B+C -> 7`

Example: `"SteamProtocols": 7`

* * *

## Kompatibilitätsmapping

Aufgrund von JavaScript-Beschränkungen, da einfache `ulong` Felder in JSON bei Verwendung des webbasierten ConfigGenerators nicht ordnungsgemäß serialisiert werden können, werden `ulong` Felder als Zeichenketten mit `s_` Präfix in der resultierenden Konfiguration dargestellt. Dazu gehört z.B. `"SteamOwnerID": 76561198006963719`, der von unserem ConfigGenerator als `"s_SteamOwnerID": "76561198006963719"` geschrieben wird. ASF enthält die entsprechende Logik für die automatische Verarbeitung dieses Zeichenketten-Mappings, so dass `s_` Einträge in deinen Konfigurationen tatsächlich gültig sind und korrekt generiert wurden. Wenn du selbst Konfigurationen generierst, empfehlen wir, nach Möglichkeit an den ursprünglichen `ulong` Feldern festzuhalten, aber wenn du dazu nicht in der Lage bist kannst du auch diesem Schema folgen und sie als Zeichenketten mit dem `s_` Präfix an ihren Namen kodieren. Wir hoffen, dass wir diese JavaScript-Einschränkung irgendwann aufheben können.

* * *

## Konfigurations-Kompatibilität

Es ist für ASF oberste Priorität mit älteren Konfigurationen kompatibel zu bleiben. Wie du bereits weißt, werden fehlende Konfigurationseigenschaften genauso behandelt, wie wenn sie mit ihren **Standardwerten** angegeben würden. Wenn also eine neue Konfigurationseigenschaft in einer neuen Version von ASF eingeführt wird, bleiben all deine Konfigurationen **kompatibel** mit der neuen Version und ASF wird diese neue Konfigurationseigenschaft so behandeln, wie sie mit ihrem **Standardwert** definiert wäre. Du kannst jederzeit Konfigurationseigenschaften nach deinen Wünschen hinzufügen, entfernen oder bearbeiten. Wir empfehlen, definierte Konfigurationseigenschaften nur auf die zu ändernden Eigenschaften zu beschränken, da du auf diese Weise automatisch Standardwerte für alle anderen vererbst, nicht nur deine Konfiguration sauber hältst, sondern auch die Kompatibilität erhöhst, falls wir beschließen, einen Standardwert für Eigenschaften zu ändern, die du nicht explizit selbst festlegen möchtest (z.B. `WebLimiterDelay`).

* * *

## Automatisches Nachladen

Ab ASF V2.1.6.2+ ist es möglich, dass Konfigurationen "on-the-fly" geändert werden - dadurch wird ASF automatisch:

- Eine neue Bot-Instanz erstellen (und startet sie bei Bedarf), wenn du die Konfiguration erstellst
- Stoppt (falls erforderlich) und entfernt alte Bot-Instanz, wenn du ihre Konfiguration löschst
- Stoppt (und startet bei Bedarf) eine beliebige Bot-Instanz, wenn du ihre Konfiguration bearbeitest
- Startet den Bot (falls erforderlich) unter neuem Namen neu, wenn du seine Konfiguration umbenennst

All dies ist transparent und wird automatisch durchgeführt, ohne dass das Programm neu gestartet oder andere (nicht betroffene) Bot-Instanzen beendet werden müssen.

Darüber hinaus wird sich ASF auch selbst neu starten (wenn `AutoRestart` es erlaubt), wenn du die ASF-Kern-Konfiguration `ASF.json` änderst. Ebenso wird das Programm beendet, wenn du sie löschst oder umbenennst.