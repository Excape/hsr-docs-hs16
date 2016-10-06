# C++
- Wiki: <https://wiki.ifs.hsr.ch/CPlusPlus/wiki.cgi?CPlusPlus>
- C++ Referenz: <http://en.cppreference.com/w/cpp>

## Vorlesung 1
- "Undefined Behaviour": Verhalten nicht definiert, wenn z.B. Divison durch 0 oder Segmentation fault ("Es kann alles passieren"). Schlimmster Fall: Es läuft!
- HelloWorld:
    - `#include`: Präprozessor Anweisung
    - `using namespace std;`: Kann alle Namen ohne `std::` benutzen. Man kann dann nicht sagen, aus welchem Namespace eine Anweisung kommt, oder ob es eine Variable oder Funktion etc. ist
    - `<<`: Links Shift, ist Teil der Bibliothek (Operator Overloading). Quasi Funktionsaufruf mit string-literal als Argument
    - `cout`: Ein Stream von `std`
    - `endl` ist eine Funktion, flusht den Stream
- Compilation
    - Im Header (z.B. `iostream`) stehen die Definitionen (z.B. dass es `cout` gibt)
    - Der Linker linkt die standard library (dort ist `cout` implementiert)
- Strings z.B. werden im Unterschied zu Java direkt auf dem Stack abgelegt
- Klassen: `struct` heisst public, `class` privat
    - Ein private bzw. public block mit `public:`, `private:` bezeichnen, alles ab dort hat dann den Scope
    - Nach Klassendefinition ein `;`: `sruct Hello {...};`

---
## Vorlesung 2
Repetition: 
- Wir trennen Header von Source-Files, um die Deklaration von der Implementierung zu trennen
- `iostream`, `iosfwd` und `ostream` sind Libraries mit unterschiedlichem Umfang. `iosfwd` ist die Kleinste, `iostream` die Grösste (mit u.a. `std::cout`)

