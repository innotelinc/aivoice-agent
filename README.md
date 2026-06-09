# AIVoice Agent

A local-first voice assistant built with LiveKit, Kyutai speech models, and Ollama.

## Features
- Fully local STT, TTS, and LLM runtime
- LiveKit-based realtime voice sessions
- Kyutai STT on `localhost:8080`
- Kyutai TTS on `localhost:8000`
- Ollama-backed LLM (`gemma3n:latest` by default)
- English-first assistant with optional French support

## Architecture
- **LiveKit** handles the voice session and agent runtime
- **Kyutai STT** provides OpenAI-compatible transcription
- **Ollama** provides the local LLM
- **Kyutai TTS** provides OpenAI-compatible speech synthesis

## Requirements
- Python 3.12
- A LiveKit server
- Kyutai STT service reachable at `http://localhost:8080/v1`
- Kyutai TTS service reachable at `http://localhost:8000/v1`
- Ollama with `gemma3n:latest`

## Quick start

### 1. Clone and create the venv
```bash
git clone https://github.com/innotelinc/aivoice-agent.git
cd aivoice-agent
uv venv --python /usr/bin/python3 .venv
source .venv/bin/activate
uv pip install -U pip setuptools wheel
uv pip install -e .
```

### 2. Create `.env.local`
Copy the included example:

```bash
cp .env.local.example .env.local
```

Then set:

```env
LIVEKIT_URL=ws://127.0.0.1:7880
LIVEKIT_API_KEY=your_livekit_api_key
LIVEKIT_API_SECRET=your_livekit_api_secret
```

### 3. Start dependencies
You need these services running before starting the agent:

- LiveKit server
- Kyutai STT on `127.0.0.1:8080`
- Kyutai TTS on `127.0.0.1:8000`
- Ollama API on `127.0.0.1:11434`
- Ollama model `gemma3n:latest`

### 4. Run the agent
```bash
source .venv/bin/activate
python agent.py dev
```

For interactive local console testing:

```bash
python agent.py console --text --log-level debug
```

## Verified local endpoints
A working local stack should return:

```bash
curl http://127.0.0.1:7880/
curl http://127.0.0.1:8080/health
curl http://127.0.0.1:8000/health
curl http://127.0.0.1:11434/api/tags
```

## Notes
- `.env.local` is intentionally gitignored
- `.env.local.example` is the safe template to commit
- This repo uses current LiveKit OpenAI-compatible TTS wiring via `openai.TTS(...)`
- Docker is optional if you run the Kyutai and Ollama stack rootlessly

## Repo contents
- `agent.py` — main LiveKit voice agent
- `.env.local.example` — environment template
- `pyproject.toml` — install metadata and dependencies

## License
MIT
