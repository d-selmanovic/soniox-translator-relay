# Setup

## Benötigte Komponenten

| Komponente | Zweck | Status |
|---|---|---|
| LiveKit CLI (`brew install livekit-cli`) | Projektverwaltung, Tokens, Dev-Server | ✅ 2.18.6, Cloud verbunden |
| `livekit-agents[soniox,silero]~=1.5` | Agents-Framework + beide Soniox-Plugins + VAD | ⬜ offen |
| `python-dotenv` | Konfiguration | ⬜ offen |
| Soniox API-Key | STT-Translation + TTS (ein Key, beide Enden) | ⬜ nötig |
| LiveKit Cloud-Projekt (oder local server) | Room + SIP-Telefonie | ⬜ entscheiden |
| LiveKit-Docs-MCP | Plugin-Referenz direkt abfragbar | ⬜ offen |

## Empfohlene Installationsreihenfolge

```bash
# 1. Python-Umgebung
cd ~/dev/soniox-translator-relay
python3 -m venv .venv
source .venv/bin/activate
pip install "livekit-agents[soniox,silero]~=1.5" python-dotenv

# 2. API-Keys in .env (nicht committen!)
cp .env.example .env
# SONIOX_API_KEY=..., LIVEKIT_URL=..., LIVEKIT_API_KEY=..., LIVEKIT_API_SECRET=...

# 3. LiveKit-Docs-MCP anbinden (Befehl: docs.livekit.io/reference/developer-tools/docs-mcp.md)

# 4. Verifikation: AgentSession ohne LLM, Translation-Optionen im Plugin-Quellcode prüfen
```

## Telefonie-Optionen (später zu entscheiden)

1. **LiveKit Cloud SIP** — Phone Numbers API + SIP-Trunk direkt in der Cloud (einfachster Pfad)
2. **Asterisk als SIP-Frontend** — bestehende Infrastruktur (Voice-Ops-Console) per Ingress an LiveKit andocken

## Sicherheitshinweise

- `.env` liegt im `.gitignore` — API-Keys niemals committen
- Der `lk project list`-Output enthält API-Keys → nicht in Doku/Commits übernehmen

## Versions-Pinning (Wichtig)

`livekit-agents` pinn einige Abhängigkeiten **exakt** (u. a. `livekit==1.1.18`,
`json-repair==0.60.1`). Ein blindes `pip install --upgrade <paket>` bricht die
Konsistenz. Nach Updates immer prüfen:

```bash
.venv/bin/pip check        # muss "No broken requirements found" melden
```

Stand 2026-09-18: livekit-agents 1.8.2, livekit 1.1.18, gh 2.101.0, livekit-cli 2.18.7
