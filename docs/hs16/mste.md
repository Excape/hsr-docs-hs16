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

## Vorlesung 4 - Vererbung (Selbststudium)
- Structs sind nicht vererbbar, können aber interfaces implementieren
- Prüfen ("instanceof"): `obj is T`
- Explizit casten
    - `(T) obj`, bei Fehler zur RuntimeInvalidCastException
    - `obj as T`, bei Fehler zur Runtime `null`
- Methode muss `virtual` markiert werden, damit sie überschrieben werden kann
- überschreiben mit `public virtual void method1()...`
- Per default sind Methoden nicht überschreibbar und überschreiben nicht implizit eine virtual Methode
- `override` Methoden sind aus Subklassen weiter überschreibbar
- `abstract`-Methoden sind implizit `virtual`
- Wenn eine Methode aus der Basisklass neu definiert wird (nicht mit `override`), überdeckt sie die Methode der Basisklasse
    - Mit `public new void ...` markieren, sonst gibt es eine Compilerwarnung
    - Dies unterbricht Dynamic binding. Von der Base-Klasse her wird die letzte überschriebene genommen
```csharp
Base b1 = new SubSub();
b1.J();
// Base.J()
((Sub)b1).J();
// Sub.J()
((SubSub)b1).J();
// SubSub.J()
```
- `sealed` Classes verhindern, abgeleitet zu werden
- überschriebene Methoden, die `seal` verwenden, können von einer weiteren Subklasse nicht mehr überschrieben werden 
- Mit `new` können sie aber weiterhin überdeckt werden
- Interface Name-Clash
    - Wenn Signatur und Rückgabetyp identisch: Eine Implementierung für beide Interfaces
    - Sonst Methode für jedes Interface explizit implementieren mit `<iface>.<method>`
    - Es kann auch eine Default-Implementation verwendet werden und für bestimmte Interfaces eine explizite
- Garbage Collection
    - `~MyClass() {}` ist Destruktor
        - Syntactic Sugar für `Finalize()`
    - Finalize wird vom GC aufgerufen, sollte möglichst schnell sein
    - `IDisposable` implementieren, um Freigabe selbst zu implementieren
    - Mit `using () {...}` wird am Ende `Dispose()` aufgerufen
    - Dispose pattern
``` csharp
~DataAccess()
{ Dispose(false); }
public void Dispose()
{
    Dispose(true);
    System.GC.SuppressFinalize(this);
}
protected virtual void Dispose(bool disposing)
{
    if (disposing)
    {
        // Dispose managed ressources
        if (connection != null)
        { connection.Dispose(); }
    }
    // Dispose unmanaged resources
    }
```

- So werden managed ressourcen von Dispose() aufgerufen, und unmanaged vom Finalize / Destruktor

---
## Vorlesung 5 - Delegates & Events
### Delegates
- Typsichere function-Pointer
- Rerence-Type, um Methoden als Parameter zu übergeben
- Delegate kann 0 bis n Methoden beinhalten
- Definition eines Delegates ist die Deklaration eines neuen Reference-Types (normalerweise ausserhalb von Klassen definieren!)
- Initialisieren mit `new <delegate-name>(<method-name>)`
    - Kurzform: Direkt Methode zuweisen: `<var> = <method-name>`
- Delegate-Instanz kann ausgeführt werden mit `()`
- Signatur von Methoden wird in Delegate-Definition festgelegt
- Jede Methode mit richtiger Signatur kann zu Delegate zugewiesen werden
- Methode kann instanzbezogen sein: `obj.Method`, `obj` ist eine Instanz
- Es sollte vor dem Aufruf geprüft werden, ob das Delegate != null ist
    - Ab C# 6.0 `delegateVar?.Invoke(params)`

#### Multicast-Delegates
- Jedem Delegate können mehrere Methoden hinterlegt werden
- Zuweisung mit `+=` und `-=`
- Beim Ausführen werden die Methoden sequentiell ausgeführt
- Im Hintergrund baut der compiler eine Klasse, die von `MulticastDelegate` erbt
- Wenn mehrere Delegate-Methoden einen Rückgabewert geben, wird der letzte genommen
- Anwendung z.B. bei `List.Sort(Comparison<T>)`

