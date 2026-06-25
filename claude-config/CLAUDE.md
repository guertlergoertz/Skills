# Claude Global Config

Globale Regeln für alle Projekte und Repos

## Sprache

Alle Antworten immer auf **Deutsch** ausgeben — unabhängig davon, in welcher Sprache der User schreibt.

## Session-Hook Auto-Setup

Am Anfang jeder Session prüfen, ob die folgenden zwei Dateien im aktuellen Projektverzeichnis (`$CLAUDE_PROJECT_DIR`) existieren:

- `.claude/hooks/session-start.sh`
- `.claude/settings.json`

Wenn eine oder beide fehlen: sofort anlegen mit exakt diesem Inhalt und committen + pushen (ohne Rückfrage).

Inhalt `.claude/hooks/session-start.sh`:
```bash
#!/bin/bash
set -euo pipefail

# Nur in Claude Code on the Web ausfuehren
if [ "${CLAUDE_CODE_REMOTE:-}" != "true" ]; then
  exit 0
fi

SKILLS_REPO="https://github.com/guertlergoertz/Skills.git"
TEMP_DIR=$(mktemp -d)
trap "rm -rf \"$TEMP_DIR\"" EXIT

echo "Syncing global skills from guertlergoertz/Skills..."
git clone --depth 1 --quiet "$SKILLS_REPO" "$TEMP_DIR/skills"

mkdir -p ~/.claude/skills
cp -rf "$TEMP_DIR/skills/claude-config/.claude/skills/." ~/.claude/skills/

if [ -f "$TEMP_DIR/skills/claude-config/CLAUDE.md" ]; then
  cp "$TEMP_DIR/skills/claude-config/CLAUDE.md" ~/.claude/CLAUDE.md
fi

echo "Skills synced successfully:"
ls ~/.claude/skills/
```

Inhalt `.claude/settings.json`:
```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/session-start.sh"
          }
        ]
      }
    ]
  }
}
```

Nach dem Anlegen: `.claude/hooks/session-start.sh` als ausführbar markieren (`chmod +x`), dann beide Dateien committen und auf den aktuellen Branch pushen.

Wenn beide Dateien bereits vorhanden sind: nichts tun, normal weiterarbeiten.

## Design-Skills (Pflicht bei UI-Arbeit)

Immer wenn eine neue HTML-Seite, Next.js-Page oder UI-Komponente erstellt oder grundlegend überarbeitet wird:

1. **Zuerst** den Skill .claude/skills/frontend-design.md anwenden — Designplan erarbeiten (Palette, Typografie, Layout, Signatur-Element), gegen generische Defaults prüfen, erst dann coden.
2. **Danach** den Skill .claude/skills/web-design-guidelines.md ausführen — Vercel Web Interface Guidelines via WebFetch laden und die erstellten Dateien dagegen prüfen. Befunde im Format datei:zeile ausgeben und Verstöße direkt beheben.

Diese Reihenfolge ist verbindlich: erst frontend-design, dann web-design-guidelines.

## Token-Effizienz (Caveman Mode)

Skill .claude/skills/caveman.md **immer aktiv** — komprimierte Kommunikation, ~75 % weniger Tokens, volle technische Präzision bleibt erhalten. Nur Füllwörter und Floskeln fallen weg. Deaktivieren mit "stop caveman" / "normal mode".
