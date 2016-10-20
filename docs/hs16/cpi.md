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

---
## Vorlesung 4
- Gute Funktion:
    - Macht nur etwas
    - Möglichst wenig Parameter
    - Garantiertes Resultat
- Lambdas
    - Funktion (oder Lambda) als Parameter übergeben
```c++
void foo(std::function<char(char)> function) {
    std:cout << function('a');
}
// main
auto const g = [](char c) { return c + 1; }
foo(g);
```
    - Scopes werden mit `{}` definiert
    - Nach dem `}` wird aufgeräumt, auch Objekte!
- Namespaces
    - namespace grenzt ein Scope ein
    - `std::` ist Namespace-Prefix
    - Globaler Namespace ist unter `::`
    - Definition mit `namespace myNamespace{..}`
    - Namespace mit gleichem Name kann mehrmals "geöffnet" werden. Er wird dann erweitert;
    - "Importieren" (ohne Prefix benutzen) mit `using myNamespace;`
    - Alias mit `using str=std::string; str t{"myString"};`
    - Namespace ohne Namensangabe ist anonym. Braucht man, um eigene Funktionen zu "verstecken" und ist nicht von anderen Files aufrufbar
- Referenzen
    - Achtung, wenn Referenz noch benutzt wird, aber die referenzierte Variable nicht mehr existiert! -> undefined behaviour
    - Wenn eine referenz `const` wird, darf die variable darunter auch nicht mehr verändert werden
    - `&var` ist *lvalue*-Referenz
    - Referenzen zurück geben
        - Für chaining nützlich, wenn Parameter schon ref ist
        - Niemals referenz auf lokale variable zurück geben, nicht mehr gültig!
    - Streams by value kopieren mit rvalue-Referenzen (`&&`)
    - `&&` "saugt" den Wert heraus, ist im Original (caller) dann nicht mehr verwendbar
- Overloading wie in Java, return-type wird nicht beachtet
- Default Arguments wie in C# (ist ein impliziter overload) und muss im Header-File definiert sein
- Funktionen als Parameter: `void func(double x, double f(double)) {...}`
- Error-Handling
    - z.B. zusätzliches funktionsargument per referenz mitgeben (z.B. boolean)
    - oder komplizierterer Rückgabewert
    - Am besten: Exception
    - Ignorieren des Fehlers (-> undefined behaviour) kann Sinn machen, wenn es schnell sein muss (Aufrufer muss dann Preconditions erfüllen)
    - exceptions
        - Mit `throw` kann jeder (kopierbare) typ geworfen werden
        - Per default keine meta-infos wie stacktrace
        - `try {...} catch (type const &e) {...}`
        - Catchen immer mit Referenz auf exception ("Throw by value, catch by const ref")
        - Catch all: `catch(...)` <- Mit 3 Punkten!
        - In `stdexcept` sind exception types, z.B. `std::logic_error` und `std::runtime_error`
        - Message in Konstruktor mitgeben, im catch mit `e.what()` aufrufen
        - Testen in CUTE mit `ASSERT_THROWS(<code>, <expected exception type>)`
```c++
try {
    // throwing code
    throw <value>;
} catch (<type> const & e) {
    // Exception handling
}
```

---
## Vorlesung 5 - Classes and Enums
- Eine gute Klasse hat eine Invariant, eine Garantie über den Status der Klasse
- Klasse wird normalerweise im Header definiert mit `Class <name> {...};`
    - Ein Verwender der Klasse muss wissen, wie der Speicher der Klasse aufgebaut ist, darum ist die Klasse mit Members im Header definiert
    - Einzelne Sektionen `public:`, `private:`, `protected:`
    - Bei Klassen sind funktionen implizit private, bei structs public
- Konstruktor: `Class(value1, value2) : member{value1}, member2{value2}`
    - In Reihenfolge der Member-Deklaration
- Kopier-Konstruktor: Wird dann ausgeführt, wenn einer instanzierung der eigene Typ als argument mitgegeben wird: `Date d2{d1}`
    - Ist implizit verfügbar: `Date(Date const &)`
- Move-Konstruktor ebenfalls implizit vorhanden
- `explicit Date(std::string const &)`, um dem Konstruktor einen anderen Datentyp mitzugeben, um sie in den Typ zu verwandeln
- Destruktor: `~Date();` ist implizit vorhanden, sollte selten explizit implementiert werden. Wird beim Aufräumen aufgerufen
- Vererbung mit `class Sub : public Base {...};` angeben
    - Folie 19: onyInBase ist private
    - Wenn `private` geerbt wird, kann man in Sub auf die private Member von Base zugreifen, aber nicht ausserhalb von Sub
    - Mehrfachvererbung möglich
- Implementierung: Jeweils `<type>::<function> {...}`
- Statische Funktionen aufrufen mit `type::function`
- `const` Funktionen können keine Klassen-Felder verändern
- Im Konstruktor prüfen, ob die Daten (z.B. das Datum) gültig ist
    - Establishes invariant
- Konstruktoren delegieren: `Date::Date() : Date{1980, 1, 1}` wie `this()` in Java
- member-Funktionen darf invarianz nicht kaputt machen
- Expliziter Zugriff auf Felder mit `this-><member>`
- Static Functions brauchen keine static-Deklartion in der Implementierung
    - Können nicht `const` sein
- `static const`-Felder können direkt im Header initialisiert werden
- Operator-Overloading
    - `<returntype> operator<op> (<parameters>)`
    - Ein Parameter für unäre Operatoren, zwei für binäre
    - Ausnahme: inkrement, dekrement prefix nimmt zwei parameter, um prefix und suffix zu unterscheiden
    - Wenn overloading im header-File implementiert wird, mit `inline` deklarieren
    - Problem mit "free" Operator (ausserhalb der Klasse): Kein Zugriff auf private Members
    - Alternative: Operator Overloading als Member in die Klasse hinein nehmen. Nur noch 1 Parameter für right-hand-side (linke Seite ist this-Objekt). Ist implizit `inline`
    - `std::tie()` vergleicht komponentenweise zwei Elemente
```c++
bool Date::operator<(Date const & rhs) const {
    return std::tie(year, month, day) <
    std::tie(rhs.year, rhs.month, rhs.day);
}
```
    - Mit boost kann man z.B. `<` implementieren, und bekommt dann `>, <= und >=` (Erben von `boost::less_than_comparable<myType>`)
    - `<<` überschreiben für Ausgabe
        - Problem: in Member-Funktion ist das linke Argument vorgegeben mit `this`, dann müsste der Stream rechts von `<<` sein
        - Lösung: Hilfsfunktion `print()` in der Klasse, die von freier Funktion aufgerufen wird
```c++
class Date {
    int year, month, day;
public:
    std::ostream & print(std::ostream & os) const {
        os << year << "/" << month << "/" << day;
        return os;
    }
};
inline std::ostream & operator<<(std::ostream & os, Date const & date){
    return date.print(os);
}
```
