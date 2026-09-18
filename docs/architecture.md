# Architektur: Translation Relay (ohne LLM)

## Prinzip

Kein Agent, sondern ein **bidirektionaler Media-Relay**: zwei unabhängige
Übersetzungsketten in einem LiveKit-Room. Jede Seite hört nur TTS, nie Originalton.

```
                 LiveKit Room (SIP: Kunde ◄──► SIP/Softphone: Mensch)
                 beide Legs getrennt, NIEMALS direkt gebridged
                │                                        │
     Kette A (Mensch → Kunde)                Kette B (Kunde → Mensch)
                │                                        │
   soniox.STT (Translation bs→de)           soniox.STT (Translation de→bs)
                │                                        │
   Custom Relay-Node (KEIN LLM)             Custom Relay-Node (KEIN LLM)
   übersetzter Text → sofort TTS             übersetzter Text → sofort TTS
                │                                        │
   soniox.TTS (geklonte Stimme, de)         soniox.TTS (andere Stimme, bs)
                │                                        │
        nur an Kunden-Track                        nur an Mensch-Track
```

## Turn-Taking / Duplex-Steuerung

Aufgabenverteilung (lt. LiveKit/Soniox-Doku):

| Aufgabe | Komponente |
|---|---|
| Wann ist ein Satz zu Ende? (Turn-Ende) | Soniox STT Endpoint Detection (`turn_detection="stt"`, `max_endpoint_delay_ms=1000`) |
| Wann fängt die Gegenseite an? (Turn-Start / Unterbrechung) | Silero VAD (`interruption={"mode": "vad"}`) → bricht laufende TTS ab |

## Sprachen

- Soniox Speech-Translation: 3600+ Sprachpaare; **Bosnisch (`bs`) bestätigt**
- Soniox TTS: **Bosnisch (`bs`, lateinische Schrift) bestätigt**, Voice Cloning verfügbar
- Konfiguration als zwei one-way-Ketten (A: bs→de, B: de→bs), NICHT der
  `two_way`-Modus — der ist für ein Mikrofon (Face-to-face), nicht für zwei Telefon-Legs

## Latenzziel

~1–2 s Gesamtverzögerung (S2TT-Partials + Streaming-TTS, kein LLM-Umweg).

## Bekannte Baustellen (ehrlich)

- `AgentSession` ist LLM-zentriert; LLM-freier Betrieb = **Custom Node / eigene Logik**,
  nicht per Konfiguration (nicht dokumentierter Pfad, aber übliche Praxis)
- Edge-Cases selbst abfangen: Abbruch mitten im TTS-Satz, Partials vs. Finals,
  beide Richtungen gleichzeitig
- Echo: TTS der einen Kette darf nie in die Mikrofon-Route der anderen gelangen
  (durch getrennte SIP-Legs Architektur-vorgegeben, trotzdem testen)
