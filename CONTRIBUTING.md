# Contributing to GUARDIAN xMS

Vielen Dank für dein Interesse an GUARDIAN xMS.

Das Projekt befindet sich derzeit in der Architekturphase. Ziel ist eine langfristig wartbare, herstellerneutrale Plattform für das Management technischer Systeme.

## Grundprinzipien

1. Architektur vor Implementierung.
2. GitHub ist die Single Source of Truth.
3. Änderungen an der Architektur werden dokumentiert.
4. Grundsatzentscheidungen erhalten einen Architecture Decision Record (ADR).
5. Code ohne Tests wird nicht gemergt.
6. Herstellerabhängige Logik gehört in Adapter oder TypePackages.

## Empfohlener Workflow

Issue
→ Architektur
→ ADR (falls erforderlich)
→ Implementierung
→ Tests
→ Dokumentation
→ Pull Request
→ Merge

## Branches

- main
- feature/*
- fix/*
- docs/*

## Commit-Nachrichten

Beispiele:

- docs: add software architecture document
- docs: define CMDB model
- feat: implement event engine
- fix: correct health calculation
- test: add replay tests
