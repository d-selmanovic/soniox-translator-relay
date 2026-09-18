# Soniox Translator Relay

Bidirektionaler Telefon-Dolmetscher: Mensch ↔ Kunde, **ohne LLM**.

Repo: https://github.com/d-selmanovic/soniox-translator-relay
Tracking: GitHub Issues (Labels: v1 / core / telephony / voice / setup)

Mensch (Bosnisch) spricht ins Telefon → Soniox Speech-to-Text-Translation (bs→de) →
Soniox TTS mit geklonter Stimme → Kunde hört Deutsch.
Kunde (Deutsch) → Soniox Translation (de→bs) → Soniox TTS (andere Stimme) → Mensch hört Bosnisch.

## Kernanforderungen

- **Kein LLM** in der Pipeline — nur STT-Translation + TTS
- **Kein Originalton**: jede Seite hört ausschließlich den übersetzten TTS-Stream
- **Kein Doppelsprechen**: laufende TTS wird unterbrochen, wenn die Gegenseite anfängt
- Telefonie über **LiveKit SIP**; beide Legs laufen getrennt in einen LiveKit-Room (nie direkt gebridged)

## Status

- [x] Machbarkeit verifiziert (Soniox-Doku, LiveKit-Doku, installierte SDKs)
- [x] LiveKit-CLI verbunden (Cloud-Projekte vorhanden)
- [ ] SDK-Umgebung (venv) einrichten
- [ ] LiveKit-Docs-MCP anbinden
- [ ] Minimal-Relay (STT→TTS ohne LLM) implementieren
- [ ] SIP-Telefonie-Anbindung testen
- [ ] Stimmenklon einrichten (eigene Stimme für bs→de-Richtung)

## Dokumentation

- [`docs/architecture.md`](docs/architecture.md) — Zielarchitektur und Datenfluss
- [`docs/verified-facts.md`](docs/verified-facts.md) — geprüfte Fakten inkl. Fehlerkorrekturen
- [`docs/setup.md`](docs/setup.md) — Installations- und Einrichtungsschritte
