# Mobile and GUI Engineering

## Prüfung
- Pro Teil 1h
- Pro Teil 10 A4-Seiten Zusammenfassung

## Vorlesung 1 - Einführung Android
- Miniprojekt Abgabe Woche 7 und Woche 14

- **Android Basics**
    - Java 7 auf Android
    - Activities sind ~"Screens"
    - Apps werden automatisch geschlossen
    - Allgemein wird der Lifecylce stark vom System gesteuert
    - Eine Activity sollte eine einzelne Aufgabe realisieren
    - Activity kann sich in verschiedenen Zuständen befinden: Wird gestartet, ist aktiv, wird in den Hintergrund gehen, etc.
    - Die einzelnen Methoden werden überschrieben (z.B. `onCreate()`)
    - Start einer Activity: `onCreate()`, `onStart()`, `onResume()`, erst dann ist sie interagierbar
    - Wird eine Activity überdeckt, wird sie pausiert `onPause()`. Kommt sie wieder in den Vordergrund, wird nur `onResume()` aufgerufen
    - `onDestroy()` könnte auch direkt anderen Zuständen aufgerufen werden!
    - Bei Konfigurationsänderungen wird die Activity neu gestartet (zerstört und neu aufgebaut). Also auch z.B. beim Drehen des Screens!
    - Daten in `onPause()` sichern
    - Activities werden in einem Stack verwaltet (muss nicht von gleicher App sein)
    - Der Back-Button poped normalerweise die oberste Activity auf dem Stack
    - Eine Gruppe von Activities (= Activity Stack) heissen "Task"
    - Eine geöffnete App ist ein Task, bzw. ist ein Eintrag im "Overview Screen" ein Task
- **Systemsicht**
    - Pro APK wird ein Prozess mit einem Thread gestartet
    - Jede APK wird unter eigenem Linux User installiert
    - APKs sind quasi JARs (= Zip-Files) 
- **Intents**
    - Alle Intents werden über das System verwaltet
    - Expliziter Intent: Eine bestimmte Klasse ansprechen
    - Impliziter Intent: z.B. "Absicht, Bild aufzunehmen"
    - Explizite Intents normalerweise für interne Activities, implizite für generische Aktionen
- **Views**
    - Alles, was der Benutzer sieht
    - Jede Activity hat eine View
    - GUI kann deklarativ mit XMl oder imperativ mit Java Code erstellt werden

## Vorlesung 2 - Grundlagen GUI
- Eine **View** ist immer eine Rechteckige Fläche, für die die View verantwortlich ist
- Widgets sind fertige Komponenten (buttons, images, checkboxes, ...)
- ViewGroup ist eine Unterklasse von View
- Layouts können ineinander verschachtelt werden (auch unterschiedliche)
- `match_parent`: Nimm den ganzen Platz ein
- `wrap_content`: Nur so viel Platz wie nötig
- Linear-Layout: Wenn kein Gewicht angegeben wird, wird möglichst wenig Platz verwendet. Mit Gewicht entsprechend dem Werten (mehr Gewicht -> mehr Platz)
- Neu gibt es `ConstraintLayout`, das auf den GUI-Builder optimiert wurde. Ist allerdings noch in Alpha
- Die `R` Klasse enthält Konstanten für alle XML-Files im res-Ordner (wird vom Compiler generiert) und bildet dessen Ordnerstruktur ab
- `@+` ist die Definition einer Ressource, `@` ein Verweis darauf
- `mipmap`: Launcher-Icon der App
- Strings mit `getString(R.string.string_name)` abrufen
- `dimens.xml` enthält Dimensionen für Layouts, z.B. `16dp` und werden über einen Namen aufgerufen
- `dp`: Density-independent-pixels: Unabhängig von Screen-dpi. Der Basis-Faktor wird von 160dpi berechnet ("mdpi")
- Für verschiedene Screen-Grössen, Sprachen, Versionen, etc. werden verschiedene XML-Files angelegt
- Die App hat nach den Lifecycle-Aufrufen keine Kontrolle mehr. Das System sendet Events (ausgelöst durch User oder z.B. Sensoren), die dann behandelt werden (Event-Listener)
- Auch Widgets können Events auslösen (-> `TextWatcher`)
- 