### Variablen / Datentypen
- `<type> <name> {<value>}`
- Wenn eine Klasse mit `const` initialisiert wird, dürfen darauf nur Member-Funktionen aufgerufen werden, die `const` sind (`void helloWorld() const {...}`)
- `constexpr` berechnet den Wert zu Compilezeit: `double constexpr pi{3.14}`
- **Immer `const` verwenden wenn möglich!**
- Variablendefinitionen "close to use"
- `bool` und `char` sind im Hintergrund Integers
- `string` und `vector` sind in der Standardbibliothek und müssen importiert werden
- Länge von int, long, etc sind nicht fix definiert (nur short < int < long < long long)
- Gleitkommazahlen sind `double` wenn nicht explizit angegeben
- String-Literale sind char-Arrays (nicht `std::string`). Umwandlung mit `"string"s` mit `using namespace std::literals`
- `R"<string>"` sind Raw-Strings. Zeichen müssen nicht escaped werden (wie `@""` in C#)
- Alles != 0 -> true, 0 -> false
- Float nur verwenden, wenn Speicher knapp ist
- NaN ist ungleich jeder Zahl
- Strings (std::string) sind mutable, können also verändert werden
- Argumente werden normalerweise "by-value" übergeben. Für "by-reference" als Argument ein `&` vor den Namen setzen
- Folie 20: Reihenfolge der Aufrufe in den Funktionsargumenten ist nicht definiert (welches `inputName()` hier zuerst ausgeführt wird)
- Streams: Wenn ein stream erst mal "kaputt" geht (!stream.good()) muss er zuerst zurück gesetzt werden mit clear()
- `<iomanip>`: z.B. `std::oct`: Alle folgenden Zeichen, die in den Stream geschrieben werden, werden als oktal interpretiert

### Übung 2
#### Std-Library
- `std::cin`: iostream
- `std:endl`: endl
- `std::tolower`: cctype
- `std::string`: string
- `std::distance`: iterator
- `std::istream_iterator`: iterator
- `std::size_t`: cstddef
- `std::vector`: vector

#### Types
- 45: int
- 0XDULL: unsigned long long 0xd (=13)
- 5.75: double 5.57
- .2f: 0.2 float
- "string": char-Array
- '': char

#### streams
```c++
int i{};
std::cin >> i;
std::cout << "input: " << i << "\n";
std::cout << "fail: " << std::cin.fail() << "\n";
std::cout << "eof: " << std::cin.eof() << "\n";
std::cout << "bad: " << std::cin.bad() << "\n";
```
- Wenn man ein Zeichen (keine Zahl eingibt), wird `fail()` = 1
- Wenn man Enter drückt, wird es ignoriert und weiter auf einen input gewartet
- Whitespace wird ignoriert

## Vorlesung 3 - Simple Sequences
### Vectors
- Wie in Java generischer Datentyp: `std::vector<T>`
- Muss aber nicht "geboxed" werden
- Initialisierung in geschweiften Klammern: `std::vector<int> v{1, 2, 3}`
- Mit Runden Klammern Parameter an den Konstruktor geben: Anzahl Elemente: `std::vector<int> v(6)` für vector mit Grösse 6
- `front()`: Erstes Element
- `back()`: Letztes Element
- front und back können auch lvalues sein
- vector kann auch wachsen. Mit `pushback(x)` Element anfügen
- `begin()`, `end()` etc sind Iteratoren
- `at(i)` gibt das i-te Element zurück, ist aber bounds-checked
- index out of bounds ist undefined behaviour!
- `size_t` ist vorzeichenloser Datentyp, gross genug für indices aller möglichen vectors
- Iteration (einzige sinnvolle for-Schleife)
```c++
for (auto const i: v) {
    std::cout << "element " << i << '\n';
}
```
- Die Laufvariable erhält eine Kopie, sonst per Referenz übergeben: `for(auto &i: v)`
- So const wie möglich (**Prüfungsthema**)

### Iterators
- Man braucht immer ein Paar von Iteratoren
- Start Iterator `begin()` mit End-Iterator `end()` vergleichen. Wenn gleich, ist es *hinter* dem Ende
- Zugriff mit `*iterator`
- mit `cbegin()` und `cend()` ist der Wert const
```c++
for (auto it=cbegin(v); it != cend(v); ++it) {
    std::cout << *it << ", ";
}
```
- Mit `rend()` und `rbegin()` rückwärts iterieren (aber auch jeweils inkrementieren!)
- Für Schleifen grundsätzlich algorithms verwenden
- containers ausgeben mit `ostream_iterator`
- Iterator um einem Array zu erweitern: `back_inserter(v)`

### Algorithms
- Bsp Leerzeichen zählen: `count(begin(s), end(s), ' ')`
- `distance(b, e)` gibt Distanz zwischen zwei iteratoren

### Lambdas
- `for_each(b, e, fn)` mit fn als Lambda oder funktionsname
- `[] (parameters)->return_type{statements}`
- return type und runde Klammer sind optional. `[]{}` ist eine gültige Lambda Expression
- Aufruf mit runden Klammern: bsp. `[]{}()`
- `auto` als Parameter-Typ erlaubt (im Gegensatz zu normalen Funktionen)
- Lambda-Funktion kann auch in eine Variable geschrieben werden: `auto l = []{}; l();`
- `[]` wird für Capture verwendet. Was in den Klammern definiert ist, ist auch ausserhalb des Lambda-Scopes noch gültig. Referenz mit `&` angeben

### Iterators for IO
- `ostream_iterator<T>{std::out, ", "}` gibt Werte auf `std::out` mit Trennzeichen `, ` aus
- `istream_iterator<T>{}` wird als Enditerator beim Input verwendet
- type alias: `using name=type;`, dann kann ein Alias für den Type verwendet werden
- `ostream_iterator` und `istream_iterator` benutzt `<<` und `>>` für Ein- und Ausgabe
- Normalerweise werden Leerzeichen ignoriert
- für char-Types gibt es dafür `istreambuf_iterator`. Einlesen mit `istream::get()`