### Events
- Compiler-Feature für Delegates
- Bei Instanzierung von Delegate `public event <Delegate> <var>` verwenden
- Delegate wird intern private deklariert
- generiert Methoden für Subscribe und Unsubscribe
- Observers anmelden mit += und -=
- Verhält sich gleich, als ob man einfach das Delegate-Feld in der Klasse public macht, aber von aussen kann das Delegate nicht ausgeführt werden (wäre sehr schlecht)
- Best Practice:
    - `public delegate void AnyHandler(object sender, AnyEventArgs e);`
    - `AnyEventArgs` ist eine Subklasse von `EventArgs`
    - Grund: Wenn z.B. Library erweitert wird, ändert sich Delegate-Signatur nicht, es wird nur die EventArgs-Klasse erweitert. Der Client-Code funktioniert weiterhin

### Anonyme Methoden
```csharp
list.ForEach(delegate(int i)
    { Console.WriteLine(i); }
);
```
---
## Vorlesung 6 - Generics (Selbststudium)
- Generische Parameter: `T`, bei mehreren: `T1`, `T2`, etc.
- Type Constraints
    - `class MyClass<T> where T : <constraint>`

---
## Vorlesung 7 - Iteratoren & Exceptions
### Iteratoren
- For-Each-Loop
    - Type muss `IEnumerable` oder `IEnumerable<T>` implementieren
    - *Oder* einer Implementation von IEnumberable ähneln, d.h. muss GeEnumerator() haben, das ein Enumerator zurückgibt, Enumerator muss MoveNext() haben und das Property Current
    - Compiler baut den Loop um in While-Loop mit dem Enumerator
- Wenn man mit eigener Klasse IEnumberable<T> implementiert, muss auch die nicht-generische Variante implementiert werden
- Collection darf nicht veränder werden während der Iteration (wie Java)
- Vereinfachung mit `yield`
    - In der `GetEnumerator()` Methode mindestens ein `yield return` statement verwenden
    - Bei jeder Iteration (`MoveNext()`) wird das nächste `yield` Statement ausgeführt (genauer das Statement *nach* dem letzten `yield`)
    - Implementation des Interfaces optional (siehe oben)
    - Compiler erstellt eine innere Klasse mit komplizierter State-Machine (langsamer als eigene Implementierung)
    - Es kann auch ein spezifischer Iterator mit Rückgabetyp `IEnumberable<T>` definiert werden, der `yield` verwendet

```csharp
MyIntList list = new MyIntList();
foreach (int elem in list.Range(2, 7))
{
    /* ... */
}
public IEnumerable<int> Range(int from, int to) {
for (int i = from; i < to; i++)
    yield return data[i];
}
```

---
## Vorlesung 8 - LINQ (Selbststudium)
- LINQ ist reine Compiler-Technologie
- Auf Objektstruktur
- Query Syntax wird vom Compiler in Lambdas umgewandelt
- Die Parameter der Lambdas müssen implizit in den Delegate-Parameter konvertierbar sein, sonst müssen sie explizit angegeben werden
- LINQ fügt IENumerable extension Methods hinzu
- Normalerweise deffered evaluation. Wenn aber rückgabewert kein IEnumerable ist, wird die "query" direkt ausgeführt (z.B. `ToList()`, `Count()`, `Sum()`, etc)
- Im Lambdas kann man auf Variablen ausserhalb des Scopes zugreifen und diese verändern (müssen nicht final sein wie in Java)
- C# 6.0 Feature: Expressoin-Bodied Methods
    - Methode mit einem Statement: `public void Print() => Console.WriteLine("Hello");`
    - Read-Only Properties: `public int MyProperty => 0;`
- Collectoin Initializer: `var l1 = new List<int> {1, 2};`
    - Geht seit C# 6.0 auch mit Dictionaries
- Anonyme Types werden vorallem mit LINQ verwendet
    - Zuweisung nur zu Variable mit `var` deklariert

