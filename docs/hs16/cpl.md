# C++
- Wiki: <https://wiki.ifs.hsr.ch/CPlusPlus/wiki.cgi?CPlusPlus>
- C++ Referenz: <http://en.cppreference.com/w/cpp>

## Notes Beratungssession
- An Prüfung kommt insbesondere: Vererbung, Const, alles aus den Testataufgaben
- Wichtigste Algorithmen auswendig kennen!
    - `lexicographical_compare`, `transform`, `find`, etc.
- r-value und l-value nicht an Prüfung gefragt
    - l-value ist ein Platz, wo man etwas hinein tun kann (z.B variable)
    - r-value ist etwas, woraus man etwas heraus holen kann
- Wenn etwas "moved" wird, darf man den alten Wert nicht mehr benutzen. Das heisst nicht, dass es nicht möglich ist, aber nicht garantiert!


## Vorlesung 1 - Intro
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
## Vorlesung 2 - Values
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
## Vorlesung 4 - Functions
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
- `inline`, weil Funtkion im Header implementiert ist. Ohne inline würde es einen Konflikt beim linken geben, wenn mehrere Files dieses Header-File verwenden (one-definition-rule)

---
## Vorlesung 6 - Overloading / Enums
- `read()`: Wenn Parsen fehlschlägt, wird das Fail-bit gesetzt
- Alle objekte mit `{}` initialisieren (auch mit leeren Konstruktor)
- Default-Initialisierungen von Member Variablen können direkt im Header angegeben werden, der default-Konstruktor kann leergelassen werden
```c++
class Date {
int year{9999}, month{12}, day{31};
... }
```
- Im Header `Date() = default;` schreiben, um einen Default-Konstruktor zu erstellen, auch wenn es explizite Konstruktoren gibt
- Implizite Konstruktoren (z.B. copy / move) können gelöscht werden: `Date() = delete;`
- Argument Dependent Lookup: Wenn ein Member in einem Namespace mit einer Klasse ist und dessen Argumente vom Typ dieser Klasse sind, kann der Compiler beim Aufruf auch ohne namespace-prefix entscheiden, welche Funktion gemeint ist.
    - z.B. `copy(begin(v)), end(v), ..)`. v ist ein Vektor aus std, deshalb wird begin() auch von dort aufgerufen, und wiederum `copy()` genauso

### Enums
- unscoped enum
```c++
enum day_of_week {
Mon, Tue, Wed, Thu, Fri, Sat, Sun
// 0, 1, 2, 3, ...
};
```
- Sind im umliegenden Scope direkt verfügbar ("leaken nach aussen")
- scoped enum: Mit `enum class day_of_week`
    - Leaken nicht ausserhalb des Scopes
- Enum-Werte können auch explizit deklariert werden (oder doppelt)
```c++
enum month {
jan = 1, feb, mar, apr, may,
jun, jul, aug, sep, oct, nov, dec,
january = jan, february, march,
april, june = jun, july, august,
september, october, november,
december
};
```
- Um einen anderen unterliegenden Typ zu verwenden, von diesem erben: `enum class my_enum : unsigned char`

---
## Vorlesung 7 - Standard containers
- Jeder Container hat Iteratoren für begin() und end(), insert, erase
    - begin zeigt auf das *erste*, end *hinter das letzte* Element, d.h bei leerem container ist begin == end
- Container können mit einem anderen container constructed werden, dann wird eine *Kopie* erstellt
- Inhalt kann mit == verglichen werden
- `std::list<int> l(5,42)` erstellt 5 Elemente mit dem Wert 42
    - Runde Klammern, weil sonst Mehrdeutigkeit mit direkter initialisierung `{5, 42}` (-> zwei Elemente)
