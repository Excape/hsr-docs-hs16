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