---
## Vorlesung 9 - Entity Framework (1) (Selbststudium)
- Ansatz Code First: Code schreiben, der dann zur Laufzeit zu Datenbank umgewandelt wird (falls nicht existierend)
- Ansatz Model First: Modell designen, wird in Code umgesetzt
- DSL: Domain specific language, eine Sprache, Objekte und ihre Assoziationen zu beschreiben
- CSDL (Conceptual Schema Definition Language) *ist eine DSL* für .NET
- Conceptual Model wird vom Framework in Logical Model gemapped
- Entity Types können voneinander erben (wird vom OR-Mapper entsprechend in relatione Objekte abgebildet)
- Complex Types: Mehrere Properties auf relationaler Ebene können auf dem Schema in ein Complex-Type zusammengefasst werden. Auf CLR-Ebene sind diese dann eigene Types, auf der Datenbank nur Spalten der entsprechenden Tabelle
- Bei Code first kann man die Mappings auf 3 verschiedene Arten definieren
    - *By Convention*: Es gibt conventions, wie die Namen von Properties gemapped werden, z.B. das "Id" Property als Primary Key
    - *By Attributes*: Den Properties und Klassen Attributes geben, z.B. `[Column("name")]` um Namen zu überschreiben, `[Required]` für not-null
    - Mit *DbModelBuilder*: Für jede Klasse ein `DbModelBuilder` erstellen und mit Methoden darauf (`HasKey()` für Primary key) die Mappings definiert. Statt Annotationen
- Seed Database: Von einer der Klassen `DropCreateDataBaseAlways<myDb>` etc. erben und `Seed()` überschreiben. Im eigenen Context dann `Database.SetInitializer(new MyDbSeed())` setzen

---
## Vorlesung 10 - Entitiy Framework (2) (Selbststudium)
- *Identity Map*: Quasi ein Cache, der alle Objekte zwischenspeichert, die gequeried wurden
- Jeder DBContext führt eine eigene Identity Map
- Navigation Properties: Neue Entries können sowohl bei der "n"-Seite der Relation hinzugefügt werden (`order.Customer = customer`) oder auf der "1"-Seite (`customer.Orders.Add(order)`) 
- *Unit of Work Pattern*: Pattern, dass von DBContext implementiert wird. Alle Änderungen werden festgehalten und danach in die Datenbank gesynct
- *Optimistic offline lock*: Transaktion wird ausgeführt in der Annahme, dass keine Konflikte entstehen. Gibt es doch welche, wird ein Rollback gemacht
- `IQueryable` kann implementiert werden, um z.B. einen eigenen Treiber für eine DB zu bauen. Der wird dann speziell mit einem Expression-Tree aufgerufen

---
## Vorlesung 11 - WCF (1)
- Client und Server kommunizieren über Messages, standardmässig über SOAP (Serialisiertes XML)
- App-Domain: Kapselung in einem Prozess, in einem Prozess können mehrere App-Domains laufen
- Client greift über Proxy auf den Service zu
    - Kann in gleicher App-Domain, in gleichem Prozess, auf gleicher maschine oder über das Netzwerk geschehen
- Kommunikation immer über *Endpoints*
    - Jeder Endpoint besteht aus *Address*, *Binding* (Kanalbeschreibung) und *Contract* (Interface-Definiton, DTOs, definiert die Service-Schnittstelle)
    - Binding und Adress werden per Konfiguration gesteuert, der Contract wird im Code implementiert
- *Metadata Exchange Endpoint*: Gibt XML, wie die Service-Schnittstelle definiert ist
    - Mit `svcutil.exe` kann Code generiert werden entsprechend der Schnittstelle
- WC*: Web Service Standard (riesig), wovon WCF einen grossen Teil implementiert
- App.config wird immer "heraus" kompiliert vom DLL, kann also auch im nachhinein noch geändert werden