- container mit Range: `std::deque<int> q{begin(v),end(v)}`
- Iteratoren-Kategorien sind Tags, wie "Marker-Interfaces" in Java (z.B. "cloneable", "serializable")
- Input Iterators können nur einmal auf das aktuelle Element zugreifen (nur lesend)
- Forward Iterator ist ein Input-Iterator, der auch schreibend auf das aktuelle Elemente zugreifen kann
- Bidirektionaler Iterator kann vortwärts und rückwärts iterieren, mehr als einmal pro Element
- Random-Access-Iteratoren sind bidirektional, aber können auch mit `[]` direkt auf den Index zugreifen
- Output-Iterator ann etwas auf das aktuelle Element *schreiben*, aber nur einmal
- `std::advance(iter, n)` springt um n Positionen weiter
    - `next()` funktioniert gleich, gibt aber den Iterator zurück
- `auto` für iteratoren-typen verwenden, da return-types oft kompliziert sind
- `std::list<>` ist double-linked-list, `std::forward_list` single-linked-list
- `std::array<int,6>` für fixe Arrays
- `std::stack` braucht vektor mit beschränktem Interface
    - `pop()` gibt void zurück! (mit `top()` abfragen)
- `std::queue` nutzt dequeue mit limitierter Funktionalität
    - Iteration nicht möglich

### Associative Containers
- Sind sortiert
- `std::set` speichert Elemente sortiert
    - mit `count(el)` prüfen, ob Element in Set ist (gibt nur 0 oder 1 zurück)
- `std::map<>` speichert key-value-pairs sortiert
    - Zugriff mit Typ `std::pair<key,value>`, mit `first()` und `second()` (`auto` verwenden)
    - Bei Zugriff auf Key, der nicht in der Map existiert, wird ein neues Element eingefügt mit Default-Value (bei vector wäre das undef. beh.!!)
- `std::multiset` erlaubt mehrere gleiche Keys
    - `equal_range` gibt paar von upper und lower bound zurück
- `unordered_set` (und "_map") ist Hash-Set, aber hashcode ist nur für default types (z.B. strings) definiert
    - Nicht sortiert
 - Bsp Folie 33:
    - `istreambuf_iterator` überspring whitespace nicht!
    - `remove_copy_if` kopiert streams, aber ohne Elemente, die Bedingung erfüllen

---
## Vorlesung 8 - Algorithms
- `<algorithm>` für die meisten Algorithmen, `<numeric>` für ein paar numerische
- Streams müssen mit einem `istream_iterator` bzw. `ostream_iterator` gewrappt werden, um sie mit Algorithmen zu verwenden
    - Default-konstruierter `_iterator` ist immer EOF
- Functor: Klasse, die Call-Operator `()` anbietet
    - Instanz davon kann z.B. einem `for_each` mitgegeben werden, oder aufgerufen mit `<instance>()`
    - Lambdas sind intern mit Functors gelöst
- Transform mappt Elemente von ein oder zwei ranges (gleiche Grösse)
- `remove` entfernt nicht Elemente, sondern schiebt nur die Werte nach vorne und gibt einen Iterator zurück, der auf das Ende der neuen Range zeigt
    - Der hintere Teil darf dann nicht mehr verwendet werden. Dieser kann entfernt werden mit `vec.erase(removed, vec.end())`, wobei `removed` der returnierte Iterator ist
- Viele Algorithmen gibt es auch mit `_if`, dem ein Predicate mitgegeben werden kann. Z.B. `count_if`, `replace_if`, `remove_if` etc.
- Wenn man keinen End-Iterator hat, kann man mit `_n` algorithmen eine konkrete Zahl angeben
- Pitfalls
    - Beim Kopieren in eine neue Sequenz muss dort genügend Platz frei sein, ein vector wird nicht selbst vergrössert!
        - Besser `back_inserter` oder `front_inserter` verwenden statt v.begin() für den neuen vector, dann werden die einzelnen Elemente gepusht
    - Wird während der Iteration z.B. push_back aufgerufen, wird der Iterator invalidiert (wie "concurrent modification exception" in java)

