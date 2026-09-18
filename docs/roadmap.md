# Roadmap

**Wichtig:** V2 und V3 sind Planungspunkte. **Alles baut auf dem V1-Agenten auf** —
die Architektur, die wir jetzt bauen, ist die Basis für beide Folgeprojekte.

## V1 — Translation Relay (aktuell)

Bidirektionaler Telefon-Dolmetscher, **ohne LLM**:
Mensch (bs) ↔ Soniox Translation ↔ Soniox TTS ↔ Kunde (de).
Kein Originalton, kein Doppelsprechen, LiveKit SIP.

- [x] Machbarkeit verifiziert, Projekt dokumentiert
- [ ] SDK-Umgebung, Relay-Node, SIP-Test (siehe Issues)

## V2 — Whisper-/Coach-Agent (geplant)

**Erweiterung von V1:** Ein Agent in der Kette, der dem menschlichen Sales-Agenten
**ins Ohr spricht** (Whisper/Coach-Kanal):

- Tipps während des Gesprächs (z. B. Einwandbehandlung, Closing-Signale)
- Infos auf Abruf (Kundendaten, Preise, Argumente)
- Training & Bewertung der Gespräche (Review, Scoring)

Technische Basis: dieselbe LiveKit-Room-Architektur wie V1; zusätzlicher Kanal,
der nur der Mensch-Seite hört. Hier wird (später) vermutlich doch ein LLM sinnvoll —
V1 ist bewusst LLM-frei, damit die Basis stabil und latenzarm bleibt.

## V3 — Rebuild bestehender OpenAI-Agent (geplant)

Der bereits gebaute Voice-Agent (ohne SDK/Plugin, manuelle HTTP-Anbindung,
Probleme mit Tool-Calling) wird **sauber mit LiveKit-LLM-Plugin/SDK neu gebaut**:
`llm=openai.LLM()`, `@function_tool`, sauberes Tool-Calling (z. B. Terminbuchung).

Nutzt die V1/V2-Infrastruktur (Telefonie, Turn-Taking, Soniox STT/TTS).

## Reihenfolge

V1 zuerst stabil bauen → V2 auf derselben Basis → V3.
Keine Parallelen, kein Kontextwechsel zwischendrin.
