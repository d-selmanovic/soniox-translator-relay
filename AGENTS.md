# AGENTS.md — Projekt-Memory

Dauerhafte Projektanweisungen für alle Agenten (Kimi, opencode, Copilot, …), die in
diesem Repo arbeiten. Wird mit git versioniert.

## Projekt

Bidirektionaler Telefon-Dolmetscher (Mensch ↔ Kunde) auf Basis von
**LiveKit + Soniox**, bewusst **ohne LLM** (V1). Details: `docs/architecture.md`,
geprüfte Fakten: `docs/verified-facts.md`, Planung: `docs/roadmap.md`.

## Harte Regeln

1. **Kein LLM in der V1-Pipeline.** Keine `openai`-/`anthropic`-Abhängigkeit
   hinzufügen. LLM kommt frühestens in V2/V3 (siehe Roadmap).
2. **Ein Vendor für Sprache:** Soniox für STT-Translation UND TTS. Kein ElevenLabs,
   Cartesia, OpenAI-TTS o. ä. einbauen (Fehlerkorrektur: siehe verified-facts.md).
3. **Zwei one-way-Übersetzungsketten** (A: bs→de, B: de→bs), nicht den
   `two_way`-Modus (der ist für Face-to-face, nicht für zwei Telefon-Legs).
4. **Beide SIP-Legs niemals direkt bridgen** — jede Seite hört nur TTS, nie Originalton.
5. Keine API-Keys in Code oder Commits (`.env` liegt im `.gitignore`).
6. opencode/Agenten niemals mit cwd=`/Users/activi` (Home) starten — nur mit
   diesem Projektordner. (Home-Repo ist deaktiviert, siehe verified-facts.md.)

## Technische Leitplanken

- Sprachen: `bs` (Bosnisch, lateinisch) ↔ `de` (Deutsch)
- Turn-Ende: Soniox Endpoint Detection (`turn_detection="stt"`, `max_endpoint_delay_ms=1000`)
- Unterbrechung: Silero VAD (`interruption={"mode": "vad"}`)
- Voice Cloning für die Stimme des Menschen (bs→de-Richtung, deutsche Ausgabe)
- Framework: `livekit-agents[soniox,silero]~=1.5` (Python), venv im Projektordner

## Arbeitsweise

- Roadmap-Punkte V2/V3 sind Planung — nicht vor V1-Abschluss implementieren.
- Status-Änderungen in `README.md` pflegen.
- Fertig bedeutet: getestet (LiveKit console mode oder Simulation), nicht nur compiliert.
- Session-Kontext/Wiedereinstieg: `docs/session-handoff.md` (Session-ID,
  nächste Schritte, verwandte Orte wie das Soniox Wiki).