---
## Vorlesung 9 - Functors / Lambdas
- Functor kann z.B. für for_each verwendet werden (wird dann für jedes Element aufgerufen)
- Vorteil: Überladener `()` Operator kann auf Member-Variablen der Instanz zugreifen
```c++
std::vector<double> v{7,4,1,3,5,3.3,4.7};
auto const res=for_each(v.begin(),v.end(),averager{});
```
- `generate(firstIt, lastIt, Generator)`:
    - Generator ist ein objekt (oder lambda), das ein Wert zurückgeben muss, der dem iterator zugewiesen werden kann
    - Generator kann auch ein functor sein (d.h darauf der call-Parameter überschreiben)
- Lambda kann einer Variable zugewiesen werden mit `auto`
    - Intern wird der Typ vom Compiler definiert (nicht vom Standard vorgeschrieben)
    - Return-Type kann mit `[]()->char{}` manuell angegeben werden, ist aber nicht nötig
- capture mit `[=]`: Kopie in Lamba
- Capture mit `[&]`: Referenz auf variable
- Mit `[x=value]` ist im Lambda-Body die Variable `x` verfügbar (gleich wie man die variable im Body deklarieren würde)
- `[this]` capture in Member-Funktion: `this` ist ein Pointer!
    - `this->var`
    - `(*this).var`
    - Wenn man `this` per copy capturen, bekommt man nur den Pointer, der ist const (siehe unten), aber man kann weiterhin member-Variablen verändern!
- Lambda mit `[]() mutable {}`
    - Per default sind variablen mit `[=]` gecaptured nicht veränderbar
    - Mit `mutable` sind sie veränderbar
    - Gilt nicht bei variablen, die per Referenz `[&]` gecaptured werden
- Lambdas sind intern meist functors
- Für einfache Funktionen gibt es Standard-Funktoren wie `std::negate<T>` oder `std::greater<T>`
- Funktionen können andere Funktionen als Parameter entgegen nehmen `foo(double bar(int))`
    - Beim Aufruf kann ein Funktionsname mitgegeben werden, oder ein Lambda
    - Geht aber nicht mit Funktoren
    - Kann mit templates gemacht werden (nächste Vorlesung)
    - Oder als parameter in der Funktion den typ `std::function<double(int)>` verwenden
- Eine `std::function<>` kann in ein bool konvertiert werden und gibt zurück, ob eine gültige Funktion darin ist

---
## Vorlesung 10 - Function Templates
- Generische Funktionen
- templates sind zu compile-time polymorph
```c++
namespace MyMin{
template <typename T>
T const& min(T const& a, T const& b){
    return (a < b)? a : b ;
}
}
```
- Parameter können Typen sein oder templates (nächste Woche) - oder ganzzahlige Werte (cpp advanced)
- `typename` legt Parameter als Type-Parameter fest (synonym `class` - deprecated)
- Parameter werden beim Aufruf vom Compiler automatisch ersetzt (vgl. `auto` keyword) - "template argument deduction"
- Body der Template-Funktion immer im header definieren
- Referenz als Rückgabetyp, weil vom Aufruf die Referenz garantiert nicht "dangling" ist
- Folie 8
    - Namespace explizit angeben, weil Argument `std::string` -> Compiler sucht zuerst in `std` nach `min()`
    - `min("Pete", "Toni")`: Literals sind vom Typ `char[5]`, d.h. die beiden müssen gleich lang sein, denn `char[6]` ist ein anderer "Typ"!
- Argumente können auch explizit angegeben werden mit `min<type>()`. `min<std::string>("Peter", "Toni")` funktioniert.
    - Es müssen nicht alle Parameter explizit angegeben werden, die restlichen werden dann impliziert
- Implizite Anforderungen an Template-Parameter sind *Concepts*
- Bei `min()` 
    - darf T nicht `void` sein (wegen Rückgabewert Referenz ist)
    - Operator < muss definiert sein (und bool zurückgeben)
