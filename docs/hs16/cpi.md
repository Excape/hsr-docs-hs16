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
