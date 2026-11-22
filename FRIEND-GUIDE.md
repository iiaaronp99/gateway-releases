# Gateway friend guide

This is for a small Windows beta. You run your own Gateway. You use your own keys, or a model on your own PC. Do not share keys. The app does not collect usage data.

The download page is https://iiaaronp99.github.io/gateway-releases/.

## Requirements

- Windows.

You do not install Docker. Gateway starts its own server when you open the app. You do not edit a file or make up a password.

Ollama is optional. It is a model that runs on your PC. If it is already open, Gateway notices it. Cloud chat uses your own OpenRouter account.

## Installing Gateway

1. Download the installer: https://github.com/iiaaronp99/gateway-releases/releases/latest/download/Ape1ron-Gateway-Setup.exe
   The file is named `Ape1ron-Gateway-Setup.exe`. This link always gets the newest version.
2. Open that file. Windows may say "Windows protected your PC". The app is not signed, because a certificate costs money and this beta stays free. Click **More info**, then **Run anyway**.
3. Finish the installer and open Gateway.

## Starting the gateway

1. Open Gateway. It creates a password and stores it on this PC. You never see that password.
2. Click **Sign in with OpenRouter**. Your browser opens. Sign in there, and allow the key.
3. Come back to Gateway. Chat is open. The model is `auto`.

If you already have a key, choose **I already have a key**, paste it, and click **Save key**, then **Start chatting**.

Before a chat that uses a free model, Gateway tells you that those providers may use what you type for training. The usual `auto` route asks OpenRouter to avoid providers that train on prompts.

If the window says Gateway is still starting, wait a few seconds. If it stays, quit the app and open it again.

## Adding your own keys or Ollama

Each friend uses a separate account. Do not put someone else's key in the app, and do not send your key in chat.

The usual way is **Sign in with OpenRouter** on the first screen. **I already have a key** saves a pasted key into Gateway. You do not open a `.env` file.

The lines below are only for someone running the optional Docker setup, not for the normal app:

| Line | What to paste |
| --- | --- |
| `GEMINI_API_KEY` | Your key from [Google AI Studio](https://aistudio.google.com/apikey). The free tier is enough. |
| `OPENROUTER_API_KEY` | Optional. Your key from [OpenRouter](https://openrouter.ai/keys). Free models on your own account are enough. |
| `LITELLM_MASTER_KEY` | A password you make up. It should start with `sk-`. This is the Gateway key other apps will ask for. It is not a provider key. |

No quotes, and no spaces around `=`. Save the file, then start that Docker setup again. The normal app does not use this file.

Ollama, if you want a model on this PC:

1. Install [Ollama](https://ollama.com/download) and open it.
2. Open PowerShell and run `ollama pull llama3.2` for local chat (`t0`).
3. Run `ollama pull nomic-embed-text` only if an app needs embeddings. The model name in Gateway is `embed`.

More companies can be turned on in the Docker setup. `docs/add-a-free-provider.md` says how. Some of those are not free. Read the note before you uncomment one. Do not add a card if you want to stay at $0.

In the Gateway app, the usual model name is `auto`. `t0` is the PC model. `t1` is the free cloud pool.

## Connecting other tools

Other apps on this PC can use Gateway as an OpenAI-compatible server.

- Base URL: `http://localhost:4000/v1`
- Key: a key for that app. An app on this PC can ask Gateway for one. The Gateway password also works, and it stays in the password store. It is not a Gemini or OpenAI key.
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

Odyssey can also ask your Gateway for its own key, so it does not have to share the password you use in the chat window. That only works from this computer. The short note for app authors is `docs/register-an-app.md` inside the project. Friends can skip that file and paste the Gateway password as below.

1. Open Gateway first, so it is running.
2. Open Odyssey Journal.
3. Open the Settings gear.
4. Open **AI Provider**.
5. Set **Provider** to the **AI Gateway** preset.
6. Check that **Base URL** is `http://localhost:4000/v1`. It is filled in for you. If it is empty, paste that address.
7. Odyssey asks Gateway for its own key. You do not paste the Gateway password.
8. Pick a **Model**. `auto` is the one to use.
9. Click **Test connection**.

These setting names match Odyssey's plan. If a label on your build is different, use the AI Gateway preset and the base URL above. The model name is `auto`.

## How to report bugs and feedback

Gateway bugs and ideas go here: https://github.com/iiaaronp99/gateway-releases/issues/new/choose

Pick **Bug report** or **Feedback or idea**. Do not paste API keys or your `.env` file.

Odyssey problems do not go there. Use Odyssey's issues: https://github.com/iiaaronp99/odyssey-releases/issues
