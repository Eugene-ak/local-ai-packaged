# Image Processing and Generation Chat Agent Setup

This workflow can:

- Describe uploaded images (vision mode)
- Generate images from text prompts (generation mode)

Workflow file:

- `n8n/backup/workflows/Image_Processing_and_Generation_Chat_Agent.json`

## 1. Import the Workflow

In n8n:

1. Open **Workflows**.
2. Choose **Import from File**.
3. Import `n8n/backup/workflows/Image_Processing_and_Generation_Chat_Agent.json`.

## 2. Model/Service Requirements

### A) Image description (local via Ollama)

Recommended model:

- `llava:13b`

Pull it:

```bash
docker exec -it ollama ollama pull llava:13b
```

Alternatives:

- `llava:7b`
- `llama3.2-vision`

### B) Image generation

This workflow uses an OpenAI-compatible image generation API by default.

Default endpoint in the workflow:

- `https://api.openai.com/v1/images/generations`

You can point to any compatible endpoint by passing in request body:

- `image_gen_url`
- `image_gen_api_key`
- `image_gen_model`

If you want fully local generation, run a local image server (for example ComfyUI API) and update `image_gen_url` accordingly.

## 3. How to Use

### Chat mode

Use the chat trigger and provide:

- `prompt`/`chatInput`
- Optional `mode`: `describe` or `generate`
- For `describe`, include image upload or `image_base64`

### Webhook mode

POST to:

- `/webhook/image-chat-agent`

Example (generate):

```json
{
  "mode": "generate",
  "prompt": "A cinematic futuristic city at sunset, ultra-detailed",
  "image_gen_api_key": "<YOUR_API_KEY>",
  "image_gen_model": "gpt-image-1",
  "image_size": "1024x1024"
}
```

Example (describe):

```json
{
  "mode": "describe",
  "prompt": "Describe this image in detail and list notable objects.",
  "image_base64": "<BASE64_IMAGE>",
  "vision_model": "llava:13b"
}
```

## 4. Notes

- If `mode` is omitted, the workflow auto-selects:
  - `describe` when an image is provided
  - `generate` otherwise
- For generation, response may include either `image_url` or `image_base64`, depending on provider response format.
