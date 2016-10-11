# Software Engineering 1
## Allgemein
- Prüfung closed book
- Abgabe von 3 von 4 Testat-Übungen
- Selbststudium: Jede Woche in Buch lesen, nach Lektionsplan sheet

## Vorlesung 1
- Ca. 70% aller IT-Projekte laufen schief
- Erwartungskonflikt Kunde - Programmierer, ...
- OOA nach Larman: Statisches Domain Model ohne Funktionen, Black-Box Sequenzdiagramm
- "Klassisch": Statisches Model mit Funktionen, White-Box Sequzendiagramms

---
## Vorlesung 2 - Domain-Modellierung
- Design-Modell zeigt eine Realisation, ein Domain-Modell auch Dinge, die nicht realisiert werden (Domain-Modell ist Inspiration für Design-Modell)
- Entitäten (Konzepte) finden: 
    - Keine Implementations-Konzepte verwenen (z.B. "Datenbank", "Webserver", etc.)
    - Keine abgeleiteten Konzepte (Dinge, die durch andere Dinge berechnet werden kann, z.B. ein Logfile)
- Semantic Gap: Unterschied zwischen realer Welt bzw. Domain Model und Design Model bzw. Code. Diese Gap ist bei OO-Design sehr klein, z.B. bei Assembler sehr gross
- n-m-Assoziationen sind im Domain Modell erlaubt, aber oft nich richtig, da die Assoziation eigene Attribute hat
- Sobald ein Attribut eine gewisse Komplexität hat, in eigenes Konzept auslagern (im Zweifelsfall Konzept!)
- Abstrakte Superclasses werden *kursiv* geschrieben (= Es gibt keine Instanzen der abstrakten Klasse)

---
## Vorlesung 4 - Requirements
### Einführung
- Requirements Modulanmeldung HSR
    - Funktional
        - Temporale und personelle Zugangsbeschränkungen
        - Alle Module müssen abgebildet sein
        - Benutzer muss sich für Module an- und abmelden können
        - Übersichtliche Strukturierung
        - Eigene Daten und Daten anderer Studenten können eingesehen werden
        - Voraussetzungen für Module prüfen
    - Nicht-Funktional (Qualität / Wie gut?)
        - System muss sicher sein
        - Antwortzeit 500ms
        - Umgebung / Programmiersprache
    - Prozess-Anforderungen
        - Kostenschätzung
- Probleme: Ablauf? Machbarkeit? Prioritäten?
- Anforderungen aus *Sicht der Benutzer* erstellen
- 30% aller Probleme in SE sind wegen Requirement Specification
- Umfassendes SRS am Anfang ist problematisch!
- Anforderungen ändern sich, daher iterativ vorgehen -> Requirements Management

### Use-Cases
- Use-Case ist *textuelle* Beschreibung "Actor tut das, dann macht das System ..."
- Geschichte aus Benutzersicht
- Aktor kann auch externes System sein (als Box dargestellt)
