# Regression Test Selection System

## Überblick

Dieses Projekt implementiert ein System zur **Reduktion von Regressionstestmengen** in komplexen Java-basierten Softwaresystemen.

Ziel ist es, auf Basis von:

* Laufzeitdaten (Logs aus Grafana/Loki)
* Codeänderungen (Git-Commits)

eine **gezielte Auswahl relevanter Integrationstests** zu ermöglichen und optional eine **Priorisierung dieser Tests** durchzuführen.

Der Fokus liegt auf der Verbesserung der Feedbackzyklen im CI/CD-Prozess durch frühzeitige Ausführung relevanter Tests.

---

## Problemstellung

In der betrachteten Systemlandschaft werden Integrationstests:

* aus einer domänenspezifischen Sprache (DSL) generiert
* in einer separaten JVM ausgeführt (z. B. via Selenium)

Dadurch ist **statische Codeanalyse nicht anwendbar**, um Abhängigkeiten zwischen Tests und Code zu bestimmen.

Aktuell müssen alle Tests vollständig ausgeführt werden (z. B. im Nightly Build), was zu:

* langen Laufzeiten
* verzögertem Feedback
* ineffizienter Ressourcennutzung

führt.

---

## Lösungsansatz

Das System basiert auf einer **laufzeitbasierten Analyse**:

1. Testausführungen werden geloggt (Grafana/Loki)
2. Daraus wird ein Mapping erzeugt:

    * Test → aufgerufene Klassen/Methoden
3. Git-Commits werden analysiert:

    * geänderte Klassen/Methoden werden extrahiert
4. Relevante Tests werden selektiert:

    * Überschneidung zwischen Änderungen und Mapping
5. Optional: Priorisierung der selektierten Tests

---

## Architektur

Das System ist als **modularer Monolith** umgesetzt und folgt einer **hexagonalen Architektur**.

### Hauptmodule

* **mapping**
  Aufbau und Pflege der Beziehung zwischen Tests und aufgerufenen Klassen/Methoden

* **commitanalysis**
  Analyse von Codeänderungen (Git)

* **selection**
  Auswahl relevanter Tests basierend auf Änderungen und Mapping

* **prioritization**
  Sortierung selektierter Tests (z. B. heuristisch oder ML-basiert)

* **domain**
  Fachliches Kernmodell

* **application**
  Use Cases und Orchestrierung

* **adapter**
  Anbindung externer Systeme (Loki, Git, Datenbank, API)

Weitere Details siehe:

* `docs/architecture.md`
* `docs/domain-glossary.md`
* `docs/adr/`

---

## Datenfluss (vereinfachte Darstellung)

1. Logs → `TestExecutionTrace`
2. Aggregation → `TestCoverageMapping`
3. Commit → `CommitChangeSet`
4. Selektion → `SelectionResult`
5. (optional) Priorisierung → `PrioritizedTest`

---

## Technologien

* Java (>= 17)
* Spring Boot
* Maven
* JUnit 5
* (optional) Datenbank via JPA/Hibernate
* (optional) ML-Komponenten extern angebunden

---

## Build & Run

### Projekt bauen

```bash
mvn clean verify
```

### Tests ausführen

```bash
mvn test
```

### Anwendung starten

```bash
mvn spring-boot:run
```

---

## API / CLI (geplant)

Typische Anwendungsfälle:

* Mapping neu aufbauen
* Commit analysieren
* Tests selektieren
* Tests priorisieren

Details folgen im jeweiligen Modul.

---

## Projektstruktur (vereinfacht)

```text
src/main/java/de/verbis/testselection/
├── adapter/
├── application/
├── commitanalysis/
├── common/
├── config/
├── domain/
├── mapping/
├── port/
├── prioritization/
└── selection/
```

---

## Architekturregeln (Auszug)

* Modularer Monolith
* Hexagonale Architektur (Ports & Adapters)
* Keine Businesslogik in Controllern oder Adaptern
* Selektion und Priorisierung strikt getrennt
* Externe Systeme nur über Ports anbinden

---

## Entwicklungsrichtlinien

* Constructor Injection verwenden
* Kleine, fokussierte Klassen
* Domänenorientierte Benennung
* Öffentliche APIs mit JavaDoc dokumentieren
* Tests für alle nicht-trivialen Logiken

---

## Status

Dieses Projekt wird im Rahmen einer Bachelorarbeit entwickelt.

Der aktuelle Fokus liegt auf:

* klassenbasierter Testselektion
* Aufbau der Mapping-Wissensbasis
* Integration in den CI/CD-Prozess

---

## Ausblick

* Methodenbasierte Selektion
* ML-basierte Priorisierung
* Performance-Optimierung
* Integration in bestehende Build-Pipelines

---

## Autor

Jonathan Freund
Bachelorarbeit Informatik
Technische Hochschule Georg-Simon-Ohm Nürnberg

---

## Lizenz

Dieses Projekt dient Forschungs- und Ausbildungszwecken.