- Concepts sind immer implizit (bis übernächsten C++ Standard)
- Bei `max()` (Folie 11) muss T kopierbar / move-bar sein, da Rückgabewert keine Referenz ist
- Folie 14: Dies ist problematisch, wenn man null-pointer übergibt -> Undef. Beh.
- Templates kann man auch overloaden - Der spezialisierteste overload wird ausgeführt
    - Gefährlich, weil schnell ambiguities entstehen
- Variadic Templates: Type-safe varying number of arguments

```c++
template <typename...ARGS>
void variadic(ARGS...args){
    println(std::cout,args...);
}
```

- Beispiel `println()`
    - Wenn `tail...` leer ist, wird per overloading resolution die `println()` funktion mit einem Argument gewählt -> Rekursions-Basecase
- Template-Parameter können default-Werte annehmen: `template <typename T, typename U=T>`

---
## Vorlesung 11 - Testat 2 Review
### Headerfile
```c++
class Word {
    std::string value;
    static bool isValidWord(std::string const & value);
public:
    Word() = default;
    explicit Word(std::string const & value);
    void print(std::ostream & os) const;
    void read(std::istream & is);
    bool operator <(Word const & rhs) const;
    
inline std::ostream & operator <<(std::ostream & os, Word const & w) {
    w.print(os);
    return os;
}
inline std::istream & operator >>(std::istream & is, Word & w) {
    w.read(is);
    return is;
}
};
```
- Im Headerfile grundsätzlich immer ein Namespace definieren
- Default-Konstruktor mit `Word() = default`, nicht explizit implementieren
- Konstruktor mit einem Parameter `explicit` deklarieren, damit der Compiler keine (impliziten) Konvertierungen macht
- Faustregel: Nicht-Primitive Datentypen (z.B. `std::string`) immer per (const) Reference übergeben, damit keine Kopie erstellt werden muss
- Einen Operator als Member implementieren
- Wenn die Implementation kurz ist und keine Dependencies braucht, im Header implementieren
- Member-Funktionen in Klassen sind implizit `inline`

### Includes
- Je nach compiler sind include abhängigkeiten unterschiedlich. Daher immer explzit!
- `<iosfwd>` für `istream` und `ostream` im header, wenn keine streams instanziert werden (nur vorwärtsdeklartiert)

### Implementation
- Wenn ein Field const ist, kann es im Konstruktor nur über Initalizer gesetzt werden, nicht im body
- Reihenfolge in der Klasse gibt an, in welcher Reihenfolge fields initialisiert werden
- In Schlaufe auf `is.good()` prüfen, statt `while(is)`, da die bool-Konvertierung auch noch true zurück gibt, wenn `eof` erreicht ist. `good()` gibt nur true, wenn noch gelesen werden kann

---
## Vorlesung 12 - Class Templates
- Bei Klassen müssen Template-Parameter immer explizit angegeben werden (nicht wie bei Funktionen)

```c++
template <typename T> class Sack;
```
- Folie 5
    - `size_type` ist unter `std::vector` als Alias definiert für den Typ der Grösse (hier unsigned int)
    - `typename`: Typname ist abhängig vom Template, deshalb wird hiermit explizit gesagt, dass `size_type` ein Typ ist
- Concept von `T` in `Sack`:
    - `T` muss in `vector<T>` passen (CopyAssignable, CopyConstructable, ... - In Standard)
    - `T` darf nicht `void` sein
    - `T` muss kopierbar sein (wegen Rückgabe bei `getOut()` und Copy-constructor in dessen Implementierung - BTW, copy-Constructor wird vom Compiler generiert)
- Member, die ausserhalb der Klasse implementiert sind, mit `inline` kennzeichnen
    - Besser direkt in Klasse implementieren
- Template-Klassen immer in Headerfiles komplett definieren!
- Bei verebten Klassen `this->` verwenden, um auf vererbte Member der Parent-Klasse zu verwenden
    - Sonst könnte einen woanders definierten Member genommen werden
- Template Specialization: Quasi Overloading von Templates
    - Teilweise oder komplett spezialisiert
    - Template-Parameter leer lassen für komplette Spezialisierung

