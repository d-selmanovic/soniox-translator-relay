# Session-Handoff

## Aktive Arbeits-Session (Kimi Code)

- **Session-ID:** `session_47f5eaa4-91ba-47ff-b681-4634a4f91629`
- **Speicherort:** `~/.kimi-code/sessions/wd_activi_91f10f4d1649/session_47f5eaa4-...`
- **Wiederaufsetzen:** Kimi Code in `/Users/activi` starten → Resume mit dieser Session-ID
  (Kontext: kompletter bisheriger Verlauf, inkl. aller Verifikationen)

## Stand des Projekts (2026-09-19)

V1 (Translation Relay ohne LLM) ist vorbereitet und verifiziert:
Projekt-Setup komplett (venv, SDKs auf neuestem Stand, AGENTS.md, Roadmap,
GitHub-Issues #1–#5). Code-Verifikation liegt vor (`llm=None` offiziell,
Translation im Soniox-Plugin, Relay-Mechanismus `user_input_transcribed` → `say()`).

## Nächste Schritte (Reihenfolge)

1. Soniox API-Key beschaffen und in `.env` eintragen → Issue #2 schließen
2. LiveKit-Docs-MCP anbinden (Anleitung in Issue #5) → schließen
3. Relay-Node implementieren (Issue #1), Test in console mode
4. LiveKit-Projekt für SIP festlegen + Telefonie-Test (Issue #3)
5. Stimmen: eigenen Klon + zweite Stimme (Issue #4)

## Verwandte Sessions/Orte

- Soniox Wiki (Obsidian): `~/AI Bibliothek/obsidian/Soniox Wiki/Soniox Wiki/Markdown/Translation/`
- Home-Verzeichnis ist **kein** Git-Repo mehr (`.git` → `.git.backup`,
  siehe verified-facts.md) — Agenten niemals mit cwd=Home starten
