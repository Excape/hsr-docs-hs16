# Web Engineering + Design 2
## Links
- CSS-Elemente zentrieren: <https://wiki.selfhtml.org/wiki/CSS/Anwendung_und_Praxis/Inhalte_zentrieren>
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

### Analyse realistischer layouts
- Mobile Layout mit `display:block`
    - Elemente werden in Reihenfolge des HTML angezeigt
- Mobile layout mit `flex`
    - Reihenfolge kann verändert werden
    - Braucht media queries für flexible layouts

### Browser support
- Graceful Degredation: Alle modernen Features nutzen, wenn nicht vorhanden, alternativen anbieten oder Hinweise einblenden
- Progressive Enhancement: Starten mit Grundfunktionalität für alle Browser, dann darauf aufbauen mit CSS und JS
- "Graceful Degradation" ist günstiger für die Entwicklung aber
normalerweise teurer als "Progressive Enhancement" in der Wartung.

---
## Vorlesung 6 - Responsive Layout
### Media Queries
- Für unterschiedliche Medien
    - `@media screen {}`
    - `@media print {}`
- Für Dimensionen
    - `@media ([width|min-width|max-width]: 375px) {}`
    - `@media ([height|min-height|max-height]: 667px) {}`
    - `@media ([device-width|min-device-width|max-device-width]: 375px) {}`
    - `@media ([device-height|min-device-height|max-device-height]: 667px) {}`
- Spec: <https://www.w3.org/TR/css3-mediaqueries/>
- Kann auch direkt als sepparates Stylesheet geladen werden (HTML5): `<link rel="stylesheet“ href=“LargeScreenLayout.css" media="(min-width: 30em)">`
- Mobile First Approach: HTML für Mobile Layout schreiben, dann für grössere Displays mit Media Queries anpassen

### Viewport
```html
<meta name="viewport" content="width=device-width,initial-scale=1">
```
- Deaktiviert die "Intelligenz" von mobilen Browsern
- Sonst funktionieren Media Queries nicht richtig, da der Viewport skaliert wird vom Browser
- Details: <https://vimeo.com/100523275>

---
## Vorlesung 7 - Responsive Webdesign
- Resizer zum Testen: http://material.io/resizer/
- Rechte Spalte wird oft ignoriert, da dort oft Werbung ist
- HSR-Site: 
    - In mobiler Version sind viel weniger Informationen vorhanden
    - Kleine "Touch-Targets" wenn nicht Mobil-Seite verwendet
- WWF-Seite: 
    - Hamburger Menü ist schlechter als "Menü"-Bezeichnung
    - Nur noch wichtigste Elemente werden auf mobiler Sicht direkt angezeigt, Rest kommt in ein Drop-Down-Menü
### Patterns
- Übersicht: <https://bradfrost.github.io/this-is-responsive/patterns.html>
- Mostly Fluid
    - Elemente untereinander platzieren, wenn sie kein Platz mehr haben
    - "Reflow"
- Column Drop
    - Spalten, die in der Desktop-Version links und rechts sind, werden nacheinander "gedroppt" und untereinander angeordnet
- Layout Shifter
    - Navigation auf Desktop ist links, INhalte untereinander
    - Navigation wird auf Mobile nach oben geschoben
    - Siehe auch "Smashing Magazine"
- Reflow
    - Viele Spalten auf Desktop
    - Eine Spalte auf Mobile
    - "Cards", siehe "Material Design"
- Expand
    - Maximale Breite der Seite auf Desktop, links und rechts einen Rand
- Off Canvas
    - Menü in Mobile nicht sichtbar, kann "reingeschoben" werden
- Folie 33
    - Wenn für einen "Review"-Mode keine Inputfelder gerendet werden müssen, spart dies Platz
- "Repeat E-Mail"-Felder sind unnötig. Besser nach dem Submit einen Review-Schritt einbauen, damit der User seine Eingaben überprüfen kann

---
## Vorlesung 8 - UCD Vertiefung
- <http://www.vischeck.com/> zum Überprüfen, wie die Seite für Farbenblinde aussieht