```c++
template <typename T>
struct Sack<T*> {
    ~Sack()=delete;
};
```
- Verhindert, dass `Sack` mit Template-Argument von Pointer erstellt wird
    - "Overloaded" das Standardverhalten
    - Der Destructor wird gelöscht
    - Dadurch kann es keinen Constructor geben -> Objekt kann nicht instanziiert werden
- *Folie 20 ff. relevant für Prüfung!*
- Constructor mit Initializer-List: Typ `std::initializer_list<T>` verwenden

```c++
template <typename T,
    template<typename...> class container=std::vector>
class Sack
{};
```

- Mehrere Template-Argumente `typename...`, weil `vector` zwei Template-Argumente nimmt (aber für zweites gibt es default-Wert)
- `decltype(auto)` impliziert Rückgabewert, wenn er von template-parameter abhängig ist
- Wenn von einem Template geerbt wird, müssen die Template-Parameter der Subklasse und Basisklasse übereinstimmen!

---
## Vorlesung 13 - main, Arrays, Pointer
- Bei `std::array` wird die Grösse immer als zweites Template angegeben
- In C wird bei Aufruf einer Funktion `int sum(int a[])` ein Pointer auf das erste Element mitgegeben. Die Funktion kennt nicht die Grösse des Arrays! Darum wird typischerweise eine Grösse mitgegeben
- Möglichkeit mit Templates: Grösse wird automatisch deduziert
```c++
template<size_t n>
int sum(int const (&a)[n]);
```
- Bindung von `&` stärker als `[]`, darum die Klammern

### Main-Function
- `int main(int argc, char *argv[])`
- `argv` ist array von char-Pointer -> Quasi Array von strings
    - Andere Schreibweise: `char **argv`
    - Alle Char-Sequzenzen sind mit `\0`-terminated
- `argc` ist Argument Counter (wegen C legacy)
- Char-Sequenzen können implizit nach `std::string` konvertiert werden

!!! danger
    Don't use pointers! Use `std::string`, `std::vector` and `std::array`

### Pointer als Iteratoren
- Pointer verhalten sich wie Random access Iteratoren (`begin()`)
- `argv` ist Iterator auf erstes Argument (= Programmname), `argv + argc` hinter das letzte (wie `end()`)

### Dynamic Heap Memory Management
- `new` und `delete` nie selbst verwenden!
    - Gibt schnell memory leaks
    - `new` erstellt Pointer und `delete` gibt es wieder frei
- Besser: `std::unique_ptr` und `std::shared_ptr`
    - Factory-Funktion `std::make_unique<T>` und `std::make_shared<T>`
    - Wird verwendet wie normaler Pointer - `*ptr`
    - `uniqe_ptr` ist nicht kopierbar (gibt es nur 1x)
    - `shard_ptr` kann es mehrfach geben (aufwändiger aufzuräumen), ähnlich zu Java
```c++
auto sp = make_shared(X)();
foo(X & x) {...}
if (sp) foo(*sp);
}
```
- check, ob Pointer noch gültig ist
- Vorsicht vor zirkulären Referenzen! Werden nicht abgeräumt
    - `weak_ptr` aus shared pointer erzeugen, er zählt aber den shared Pointer nicht hoch

---
## Vorlesung 14 - Inheritance / Dynamic Polymorphism
Dazu kommt Prüfungsfrage!!

### Inheritance
- Vererbung ist normalerweise private -> man will meist public!
- `class Subclass : public Baseclass`
    - Ausser bei structs: `struct Substruct : Basestruct`
- Per default werden die Konstruktoren nicht automatisch vererbt
    - mit `using Baseclass::Baseclass;` Konstruktoren erben
- Typdefinitionen immer mit Semikolon abschliessen
    - Darum `class MyClass {};`

