# Verifizierte Fakten (Stand 2026-09-18)

Alles hier wurde gegen die offiziellen Dokus geprüft — nicht aus dem Gedächtnis.

## Soniox-Seite (soniox.com/docs)

1. **Speech-to-Text-Translation (S2TT) existiert als eigene API.**
   Audio rein → übersetzter Text direkt raus, mit Echtzeit-Partials.
   `/translation/stt-translation`
2. **Bosnisch (`bs`) ist unterstützt** — in Translation (3600+ Paare) und TTS
   (lateinische Schrift). `/translation/supported-languages`, `/tts/concepts/supported-languages`
3. **Voice Cloning** ist im TTS verfügbar (Referenzclip → eigene Voice-ID).
   `/tts/concepts/voice-cloning`
4. **Twilio-Integration** offiziell dokumentiert (Telefon-Audio per WebSocket).
   `/integrations/twilio`
5. **`two_way`-Translationsmodus ist für EIN Mikrofon** (Face-to-face), nicht für
   zwei getrennte Telefon-Legs → wir bauen zwei one-way-Ketten. `/translation/stt-translation`

## LiveKit-Integration (Soniox-Doku)

6. **`livekit.plugins.soniox` umfasst STT UND TTS.** Beide dokumentiert:
   `/integrations/livekit/stt`, `/integrations/livekit/tts`
   - TTS: `soniox.TTS(model="tts-rt-v2", voice=..., language=...)`, Streaming,
     EU-Endpoint (`wss://tts-rt.eu.soniox.com/tts-websocket`), `update_options()` zur Laufzeit
7. **Turn-Aufteilung:** Soniox STT = Turn-ENDE (Endpoint Detection), Silero VAD =
   Turn-START/Interruption (bricht TTS ab). `/integrations/livekit/voice-agent`
8. **Tuning:** `turn_detection="stt"`, `interruption={"mode": "vad"}`,
   `max_endpoint_delay_ms=1000` (sonst Desync-Warnungen).
9. **KEIN dokumentierter LLM-freier Pfad.** `AgentSession` = VAD→STT→LLM→TTS;
   LLM weglassen = Custom Node/Eigenbau auf Framework-Ebene. Möglich, aber nicht
   offiziell dokumentiert.

## Fehlerkorrektur: Soniox-Doku-KI (Support-Chat)

Die Support-KI des Soniox-Doku-Systems behauptete:

> ❌ „Ein Soniox-TTS-Plugin ist nicht dokumentiert. Nimm ElevenLabs, Cartesia oder OpenAI TTS."

**Falsch.** `/integrations/livekit/tts` dokumentiert `soniox.TTS` vollständig.
Das Plugin wird im LiveKit-Agents-Monorepo gepflegt
(`livekit-plugins/livekit-plugins-soniox`) — die KI hat vermutlich nur auf
Soniox-Seite gesucht. Konsequenz: Ein Vendor für beide Enden, Bosnisch nativ,
eigene Stimme klonbar. Externer TTS war unnötig und wäre für bs schlechter gewesen.

## Code-Verifikation (installierte SDKs, livekit-agents 1.8.2)

10. **`AgentSession` akzeptiert `llm=None`** — im Quellcode bestätigt
    (`agent_session.py`: `self._llm = ... (llm or None)`). LLM-freier Betrieb
    ist im Framework vorgesehen, nicht ein Hack.
11. **Soniox-STT-Plugin hat Translation nativ:** `STTOptions.translation` mit
    `TranslationConfig(type="one_way", target_language=...)` — im Plugin-Quellcode
    (`stt.py`) vorhanden und wird in die WebSocket-Config serialisiert.
12. **Relay-Mechanismus vorhanden:** Session emittiert `user_input_transcribed`
    (übersetzte Finals) und `session.say(text)` spricht TTS. Damit lässt sich
    STT→TTS-Durchleitung ohne LLM direkt auf Session-Ebene bauen.
13. Installiert: `livekit-agents 1.8.2`, `livekit-plugins-soniox 1.8.2`,
    `livekit-plugins-silero 1.8.2` (venv im Projektordner).

## Umgebung

- LiveKit-CLI 2.18.6 installiert, mit LiveKit Cloud verbunden
  (Projekte u. a. `aai`, `activi`, `energy`; Default: `aai`)
- Soniox-Docs-MCP angebunden; LiveKit-Docs-MCP noch offen

## Historische Kontextnotiz: Home-Verzeichnis

Das Home-Verzeichnis `/Users/activi` war versehentlich ein Git-Repo (403 MB),
dessen permanente Scans durch Devin/opencode den Mac thermisch belasteten.
`.git` wurde nach `~/.git.backup` verschoben (reversibel via
`mv ~/.git.backup ~/.git`). opencode darf nicht mehr mit cwd=Home gestartet werden.
