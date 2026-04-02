# Domain Glossary

Dieses Dokument definiert zentrale Fachbegriffe des Systems zur Regressionstestselektion.
Alle Begriffe sind verbindlich und sollen konsistent im Code, in der Dokumentation
und in der Bachelorarbeit verwendet werden.

---

## TestCase
Ein ausführbarer Integrationstest, der über die Testautomatisierung generiert und ausgeführt wird.

---

## TestExecutionTrace
Die Gesamtheit aller während der Ausführung eines Tests beobachteten Aufrufe von Klassen
und optional Methoden.

Ein Trace gehört immer zu genau einem Test und einer konkreten Build-Ausführung.

---

## ObservedInvocation
Ein einzelner beobachteter Aufruf innerhalb eines TestExecutionTrace.

Enthält:
- aufgerufene Klasse
- optional aufgerufene Methode
- Zeitstempel

---

## TestCoverageMapping
Die verdichtete Repräsentation der Beziehung zwischen einem Test und den von ihm
aufgerufenen Klassen bzw. Methoden.

Wird aus mehreren TestExecutionTraces aggregiert.

---

## CommitChangeSet
Die Menge aller durch einen Commit geänderten Java-Artefakte.

Enthält:
- betroffene Klassen
- optional betroffene Methoden

---

## ChangedArtifact
Ein einzelnes durch einen Commit betroffenes Artefakt.

Kann sein:
- Klasse
- Methode

---

## Selection
Der Prozess der Bestimmung, welche Tests durch eine Codeänderung betroffen sein könnten.

Die Selektion basiert auf der Überschneidung zwischen:
- geänderten Klassen (CommitChangeSet)
- bekannten TestCoverageMappings

---

## SelectionResult
Das Ergebnis der Testauswahl für einen Commit.

Enthält:
- Liste selektierter Tests
- Begründung für jede Auswahl

---

## SelectedTest
Ein einzelner durch die Selektion ausgewählter Test.

Enthält:
- TestId
- Auswahlbegründung
- optional Konfidenzwert

---

## Prioritization
Der Prozess der Sortierung bereits selektierter Tests nach Relevanz.

Die Priorisierung beeinflusst die Reihenfolge, nicht die Auswahl.

---

## PrioritizedTest
Ein Test mit zugewiesenem Prioritätswert.

---

## Klassenbasierte Selektion
Selektion auf Basis von Klassen:
Ein Test wird ausgewählt, wenn er mindestens eine Klasse verwendet,
die im Commit geändert wurde.

---

## Methodenbasierte Selektion
Feingranulare Variante:
Ein Test wird nur ausgewählt, wenn eine tatsächlich verwendete Methode geändert wurde.

---

## Laufzeitbasierte Analyse
Analyseansatz, bei dem tatsächliche Programmausführung verwendet wird,
um Abhängigkeiten zwischen Tests und Code zu bestimmen.

---

## Statische Analyse
Analyse ohne Programmausführung (z. B. Code-Parsing).
In diesem System nicht anwendbar aufgrund getrennter Laufzeitumgebungen.
