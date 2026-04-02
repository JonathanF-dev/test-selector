# Architekturübersicht

## Ziel des Systems
Dieses System dient der Reduktion von Regressionstestmengen in einem Java-basierten Softwaresystem.
Da klassische statische Analyse für die vorliegende Testarchitektur nicht anwendbar ist, wird die Beziehung
zwischen Tests und tatsächlich aufgerufenen Java-Klassen bzw. -Methoden aus Laufzeitlogs abgeleitet.
Auf Basis von Commit-Änderungen und dieser Mapping-Wissensbasis werden relevante Tests ausgewählt und
optional priorisiert.

## Fachlicher Kontext
Die Integrationstests werden in einer getrennten Laufzeitumgebung ausgeführt.
Dadurch kann nicht direkt über statische Analyse bestimmt werden, welche Java-Klassen durch einzelne Tests
tatsächlich ausgeführt werden.
Stattdessen werden Laufzeitdaten aus Grafana/Loki ausgewertet, um ein Mapping zwischen Tests und
aufgerufenen Klassen/Methoden aufzubauen.

## Architekturstil
Das System ist als modularer Monolith aufgebaut.

Die interne Struktur orientiert sich an einer hexagonalen Architektur:
- fachliche Kernlogik ist von Infrastruktur getrennt
- externe Systeme werden über Ports und Adapter angebunden
- Businesslogik darf nicht in Controllern oder Infrastruktur-Adaptern liegen


## Hauptmodule

### mapping
Verantwortlich für die Erzeugung und Pflege der Beziehung zwischen Tests und tatsächlich
aufgerufenen Klassen bzw. Methoden auf Basis von Laufzeitlogs.

### commitanalysis
Verantwortlich für die Analyse von Codeänderungen und die Ermittlung betroffener Java-Artefakte
auf Klassen- oder später Methodenebene.

### selection
Verantwortlich für die Auswahl relevanter Tests auf Basis von Commit-Änderungen und Mapping-Daten.

### prioritization
Verantwortlich für die Sortierung bereits ausgewählter Tests anhand definierter Strategien
oder ML-basierter Verfahren.

### adapter
Enthält technische Anbindungen an externe Systeme wie Loki, Git, Datenbank oder REST.

### application
Enthält Use Cases und Orchestrierung der fachlichen Abläufe.

### domain
Enthält das fachliche Kernmodell und domänennahe Services.

## Datenfluss

1. Laufzeitlogs werden aus Loki geladen.
2. Aus den Logs werden TestExecutionTraces erzeugt.
3. Die Traces werden zu TestCoverageMappings verdichtet.
4. Ein Git-Commit wird analysiert und in ein CommitChangeSet überführt.
5. Die Selection-Komponente bestimmt relevante Tests anhand der Überschneidung
   zwischen geänderten Klassen und bekannten Coverage-Mappings.
6. Optional werden die selektierten Tests priorisiert.

## Architekturregeln

- Businesslogik gehört in domain-, mapping-, commitanalysis-, selection- oder prioritization-Services.
- Controller, CLI-Kommandos und Scheduler dürfen nur Use Cases aufrufen.
- Adapter enthalten keine fachliche Entscheidungslogik.
- Externe Systeme werden ausschließlich über Ports angebunden.
- Die Priorisierung ist strikt von der Testauswahl getrennt.
- Die erste Ausbaustufe der Testauswahl arbeitet auf Klassenebene.
- Domain-Klassen sollen möglichst framework-unabhängig bleiben.

## Erlaubte Abhängigkeiten

Erlaubte Richtung:
- adapter.in -> application
- application -> domain
- application -> port.out
- adapter.out -> port.out
- fachliche Module -> domain

Nicht erlaubt:
- domain -> adapter
- selection -> direkt auf ML-Infrastruktur
- controller -> repository
- prioritization -> selection-internen Zustand manipulieren

## Zentrale Domänenbegriffe

- TestExecutionTrace: Rohrepräsentation der beobachteten Aufrufe eines Tests
- ObservedInvocation: einzelner beobachteter Klassen- oder Methodenaufruf
- TestCoverageMapping: verdichtete Wissensbasis eines Tests
- CommitChangeSet: Menge geänderter Artefakte eines Commits
- SelectionResult: Ergebnis der Testauswahl
- SelectedTest: einzelner ausgewählter Test mit Begründung

## Erweiterungspunkte

- Erweiterung von Klassen- auf Methodenebene
- ML-basierte Priorisierung
- zusätzliche Selektionsstrategien
- Auslagerung einzelner Adapter oder der Priorisierung in separate Services