#### Contracts
```csharp
[ServiceContract]
public interface ITimeService
{
    [OperationContract]
    string GetTime();
    [OperationContract]
    TimeDescData GetTimeDesc();
}
public class TimeService : ITimeService
{
    public string GetTime()
    { return DateTime.Now.ToString(); }
    public TimeDescData GetTimeDesc()
    { return new TimeDescData(); }
}
```
- DTO:
```csharp
[DataContract]
public class TimeDescData
{
    public TimeDescData()
    {
        TimeLong =
        DateTime.Now.ToLongDateString();
        TimeShort =
        DateTime.Now.ToShortDateString();
    } 
    [DataMember]
    public string TimeShort { get; set; }

    [DataMember]
    public string TimeLong { get; set; }
}
```
- Attribute für Prüfung wichtig!

### Konfiguration
- Der Name des Services muss der Typ des implentierten Services sein
- Das Interface wird als Contract angegeben
- Endpoint-Adresse ist relativ zur `baseAddress`
    - Leerer String heisst "auf /"
- Konfiguration kann in app.config oder in Programmcode gegeben werden

### Client
- WSDL kennt nur Collections und Dictionaries, ist nicht .NET spezifisch
- Wenn der Service sich ändert, muss im Client-Projekt die Service-Referenze von Hand aktualisiert werden
- Properties auf dem Service werden abgebildet auf `get_` und `set_` Methoden, über `[OperationContract(Name="<name>")]` kann der Name angepasst werden
- Client mit *Shared Assembly*: Server und Client haben gleiche Library, es gehen keine Type-Information über WSDL verloren
    - Vorteil: Änderungen werden automatisch auf dem Client übernommen, Typeninformationen gehen nicht verloren
    - Nachteil: Client hat auch Member, die nicht mit `[DataMember]` deklariert wurden -> Sie werden nicht über den Service serialisiert, der Client hat sie aber trotzdem!

### Faults
!!! warning
    Faults sind keine Exceptions, sondern reine Informationsobjekte

- Exceptions können nicht serialisiert werden
- Sie werden über `FaultException` gekapselt
- Leitet nicht von `Exception` ab!
- Error-Handling
    - Alle Exceptions catchen und neue `FaultException<T>` oder allgemeine `FaultException` werfen

### Kommunikations-Muster
- Standardmässig sind alle Vorgänge synchron
- `[OperationContract(IsOneWay=true)]` wartet nicht auf Server (asynchron), erwartet aber auch keine Antwort

#### Duplex-Kommunikation
```csharp
[ServiceContract(SessionMode = SessionMode.Required, CallbackContract = typeof(ICalcCallback))]
public interface ICalculatorDuplex {
    // IsOneWay = true Operationen
}
```

- Das Callback-Interface wird vom Client implementiert (ist kein ServiceContract!)
- Callback auf Serverseite mit `OperationContext.Current.GetCallBackChannel<ICalcCallback>()` holen
- Client: (`CallbackHandler` implementiert Callback-Interface)

```csharp
CallbackHandler h = new CallbackHandler();
InstanceContext ic = new InstanceContext(h);
CalculatorDuplexClient client = new CalculatorDuplexClient(ic);
```
- Asynchrone Programmierung macht Debugging schwieriger

---
## Vorlesung 12 - WCF (2)
- Übertragungskanal wird auch via MEX übertragen
    - Gibt Protokolle und Encoding vector
- `BasicHTTPBinding` unterstützt keine Duplex-Endpoints, dazu `WSDualHttpBinding` verwenden
- Vererbung
    - Wenn eine Operation einen komplexen Typ verwendet, der vererbt wird, und diese auch verwendet werden sollen, muss die Basis-Klasse mit `[Type(typeof(MySubclass))]` markiert werden
    
    ```csharp
    [ServiceContract]
    public interface IClassroomService
    {
        [OperationContract]
        List<Student> GetStudents();
    } 
    
    [KnownType(typeof(TiredStudent))]
    [KnownType(typeof(BoredStudent))]
    [DataContract]
    public class Student { }
    
    [DataContract]
    public class TiredStudent : Student { }
    
    [DataContract]
    public class BoredStudent : Student { }
    ```

