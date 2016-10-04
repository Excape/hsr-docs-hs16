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
    - Best Case: Mit der Mitte beginnen, komplett Balanciert.
    -  Höhe \(log(n)\)
    - Balanciert sich nicht selbst (wie andere Bäume, s. später)
- **Implementierung**
    - Hier haben die Attribute der Klasse `Knoten` die Visibility `package`, d.h es kann direkt darauf zugegriffen werden
    - Einfügen: Map (nicht Multimap). Wenn der Key schon vorhanden ist, bleibt er einfach bestehen (key wird zurück gegeben)
---
## Vorlesung 2 - AVL-Trees (1)
Bei einem AVL Tree muss für jeden Teilbaum gelten, dass die Kinder maximal einen Höhenunterschied von 1 haben.

Beweis: Jeder AVL-Baum mit minimalen Anzahl Knoten \(n\) bei Höhe \(h\) hat einen linken und rechten Teilbaum. Der eine Teilbaum hat Höhe \(h - 1 \) und der rechte \(h - 2\)

$$n(h) = 1 + n(h-1) + n(h-2)$$
$$n(h) > 2n(h-2)$$
$$n(h-2) = 1 + n(h-3) + n(h-4)$$
$$n(h-2) > 2n(h-4)$$
$$n(h) > 4n(h-4)$$
$$n(h-4) = 1 + n(h-5) + n(h-6)$$
$$n(h-4) > 2n(h-6)$$
$$n(h) > 8n(h-6)$$
$$\ldots$$
$$n(h) > 16n(h-8)$$
$$n(h) > 2^i n(h-2i)$$

$$n_{min}(h=1) = 1$$
$$n_{min}(h=2) = 2$$

$$i: h-2i = (1 | 2)$$
$$h = (1 | 2) + 2i$$
$$\text{bsp.:}$$
$$i = 1 : h = 3 | 4$$
$$i = 2 : h = 5 | 6$$
$$i = 3 : h = 7 | 8$$
$$ i = \frac h2 - 1 \text{ wobei h/2 gerundet}$$
Einsetzen in \(n(h) > 2^i n(h-2i)\):
$$n > 2^{\frac h2 - 1} \cdot (1 | 2)$$
Konstanten heraus streichen:
$$n > 2^{\frac h2-1}$$
$$log(n) > \frac h2 - 1$$
$$h < 2\cdot log(n) + 2$$
$$\rightarrow h \in O(log(n))$$

- Einfügen:
    - Den neuen Knoten wie bei einem BST einfügen
    - Prüfen, ob AVL-Bedingungen verletzt wurden
    - Aus dem neuen Knoten aus solange nach oben wandern, bis man auf einen Eltern-Knoten eines unbalancierten Teilbaums trifft
- Umstrukturierung:
    - x, y, z: Aufgrund des Suchpfades Kind, Eltern, Grosseltern
    - a, b, c: Inorder-Reihenfolge
    - Wenn es von x bis z nur in eine Richtung geht: Rotation um b (= y)
    - Bei Richtungsänderung von x bis z:
        - Zuerst den Teilbaum b und c (x und y) rotieren, so dass a, b, c wieder in einer Richtung ist wie oben
        - Wieder wie oben um b rotieren
    - *(Tipp:)* Jeweils nach Rotation mit Inorder Traversierung prüfen
    - Eine Restrukturierung mit Cut/Link muss nicht den gleichen Baum ergeben wie mit dem Rotationsverfahren!
- Löschen
    - Löschen wie bei BST
    - Die Balance kann verletzt werden
    - Die Knoten x, y, z sollten im höheren Teilbaum sein (beim Einfügen automatisch gegeben)
    - Nach dem Restrukturierung kann eine neue Unbalance entstehen! (Im Gegensatz zum Einfügen). Man muss bis zur Root weiter nach Unbalancierten Teilbäumen suchen
- Implementierung
    - Mit einer AVL-Node wird dessen Höhe gespeichert
    - `actionPos`: Die Position, in der etwas passiert ist (z.B. letzte Einfügeposition). Ist ein Attribut der BST-Klasse
    - Nach dem Einfügen wird die (BST-)Node (`Item`) mit einer Instanz von `AVLItem` ersetzt
    - Besser: Funktion `newNode()` des BST überschreiben
