# Web Engineering + Design 2
## Prüfung
- 2h, closed book
- Zusammenfassung 1 A4 Seite (muss abgegeben werden)

## Vorlesung 2 - NodeJS
- "Klassischer" Webserver
    - Bei jeder Anfrage wird die Seite auf dem Server neu aufgebaut
    - z.B. PHP, ASP...
- Event-driven, non-blocking
    - Bei einem Prozess (Thread) kann nur eine Anfrage gleichzeitig bearbeitet werden (blocking)
    - Mehrere Threads (ASP, ...): Mehrere Anfragen gleichzeitig bearbeiten (feste Anzahl mögliche Threads)
    - Event-driven (node): Es gibt einen Thread, der den Task "abliefert" und benachrichtigt wird, sobald er bearbeitet wurde (callback, asynchron). Die Antwort wird dann wieder über den einen Thread ausgeliefert
- Folie 17: Die Funktion wird 1 Sec verzögert aufgerufen (asynchron), d.h. sie blockiert den Ablauf nicht
- Callback: 1:1-Verbindung
- Event: 1 : * Verbindungen (Es können z.B. mehrere sich auf einem Event "anmelden", -> "Newsfeed")
- *Tipp*: Bei `server.listen(port, hostname, fn)` den Hostnamen weglassen, dann wird auf jede NIC gehört
- Query parsen mit `url.parse(request.url, true).query` (`true` gibt gerade JSON-object zurück)
- Die `handler` Funktion von Node ist blockierend, sollte also möglichst schnell sein
- Ein Modul wird nur einmal durchlaufen. D.h. wenn man zwei mal `require()` auf das selbe Modul aufruft, ist das Objekt dasselbe
- Modul als requirement hinzufügen: `npm install <name> --save`
- Folie 28: Bei Zyklen macht Node eine Kopie eines Moduls, sollte aber vermieden werden
- Folie 33: Variante mit Streams zu bevorzugen, weil sonst das ganze File ins Memory geladen werden muss. Beim Stream werden vorzu die Daten zum Client geschickt
