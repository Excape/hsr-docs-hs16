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
    - 
