# Claude Global Config

Globale Regeln für alle Projekte und Repos

## Sprache

Alle Antworten immer auf **Deutsch** ausgeben — unabhängig davon, in welcher Sprache der User schreibt.

## Design-Skills (Pflicht bei UI-Arbeit)

Immer wenn eine neue HTML-Seite, Next.js-Page oder UI-Komponente erstellt oder grundlegend überarbeitet wird:

1. **Zuerst** den Skill .claude/skills/frontend-design.md anwenden — Designplan erarbeiten (Palette, Typografie, Layout, Signatur-Element), gegen generische Defaults prüfen, erst dann coden.
2. **Danach** den Skill .claude/skills/web-design-guidelines.md ausführen — Vercel Web Interface Guidelines via WebFetch laden und die erstellten Dateien dagegen prüfen. Befunde im Format datei:zeile ausgeben und Verstöße direkt beheben.

Diese Reihenfolge ist verbindlich: erst frontend-design, dann web-design-guidelines.

## Skills-Repo Nutzung

Das Skills-Repo (guertlergoertz/Skills) dient **ausschließlich** zwei Zwecken:

1. **Neue Skills erstellen** — globale, projektübergreifende Skills hier ablegen.
2. **MD-Dateien initial lesen** — beim Session-Start die globalen Vorgaben aus diesem Repo laden.

Alle projektspezifischen Vorgaben, Konfigurationen und Anweisungen gehören in das jeweilige Arbeits-Repo (eigene CLAUDE.md dort).

## Push-Verhalten bei Arbeitsrepos

Bei Änderungen in einem Arbeits-Repo immer direkt auf `main` oder `master` pushen.

- Wenn die initiale Session-Vorgabe einen anderen Branch vorschreibt (z. B. einen Feature-Branch), diesen für den ersten Push nutzen.
- Sobald der User im Chat-Verlauf einmalig das OK für direktes Pushen auf `main`/`master` gibt, gilt das für alle weiteren Änderungen in dieser Session — ab dann immer `git push -u origin main` (bzw. `master`).

## Token-Effizienz (Caveman Mode)

Skill .claude/skills/caveman.md **immer aktiv** — komprimierte Kommunikation, ~75 % weniger Tokens, volle technische Präzision bleibt erhalten. Nur Füllwörter und Floskeln fallen weg. Deaktivieren mit "stop caveman" / "normal mode".