### Serialisierung
- Default: Alles wird nacheinander serialisiert
    - Auch wenn zwei Referenzen auf das gleiche Objekt zeigen -> Redundanz in Serialisierung
- `[DataContract(IsReference = true)]`: Referenzen werden berücksichtigt
    - Problematisch bei Zyklen

### Service Behaviour
- Behaviour kann mit `[ServiceBehaviour(InstanceContextMode=..)]` *auf Service-Implementation* definiert werden
- Single: Ein einzige Service-Instanz (Singleton)
- Per Call: Jeder Aufruf eine neue Instanz (Stateless)
- Per Session
    - Auf Interface kann Anforderungen gestellt werden. `Allowed`, `NotAllowed`, oder `Required` (Implementation *muss* per Session sein)
    - Auf Operationen kann mit `IsInitiating = true` (default) oder `IsTerminating = true` eine Session gestaret / beendet werden
- `RequireOrderDelivery`: Festlegen, ob die Reihenfolge beibehalten werden muss
- *Transaktionen sind nicht prüfungsrelevant*

---
## Vorlesung 13 - Reflection / Attributes
### Reflection
- WCF z.B. nutzt Type Discovery, um Service Attribute zu setzen
- Einstiegspunkt immer über `System.Type`
    - Ist Abstrakt, es wird Unterklasse `RuntimeType` verwendet
- Aufruf mit `<obj>.GetType()` oder `typeof(<classname>)`
- Der Typ `System.Type` beschreibt sich auch selbst (gibt selbe Instanz zurück)
- Mit dem Typ-Objekt
    - `GetMembers()` für alle Member
    - `GetFields()`
    - `GetProperties()`
    - `GetEvents()`
    - `GetConstructors()` und `GetMethods()`
        - Darauf wiederum `GetParameters()`
- Alle diese Rückgabe-Typen (und Type) sind abgeleitet von `MemberInfo`
- Properties werden über Reflection mit Setter und Getter Methoden aufgelöst
- Den Methoden kann u.a. `BindingFlags`  mitgegeben werden, um z.B. nur public, private, usw. zu filtern
- `DeclaredOnly`: Nur Member, die in Klasse deklariert sind, also nicht z.B. vererbte Member
- Reflection ist älter als Generics, es werden alle Generics mit Object abgebildet
- Felder können auf `FieldInfo` mit Get- und SetValue verändert werden. Dabei muss die Instanz mitgegeben werden
- Das gleiche geht auch mit `PropertyInfo` mit ein paar zusätzlichen Informationen (`CanRead()`, `CanWrite()`)
- Methoden können mit `Invoke()` aufgerufen werden

### Attributes
- Attribute werden über Reflection abgegfragt
- public, static, etc. sind eigentlich auch Attribute aus Sicht des Compilers
- Attribute sollen Deklarationen um weitere Aspekte erweitern ("Aspektorientierte Programmierung")
- Benutzt z.B. für Security, Serialisierung, OR-Mapping, Dokumentation, etc.
- Als Parameter an Attribute können nur *konstante* Werte übergeben werden
- Auf `MemberInfo` kann mit `GetCustomAttributes()` die Attribute geholt werden
    - Mit `IsDefined(<AttrbutType>)` abfragen, ob Attribut auf einem bestimmten Typ gesetzt wurde
- Mit Named Parameter können auch Properties auf dem Attribut gesetzt werden, die nicht über den Konstruktor initialisiert werden


#### Eigenes Attribut
```csharp
[AttributeUsage(
AttributeTargets.Class |
AttributeTargets.Constructor |
AttributeTargets.Field |
AttributeTargets.Method |
AttributeTargets.Property,
AllowMultiple = true)]
public class BugfixAttribute : Attribute
{
    public BugfixAttribute(int bugId,
    string programmer, string date)
    { /* ... */ }
}
```
- Mit `[AttributeUsage()]` festlegen, wo es verwendet werden kann, und ob es auf dem *gleichen Typ* mehrmals verwendet werden darf
- Muss auf "Attribute" enden, damit es später abgekürtzt als `[Bugfix]` verwendet werden kann

### Code Emittierung
- Todo
