# Connect other apps to Gateway

Gateway speaks the OpenAI HTTP shape on your own computer.

- Base URL for OpenAI-style apps: `http://localhost:4000/v1`
- Key: your Gateway password. Open Settings in the Gateway app and press Copy. It is not a Gemini, OpenAI, or Anthropic key. An app on this computer can also ask Gateway for its own key. See [register-an-app.md](register-an-app.md).
- A good first model name: `auto`. `t0` is the model on this PC (Ollama). `t1` and `smart` are the free cloud pool. `t2` is the stronger Gemini model.

Each app below says which of those to paste, then a test. The test message is: Reply with the single word: pong. A working reply contains pong.

These routes were checked against the router in this project and against the LiteLLM v1.109.1 proxy source (the image the gateway runs). A live answer still depends on your own key and on the model you picked. Nothing here calls a paid API by itself.

What other apps can call:

| Call | What Gateway does |
| --- | --- |
| `GET /v1/models` | Returns the model list and adds `auto`. |
| `POST /v1/chat/completions` | Chat, including streaming (`stream: true`). |
| Tools | `tools` and `tool_choice` are forwarded as you sent them. The model still has to support tools. Gemini on `t1` is the better trial. `t0` (llama3.2) may ignore tools. |
| `POST /v1/embeddings` | Forwarded. The free model name is `embed`. Run `ollama pull nomic-embed-text` first. This is not OpenAI's `text-embedding-3-small`. Gemini's embedding model has a list price and is left off. |
| `POST /v1/messages` | Anthropic-style messages. LiteLLM v1.109.1 has this route and translates it onto the model you name. Clients that add `/v1/messages` themselves should use the base URL `http://localhost:4000` (no extra `/v1`). `max_tokens` is required by that shape. The Gateway key is the credential. |

Continue, Cline, and Open WebUI run on this PC, so try those first. They can use `http://localhost:4000/v1`. Cursor needs a public HTTPS address, so it comes after those. Aider, LangChain, and the OpenAI SDK also run on this PC.

## Continue

- Base URL: `http://localhost:4000/v1`
- Key: your Gateway password. Open Settings and press Copy.
- Test: open a Continue chat, pick Gateway, and send the pong message.

Steps:

1. Install Continue in VS Code.
2. Add a chat model and choose the OpenAI provider.
3. API base: `http://localhost:4000/v1`
4. API key: the Gateway password you copied from Settings.
5. Model: `auto`

The same values in Continue's config file (`config.yaml`):

```yaml
name: Gateway
version: 1.0.0
schema: v1
models:
  - name: Gateway
    provider: openai
    model: auto
    apiKey: YOUR_GATEWAY_KEY
    apiBase: http://localhost:4000/v1
    roles:
      - chat
```

## Cline

- Base URL: `http://localhost:4000/v1`
- Key: your Gateway password. Open Settings and press Copy.
- Test: send the pong message as a Cline task.

Steps:

1. Open Cline.
2. API Provider: OpenAI Compatible.
3. Base URL: `http://localhost:4000/v1`
4. API Key: the Gateway password you copied from Settings.
5. Model: `auto`

## Open WebUI

- Base URL: `http://localhost:4000/v1` when Open WebUI is running as a Windows app.
- Key: your Gateway password. Open Settings and press Copy.
- Test: start a chat, pick `auto`, and send the pong message.

Steps:

1. Open Admin Panel, then Settings, then Connections, then OpenAI.
2. API URL: `http://localhost:4000/v1`
3. API key: the Gateway password you copied.
4. Save, then refresh the model list.

Limit: if Open WebUI itself is running inside Docker, `localhost` means that container, not your PC. Use `http://host.docker.internal:4000/v1` on Docker Desktop for Windows.

## Cursor

- Base URL: a public `https://.../v1` address, not localhost. See the limit below.
- Key: your Gateway password. Open Settings and press Copy, then paste it into Cursor's OpenAI API key field.
- Test: click Verify, then send the pong message with your added model selected.

Steps:

1. Open Cursor Settings, then Models.
2. Turn on OpenAI API Key and paste the Gateway password you copied.
3. Add a model whose name is exactly `auto` (or `t1`, `smart`, or `t0`).
4. If a label has moved, the three values that matter are still that key, the base URL, and the model name.

Limit, checked against Cursor staff replies on the Cursor forum in 2026: Override OpenAI Base URL is called from Cursor's servers, so it needs a public HTTPS URL. `http://localhost:4000/v1` points at Cursor's machines, not yours, so it does not work. A home-network address fails for the same reason. Tab autocomplete still uses Cursor's own model. Your prompt still passes through Cursor's servers. Turning the OpenAI key override on can stop Cursor's own OpenAI-family models until you turn it off.

A free tunnel is optional, and only for Cursor. Cloudflare's quick tunnel does not need an account:

```
cloudflared tunnel --url http://localhost:4000
```

Paste the `https://` address it prints, with `/v1` on the end, into Override OpenAI Base URL. That address is public. Anyone who has it and your Gateway password can use your gateway until you close the tunnel window. Do not post the address. Do not buy a tunnel service for this beta.

## Aider

- Base URL: `http://localhost:4000/v1`
- Key: your Gateway password. Open Settings and press Copy.
- Test: ask Aider to reply with the single word pong.

Aider wants the `openai/` prefix on the model. It sends `auto` to Gateway.

```
aider --model openai/auto --openai-api-base http://localhost:4000/v1 --openai-api-key YOUR_GATEWAY_KEY
```

Or a `.aider.conf.yml` file in the project:

```yaml
model: openai/auto
openai-api-base: http://localhost:4000/v1
openai-api-key: YOUR_GATEWAY_KEY
```

## LangChain (Python)

- Base URL: `http://localhost:4000/v1`
- Key: your Gateway password. Open Settings and press Copy.
- Test: run the script. The printed line should contain pong.

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="auto",
    base_url="http://localhost:4000/v1",
    api_key="YOUR_GATEWAY_KEY",
)
print(llm.invoke("Reply with the single word: pong").content)
```

Install the library with `pip install langchain-openai`. That package is free. The call uses your Gateway, and your Gateway uses your own key.

## Generic OpenAI SDK

- Base URL: `http://localhost:4000/v1`
- Key: your Gateway password. Open Settings and press Copy.
- Test: run the script. The model list should include `auto`, and the chat line should contain pong.

```python
from openai import OpenAI

client = OpenAI(base_url="http://localhost:4000/v1", api_key="YOUR_GATEWAY_KEY")
print([item.id for item in client.models.list().data])
reply = client.chat.completions.create(
    model="auto",
    messages=[{"role": "user", "content": "Reply with the single word: pong"}],
)
print(reply.choices[0].message.content)
```

Embeddings, after `ollama pull nomic-embed-text`:

```python
embedded = client.embeddings.create(model="embed", input="hello")
print(len(embedded.data[0].embedding))
```

A streaming chat uses `stream=True` on `chat.completions.create`. Tool calls are a normal `tools` list on that same method. Gateway forwards them.

Anthropic-style clients (the Messages API) use base URL `http://localhost:4000` and the same Gateway password. They call `/v1/messages` themselves.
