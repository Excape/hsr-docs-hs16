# Algorithmen und Datenstrukturen 2

## Vorlesung 1 - Binary Search Tree
- Ein Heap hat das kleinste oder grösste Element als Root
- Eine Suche auf einem Heap würde mit \(O(n)\) laufen, da man nicht weiss, welches Child jeweils kleiner / grösser ist
- Multimaps: Pro Key sind mehrere Values erlaubt

- **Folie 5**  
Voraussetzung: Random Access, Daten müssen sortiert sein

- **Folie 6** 
    - Insert: Finden der richtigen Position: \(O(log(n))\), Rest verschieben: \(O(n)\)  
    - Remove gleich wie Insert

- **Folie 7**  
    - Der Baum muss so aufgebaut sein, dass der linke Child-Knoten immer <= Parent-Node ist
    - Externe Knoten speichern keine Daten. Wenn man beim Suchen einen Blattknoten erreicht, ist der Key nicht gefunden
    - Eine Inorder Traversierung besucht die Keys in aufsteigender Reihenfolge

- **Einfügen**  
    - Suchen nach dem einzufügenden Key
    - Man wird auf einen "Sentinel" external Node treffen
    - Dort den neuen Key einfügen und in einen internen Knoten umwandeln
    - Bei einer *Multimap* kann es mehrere gleiche Keys geben. Dann:
        - find, bis man auf den Knoten trifft
        - nach links weiter suchen bis man auf einen external Node trifft
        - Dort einfügen und in internen Node umwandeln

- **Löschen**  
    - Knoten hat zwei Blatt-Kinder:
        - Lösche v und sein linkes Child, rechtes Child rutscht nach oben
    - Knoten hat ein Blatt-Kind:
        - Lösche v und den Blatt-Knoten (links oder rechts), der andere Knoten (mit einem Key) rutscht nach oben
    - Knoten hat keine Blatt-Kinder:
        - Finde den Knoten `w`, der `v` in der Inorder-Traversierung folgt ("links unten des rechten Teilbaums von `v`")
        - Kopiere den Key von `w` zu `V`
        - Lösche `w` mit dem gleichen Algorithmus wie bei Fall 1 und 2
    - *Für die ersten beiden Fälle ist der Algorithmus derselbe*

- **Performance**  
    - Worst Case: Komplett unbalancierter Baum (z.B. Einfügen mit *sortiertem* Input). Höhe ist \(O(n)\). Dann ist es eine Linked-List
    - Best Case: Mit der Mitte beginnen, komplett Balanciert. Höhe \(log(n)\)
    - Balanciert sich nicht selbst (wie andere Bäume, s. später)
