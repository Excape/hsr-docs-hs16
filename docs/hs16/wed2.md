# Web Engineering + Design 2
## Prüfung
- 2h, closed book
- Zusammenfassung 1 A4 Seite (muss abgegeben werden)

---
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

---
## Vorlesung 4 - REST
- REST ist eine Guideline / Best Practices, wie Daten von Client zu Server gesendet werden
- Je mehr Richardson's Maturity Model Level umgesetzt werden, desto besser
- Vor REST gab es viele unterschiedliche Standards
- RESTful Services sollten *statuslos* sein. D.h. bei jeder Anfrage teilt der Client mit, wer er ist und was er will. Dadurch kann man die Server beliebig skalieren
- Eine REST-Schnittstelle sollte selbstbeschreibend sein. z.B. Ressource über ID abrufbar
- **ROA**: Ressource oriented Architecture:
    - z.B. GET auf /orders zeigt alle orders, /orders/1 zeigt order mit ID 1
    - Jede Ressource hat eine URL
    - Query-Parameter nur für Suchen oder Filter verwenden
    - Referenzen auf andere Ressourcen werden über hyperlinks (HTTP-Pfad) angegeben
    - *POST* erstellt eine neue Ressource (ID automatisch vergeben)
    - *PUT* geht direkt auf eine ID und aktualisiert eine Ressource, oder erstellt sie, falls noch nicht vorhanden
    - *HEAD* liefert nur der Header eines *GET*. Wird verwendet, um z.B. zu prüfen, ob es Änderungen gab (caching)
- Im Request Header wird mit `Accept` angegeben, in welchem Format (JSON, XML, ...) die Antwort sein sollte
- <https://www.getpostman.com/> zum Testen von APIs
- **HATEOAS**
    - "Oberste Stufe" einer REST API
    - Übergänge zu anderen States werden in Antwort mitgeschickt (in Form von URIs)
    - Aufgrund der Antwort kann der Client dann schliessen, wie er weiter machen muss (z.B. continue, cancel) und den entsprechenden Links folgen
    - URI kann serverseitig geändert werden

---
## Vorlesung 5 - Flexibles Layout
- Flexibles Layout: Seite passt sich Geräte-Grösse an, z.B. fliesst der Text automatisch
- Responsive Design: Auf unterschiedlichen Grössen werden verschiedene layouts eingesetzt (mit bestimmten Break-Points)
- `content-box`: Höhe und Breite bezieht sich nur auf den content
- `border-box`: Höhe und Breite bezieht sich auf content + padding + border
- `overflow`: Verhalten, wenn der Text zu viel Platz für die Box beansprucht ("overflowed")
    - Default `visible`: Text fliesst über Box hinaus
    - `hidden`: überfliessender Text wird abgeschnitten
    - `scroll`: Scroll-Balken anzeigen
- position `absolute`: Position ist relativ zum ersten Parent (das `relative` oder `absolute` ist, spätestens body)
- position `fixed`: Feste Position auf dem Screen
- Bei beiden `top`, `left`, `bottom`, `right` für die Abstände
- default ist `position: static`
- Kombination: Einige Elemente absolute positionieren, und dann z.B. Elemente im Flow lassen, aber die margin entsprechend vergrössern
- `inline-block`: Ein Block mit margin / padding, width / height, der Inline dargestellt wird
    - Anordnung zu anderen Blocks in gleicher Zeile mit `vertical-align`

### Flexbox
<https://css-tricks.com/snippets/css/a-guide-to-flexbox/>

- Definiert mit `display: flex` bzw. `inline-flex`
- Alle Blocks innerhalb einer Flexbox sind "inline blocks"
- `flex-direction`: `column` für Elemente untereinander, `row` für Elemente in einer Zeile
- `flex: <flex-grow> <flex-shrink> <flex-basis>`
    - grow, shrink: Faktor, wie die Box verkleinert / vergrössert werden kann
- `flex: 0 0 50px` heisst eine fixe Grösse von 50px
- `justify-content`
    - `flex-start`: An start positionieren (links bzw. oben)
    - `space-between`: Gleicher Abstand zwischen Elementen
    - `space-around`: Gleicher Abstand um Elemente herum
- 
