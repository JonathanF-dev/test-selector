# Pull Request

## Ziel
<!-- Was wurde umgesetzt? -->
<!-- Beispiel: Implementierung der klassenbasierten Testselektion -->

---

## Motivation
<!-- Warum ist diese Änderung notwendig? -->
<!-- Bezug zur Problemstellung oder Zielsetzung -->
<!-- Beispiel: Ermöglicht erste gezielte Testauswahl basierend auf Commit-Änderungen -->

---

## Umsetzung
<!-- Welche Komponenten wurden implementiert oder geändert? -->
<!-- Auflistung der wichtigsten Klassen/Module -->

-
-
-

---

## Architektur
<!-- Warum ist die Lösung architektonisch korrekt? -->
<!-- Bezug auf Architekturregeln (Monolith, Hexagonal, Modultrennung) -->

- Einhaltung der Modulgrenzen:
- Verwendung von Ports/Adaptern:
- Trennung von Selektion und Priorisierung:

---

## Datenfluss (optional)
<!-- Kurze Beschreibung des Ablaufs -->

1.
2.
3.

---

## Tests
<!-- Welche Tests wurden hinzugefügt oder angepasst? -->

- [ ] Unit-Tests vorhanden
- [ ] Integrationstests vorhanden (falls relevant)
- [ ] Bestehende Tests laufen erfolgreich

---

## Dokumentation
<!-- Wurde Dokumentation aktualisiert? -->

- [ ] JavaDoc ergänzt
- [ ] docs/ aktualisiert
- [ ] architecture.md angepasst (falls nötig)
- [ ] domain-glossary.md berücksichtigt

---

## Offene Punkte / Einschränkungen
<!-- Was ist noch nicht vollständig oder bewusst ausgelassen? -->

-
-

---

## Checkliste

- [ ] Code kompiliert (`mvn clean verify`)
- [ ] Architekturregeln eingehalten
- [ ] Keine Businesslogik in Adaptern/Controllern
- [ ] Naming entspricht Domain-Glossary
- [ ] Keine unnötigen Abhängigkeiten eingeführt