#### Dynamic Dispatch
- Grundsätzlich wie in Java
- Damit das funktioniert, müssen Methdoen, die überschrieben werden, mit `virtual` deklariert werden
- Sonst wird immer Funktion im deklarierten Typ aufgerufen
- Damit Compiler dynamic dispatch machen kann
- In Subklasse `override` explizit angeben, damit Compiler Fehler wirft, falls nicht überschrieben werden kann (nicht virtual oder andere Signatur)
- Referenz auf Klasse hat einen (versteckten) Pointer auf "vtable" mit den Informationen der überschriebenen Funktionen
- Virtuelle Members, die mit `virtual fn() = 0` definiert sind, sind abstract -> Klasse ist abstract


- Die Basis-Klasse muss vor den eigenen Member-Variablen initialisiert werden
    - `DerivedWithCtor(int i) : Base{i}, mymember{i} {}`
- Man kann auch an einen eigenen Konstruktor delegieren
    - `DerivedWithCtor(int i) : DerivedWithCtor{i,i} {}`
- public, private und protected wie in Java
- Gleichnamige Funktionen von Basisklasse in Subklasse überdecken diejenigen in der Basisklasse! (Member Hiding)
    - Auch wenn Parameter unterschiedlich
    - Lösung: In Subklasse mit `using` Funktion "importieren", dann ist es wie ein normaler Overload
- Overloading und Templates (compile-time) sind oft effizienter als dynamic polymorphism
- Zuweisungen immer mit Referenzen, sonst wird eine Kopie der Basisklasse erstellt und die Subklasse geht verloren (slicing)
- Inheritance can be bad
    - Höchste Kopplung zwischen Klassen
- Value-Variablen von Typen haben kein dynamic dispatch, dafür immer Referenzen verwenden

- Bad Example
    - Output:
```c++
(a) -------
// Humminbbird hummingbird;
animal born
bird hatched
humminbird hatched
// Bird bird = hummingbird; - Kopie mit slicing
// Animal & animal = hummingbird - Referenz auf hummingbird
(b)----
// humminbird.makeSound();
beep
// bird.makeSound();
chirp // Compiler schaut nur auf Typ weil keine Referenz!
//animal.makeSound();
--- // makeSound() ist nicht virtual
(c)----
//humminbird.move();
hum // konkretes Objekt
// bird.move();
fly // konkretes Objekt
// animal.move()
hum // ACHTUNG dynamic dispatch, überschriebene Methode Bird::move ist automatisch virtual!!
(d) ----
//Aufruf Destruktoren in umgekehrter Reihenfolge
// Animal nur eine Referenz, darum keine Destruktion
// Destruktor bird
bird crashed
animal died
// Destruktor humminbird
hummingbird died
bird crashed
animal died
```

---
### Exercise 14
#### Theory questions
- mit `using` die `process(float)` von der Baseklasse einbinden
- `function()` in Baseklasse `virtual` deklarieren
- Pure virtual member: Die Klasse ist "abstrakt" und kann nicht instanziiert werden (Compiler-Error)
- Shared Pointers verwenden

#### Understanding dynamic Polymorphism and Object lifetime
- Forumtroll ft: forum_troll
- Troll t: troll - slicing!
- Monster m: forum_troll

Output:
```c++
a --------
a monster is bread // base ctor is always called!
a troll grows // base ctor
not quite a monster // ft ctor
// troll and monster use copy ctors
b --------
write stupid things // ft.attack()
clubbing kills me // t.attack(), t.myhealth = 9
write stupid things // m.attack() -> ft.attack()
c --------
swinging is healthy // ft.swing_club(), ft.myhealth = 11
clubbing kills me // t.swing_club(), t.myhealth = 8
d --------
troll-health:11 // ft.health()
troll-health:8 // t.health()
immortal? // m.health() - not virtual
end ------
troll petrified // t deconstruct
monster killed
troll banned // ft deconstruct
troll petrified
monster killed
```

- Lessons:
    - Base ctor is always called, no matter if it is declared explicitly like with `forum_troll():troll{}` or not
    - Deconstructor calls the deconstructor of the base class AFTER itself
