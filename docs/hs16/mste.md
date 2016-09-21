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
