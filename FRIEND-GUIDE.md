# Gateway friend guide

This is for a small Windows beta. You run your own Gateway. You use your own keys, or a model on your own PC. Do not share keys. The app does not collect usage data.

The download page is https://iiaaronp99.github.io/gateway-releases/.

## Requirements

- Windows.
- [Docker Desktop](https://www.docker.com/products/docker-desktop/), installed and open. Wait until it says it is running.

Ollama is optional. It is the free model that runs on your PC. Cloud chat works with your own Gemini key and does not need Ollama.

## Installing Gateway

1. Open https://github.com/iiaaronp99/gateway-releases/releases/latest
2. Click the file whose name ends in `-setup.exe`.
3. Windows may say "Windows protected your PC". The app is not signed, because a certificate costs money and this beta stays free. Click **More info**, then **Run anyway**.
4. Finish the installer and open Gateway.

The installer is the chat window. It does not start the models by itself. The next section does that.

## Starting the gateway

The model server is a zip, because the app project is private. The zip has no secrets in it.

1. Download the gateway bundle: https://github.com/iiaaronp99/gateway-releases/raw/main/gateway-bundle.zip
2. In File Explorer, open the zip and click **Extract all**. Remember that folder. It contains `docker-compose.yml`.
3. Open `HOW-TO-START.txt` in that folder and follow it. The short version:
   - Turn on file name extensions (File Explorer, View, Show, File name extensions).
   - Copy `.env.example` and rename the copy to `.env`.
   - Right-click `.env`, Open with, Notepad. Paste your own keys. Save.
   - Double-click `Start-Gateway.bat`. The first time can take a few minutes.
4. Go back to the Gateway app. If it says the gateway is not running, wait a minute and try again.

The app may say to run a command in "the gateway folder". For this beta, that folder is the unzipped bundle.

## Adding your own keys or Ollama

Each friend uses a separate account. Do not put someone else's key in your `.env`, and do not send your key in chat.

In Notepad, in the `.env` file next to `docker-compose.yml`:

| Line | What to paste |
| --- | --- |
| `GEMINI_API_KEY` | Your key from [Google AI Studio](https://aistudio.google.com/apikey). The free tier is enough. |
| `OPENROUTER_API_KEY` | Optional. Your key from [OpenRouter](https://openrouter.ai/keys). Free models on your own account are enough. |
| `LITELLM_MASTER_KEY` | A password you make up. It should start with `sk-`. This is the Gateway key other apps will ask for. It is not a provider key. |

No quotes, and no spaces around `=`. Save the file, then double-click `Start-Gateway.bat` again.

Ollama, if you want a model on this PC:

1. Install [Ollama](https://ollama.com/download) and open it.
2. Click the address bar of the bundle folder, type `powershell`, and press Enter.
3. Run `ollama pull llama3.2` for local chat (`t0`).
4. Run `ollama pull nomic-embed-text` only if an app needs embeddings. The model name in Gateway is `embed`.

More companies can be turned on later. `docs/add-a-free-provider.md` inside the zip says how. Some of those are not free. Read the note before you uncomment one. Do not add a card if you want to stay at $0.

In the Gateway app, the usual model name is `auto`. `t0` is the PC model. `t1` is the free cloud pool.

## Connecting other tools

Other apps on this PC can use Gateway as an OpenAI-compatible server.

- Base URL: `http://localhost:4000/v1`
- Key: your Gateway key, the `LITELLM_MASTER_KEY` value. Not a Gemini or OpenAI key.
- A good first model name: `auto`

The full click paths for Cursor, Continue, Cline, Open WebUI, Aider, LangChain, and a generic OpenAI SDK are in the connect guide: https://github.com/iiaaronp99/gateway-releases/blob/main/connect-other-apps.md

One limit to know before you try Cursor: Cursor's servers make the request, so `http://localhost:4000/v1` does not work there. The connect guide says what is true and what a free tunnel would mean. The other apps in that list run on your PC and can use localhost.

There is an optional check in the bundle folder, `compat-smoke.py`. It needs Python and a running gateway. Friends can skip it. The app's own test is enough.

## Getting Odyssey Journal

Odyssey Journal is Aaron's other app. It is not Gateway.

- Download page: https://iiaaronp99.github.io/odyssey-releases/
- Latest installer list: https://github.com/iiaaronp99/odyssey-releases/releases/latest

## Connecting Odyssey to Gateway

Odyssey talks to your Gateway. Your Gateway key stays yours. There is no shared key.

On first run, Odyssey asks you to pick a provider. There is no default.

1. Open Odyssey Journal.
2. Open the Settings gear.
3. Open **AI Provider**.
4. Set **Provider** to the **AI Gateway** preset.
5. Check that **Base URL** is `http://localhost:4000/v1`. It is filled in for you. If it is empty, paste that address.
6. Paste your Gateway key into **API key**. That is the `LITELLM_MASTER_KEY` value from your `.env`.
7. Pick a **Model**. `auto` is a good first choice.
8. Click **Test connection**.

Gateway has to be running first (see Starting the gateway). These setting names match Odyssey's plan. If a label on your build is different, use the AI Gateway preset, the base URL above, and your Gateway key.

## How to report bugs and feedback

Gateway bugs and ideas go here: https://github.com/iiaaronp99/gateway-releases/issues/new/choose

Pick **Bug report** or **Feedback or idea**. Do not paste API keys or your `.env` file.

Odyssey problems do not go there. Use Odyssey's issues: https://github.com/iiaaronp99/odyssey-releases/issues
