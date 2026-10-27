# voice-agent

A real-time voice assistant called **Jarvis**, in the spirit of the AI from Iron Man. You talk, it talks back instantly: a classy, slightly sarcastic butler who acknowledges a task ("Will do, Sir", "Roger Boss") and answers in one short sentence.

## How it works

- **[LiveKit Agents](https://docs.livekit.io/agents/)** runs the session: audio in, audio out, over a LiveKit room.
- **Gemini Realtime** (`google.beta.realtime.RealtimeModel`, voice "Charon") is the brain and the voice in one model, so there is no separate speech-to-text and text-to-speech step and replies come back fast.
- **Background voice cancellation** (LiveKit's BVC) strips out background noise before the model hears you.
- The persona and the opening line live in [`prompts.py`](prompts.py), so the same agent can become a receptionist, a booking agent or a support line by changing the prompt.

| File | What it does |
|---|---|
| [`gemini.py`](gemini.py) | The agent: creates the session, joins the room and greets you |
| [`prompts.py`](prompts.py) | The persona (`AGENT_INSTRUCTION`) and the first thing it says (`SESSION_INSTRUCTION`) |

## Try it

You need Python 3.10+, a free [LiveKit Cloud](https://cloud.livekit.io) project and a [Gemini API key](https://aistudio.google.com/apikey).

```bash
pip install -r requirements.txt livekit-plugins-turn-detector
```

Create a `.env` file:

```
LIVEKIT_URL=wss://<your-project>.livekit.cloud
LIVEKIT_API_KEY=...
LIVEKIT_API_SECRET=...
GOOGLE_API_KEY=...
```

Download the model files once, then talk to it straight from your terminal:

```bash
python gemini.py download-files
python gemini.py console
```

To use it from a browser or phone instead, run `python gemini.py dev` and join the room from the [LiveKit Agents Playground](https://agents-playground.livekit.io).
