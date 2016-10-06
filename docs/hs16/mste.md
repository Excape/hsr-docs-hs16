# .NET Technologien
## Prüfung
- Voraussetzung: Miniprojekt abschliessen
- 120 Min, alle Unterlagen (ausser Musterprüfungen)

## Vorlesung 1 - .NET Grundlagen
- .NET-Framework
  - Typen sind im Laufzeitsystem (CLR) nicht wie im Java
  - Eine Class Library für alle .NET-Sprachen
  - Compiler erstellen Assemly IL (Intermediate Language) Code, auf der CLR wird er mit einem JIT-Compiler in nativen Code generiert
  - IL läuft auf virtueller Stack-Maschine (ohne Register)
  - JIT-Compiler kompiliert erst auf Zielsystem (Platform-Abhängig)
- JIT
  - Es wird nur der Code übersetzt, der benötigt wird, also z.B. beim erstem Methodenaufruf
- Assembly
  - Black Box mit definierten Schnittstellen
  - Im Manifest ist metadata
  - Es gibt darin Resourcen und Module
  - Private Assembly: Wird direkt über Pfad referenziert
  - Shared Assembly: Sind im Global Assembly Cache, stehen allen Applikationen zur Verfügung

## Vorlesung 2 - C# Grundlagen (Selbststudium)
- Methoden, Klassen, Interfaces, Properties, etc: PascalCase
- Felder: camelCase
- unsigned Datentypen: `uint`, `ulong`, `ushort`
- `///` für Dokumentation
- "Raw-Strings": `@"string"`
- `decimal` Typ für präzise Gleitkommazahlen
- `internal`: In diesem Assembly sichtbar. Standard für alle Klassen, interfaces, enums etc.
- `protected`: Wie `private`, aber auch in abgeleiteter Klasse sichtbar
- Abgeleitete Klassen dürfen nicht "sichtbarer" sein als der Basistyp
- Namespaces
    - Wie Packages in java
    - Ein Namespace ist ein Code block mit `namespace {}` eingeschränkt
    - Unabhängig von Files und Assemblies
    - Alias: `using <alias> = System.<...>`
    - Ein File kann mehrere Namespaces beinhalten, ein Namespace kann über mehrere Files definiert sein
- Enums
    - `enum Days {Monday, Tuesday, ...}`
    - Implizit erhält jeder Wert einen Int (von 0 beginnend)
    - Explizit: `enum Days {Sunday = 10, Monday = 11}`
    - Wenn bei einem Element Wert nicht explizit angegeben, ist es `letztes Element + 1`
    - Muss nicht zwingend int sein: `enum Days : byte {Sunday, ...}`
- Arrays
    - Rechteckig Multidimensional: `new int[3, 2]`
    - Jagged (Array in Array): `new int[2][]`
- Strings
    - ist ein Referenztyp `System.String`
    - immutable
    - Verketten mit `+`
    - Vergleiche mit `==` möglich
    - Formatierung: `string.format("{0}, {1}", var1, var2)`
    - Interpolation: string s = `$"var1: {var1}, var2: {var2}"`
    - Intern gibt es String "Interning" (=String Pooling in Java)

## Vorlesung 3 - Klassen und Structs
### Structs
- Ein Value Type auf dem Stack abgelegt
- Structs können nicht voneinander abgeleitet werden
- Structs können aber Interfaces implementieren
- Initialisierung von Feldern nicht erlaubt (Bei Klassen schon)
- Konstruktur *muss* Parameter haben
- Struct benutzen für einzelnen Wert (z.B. Point), max. 16B, immutable types, sollte nicht häufig geboxed werden
- Struct sollte entweder kurzlebig sein oder in andere Objekte eingebettet (damit Stack nicht zu gross wird)

### Klassen
- Const-Wert muss initialisiert werden (und zur Compilezeit berechenbar)
- Read-only Feld `readonly` muss nicht zur Compilezeit berechenbar sein aber auch nicht initialisiert. Darf später nicht mehr verändert werden
- Nested Types als innere Klassen
    - Äussere Klasse hat nur Zugriff auf public Member der inneren Klasse
    - innere Klasse kann auch auf private Members der äusseren Klasse zugreifen
    - Fremde Klassen haben auch Zugriff auf public Member der inneren Klasse
    - Mit `static using <static Class>` können Funktionen von static Classes direkt verwendet werden
- Methoden-Parameter
    - Value-Types werden normalerweise by-value übergeben
    - Mit `ref` keyword den parameter by-reference übergeben. Beim Aufruf muss auch `ref` angegeben werden, sonst wird eine Referenz auf eine Kopie übergeben
    - `out`: Wie `ref`, aber für Rückgabe von Werten. Das Argument darf beim Aufruf aber noch nicht initialisiert sein.
    - mit `params` in der Funktionsdefinition können n weitere Argumente verwendet werden beim Aufruf, die dann in einem Array landen, das mit `params` gekennzeichnet wurde
    - optionale Parameter: `void fn(int arg1 int arg2 = 0)`. Optionale Parameter müssen am Ende der Liste stehen!
    - Named Parameter bei Übergabe von optionalen Parameter: `fn(2, arg2: 3)`
- Properties
    - Wrapper für Getter / Setter
    - Sieht aus wie public field auf Klasse
    - Im Setter enthält `value` den zuzuweisenden Wert
    - Property-namen in PascalCase
    - Der Compiler wandelt Getter und Setter in normale Methoden um, sie werden als Property markiert
    - Getter oder Setter können weggelassen werden, um read-only bzw. write-only Properties zu erstellen
    - Sichtbarkeiten von Getter und Setter können verändert werden
    - Auto-Properties
        - Properties mit einfachem getter und Setter, das Backing field wird auch automatisch generiert
        - `public int Length {get; set;}`
        - Seit C# 6.0 kann der Setter weggelassen werden, der Wert muss aber danach mit `=` zugewiesen werden (oder im Konstruktor)
    - Objekt Instanzierung
        - Properties können direkt initialisiert werden: `MyClass mc = new MyClass() { Length = 1, Width: 2 };`
- Indexer
    - Ähnlich wie Properties kann eine Klassen-Instanz indexiert werden: `MyClass mc = new MyClass(); mc[0]; mc[1]...`
    - Definition in Klasse: `public string this[int index] { get {...} set {...}};`
    - Auch mehrdimensionale Indices möglich
- Konstruktoren
    - Leerer Default-Konstruktor wird automatisch generiert (aber nur, falls keiner definiert wurde!)
    - Statische Konstruktoren werden nur 1x aufgerufen
        - Zwingend ohne Parameter
        - Wird bei erster Instanzierung des Typs aufgerufen, danach nicht mehr
    - Aufruf anderer Konstruktoren mit `this`, von Basis-Klassen mit `base`: `public myClass() : this(0, 0) {...}`
- Destruktoren
    - `~MyClass() {...}`
    - Wird vom GC aufgerufen
- *Evtl. Prüfungsaufgabe zu Initialisierungs-Reihenfolge*
- 
