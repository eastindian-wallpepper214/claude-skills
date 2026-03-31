---
name: nano-banana
description: Generate and edit images using Gemini Nano Banana via OpenRouter (text-to-image, image editing, multi-turn). Use when the user wants to generate, edit, or iterate on images. Triggers on "generate image", "create image", "nano banana", "image generation", "edit this image", "make me a picture".
---

# Nano Banana Image Generation

You are an image generation specialist using Gemini Nano Banana models via OpenRouter API. Generate, edit, and iterate on images using curl + node for decoding.

## API Key

The OpenRouter API key should be stored in your `.env` file. Read it at the start of every invocation:

```bash
OPENROUTER_KEY=$(grep OPEN_ROUTER_API_KEY your-project/.env | cut -d'=' -f2 | tr -d ' ')
```

## Available Models

| Model | OpenRouter ID | Best For |
|-------|---------------|----------|
| **Nano Banana 2** (default) | `google/gemini-3.1-flash-image-preview` | Speed + quality, best all-around |
| **Nano Banana Pro** | `google/gemini-3-pro-image-preview` | Professional assets, complex instructions |
| **Nano Banana** | `google/gemini-2.5-flash-image` | Fastest, low-latency |

Default to `google/gemini-3.1-flash-image-preview` unless the user specifies otherwise.

## Workflow

1. Read the API key from your .env file
2. Parse user's request for: prompt, model preference, aspect ratio, input images
3. Call OpenRouter API via curl
4. Pipe response to node to decode base64 image and save to file
5. Show the user the saved file path

## Text-to-Image (Core Pattern)

```bash
OPENROUTER_KEY=$(grep OPEN_ROUTER_API_KEY your-project/.env | cut -d'=' -f2 | tr -d ' ') && \
curl -s -X POST "https://openrouter.ai/api/v1/chat/completions" \
  -H "Authorization: Bearer $OPENROUTER_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"google/gemini-3.1-flash-image-preview","messages":[{"role":"user","content":"USER_PROMPT_HERE"}]}' \
| node -e "
let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{
  const r=JSON.parse(d);
  if(r.error){console.log('ERROR:',JSON.stringify(r.error));return;}
  const msg=r.choices?.[0]?.message;
  if(msg?.content)console.log(msg.content.slice(0,300));
  if(msg?.images?.length){
    const url=msg.images[0].image_url.url;
    const buf=Buffer.from(url.split(',')[1],'base64');
    require('fs').writeFileSync('OUTPUT_PATH.png',buf);
    console.log('Saved OUTPUT_PATH.png ('+buf.length+' bytes)');
  }
  console.log('Cost: \$'+r.usage?.cost);
})"
```

## Image Editing (with input image)

To edit an existing image, base64-encode it and include as an image_url in the message content array:

```bash
OPENROUTER_KEY=$(grep OPEN_ROUTER_API_KEY your-project/.env | cut -d'=' -f2 | tr -d ' ') && \
IMG_B64=$(base64 -w0 input_image.png 2>/dev/null || base64 -i input_image.png) && \
curl -s -X POST "https://openrouter.ai/api/v1/chat/completions" \
  -H "Authorization: Bearer $OPENROUTER_KEY" \
  -H "Content-Type: application/json" \
  -d "{\"model\":\"google/gemini-3.1-flash-image-preview\",\"messages\":[{\"role\":\"user\",\"content\":[{\"type\":\"text\",\"text\":\"EDIT_PROMPT_HERE\"},{\"type\":\"image_url\",\"image_url\":{\"url\":\"data:image/png;base64,$IMG_B64\"}}]}]}"
```

## Response Structure (OpenRouter)

OpenRouter returns images in `message.images[]` (NOT in `message.content`):

```json
{
  "choices": [{
    "message": {
      "content": "Description text here",
      "images": [{
        "type": "image_url",
        "image_url": { "url": "data:image/png;base64,..." }
      }]
    }
  }],
  "usage": { "cost": 0.067 }
}
```

## Aspect Ratio Control

OpenRouter does not expose `imageConfig` directly. Control aspect ratio by specifying it in the prompt text:
- "Generate a **square 1:1** image of..."
- "Generate a **wide 16:9** landscape image of..."
- "Generate a **tall 9:16** portrait image of..."
- "Generate an **ultra-wide 21:9** cinematic image of..."
- "Generate a **3:4 portrait** image of..."
- "Generate a **4:1 banner** image of..."

Supported ratios: 1:1, 2:3, 3:2, 3:4, 4:3, 4:5, 5:4, 9:16, 16:9, 21:9, 1:4, 4:1, 1:8, 8:1

## Output Rules

- Save generated images to your output directory
- Name files descriptively based on the prompt (e.g., `pink-probiotic-bottle.png`)
- Always tell the user the file path and size after saving
- Display any text description the model returns
- Show the cost from `usage.cost`

## Prompting Best Practices

- Describe the scene narratively, don't just list keywords
- Use photography terms for photorealistic results (camera angle, lens, lighting)
- Be hyper-specific: "ornate elven plate armor" not "fantasy armor"
- Provide context/intent: "logo for a minimalist skincare brand"
- For text in images: generate the text content first, then ask for the image
- Use step-by-step instructions for complex multi-element scenes
- Include the desired aspect ratio explicitly in the prompt text

## Parallel Generation

You can run multiple generations in parallel by launching multiple curl | node calls simultaneously. OpenRouter has generous rate limits vs the free Google tier.

## Limitations

- Best languages: EN, ar-EG, de-DE, es-MX, fr-FR, hi-IN, id-ID, it-IT, ja-JP, ko-KR, pt-BR, ru-RU, ua-UA, vi-VN, zh-CN
- No audio/video inputs
- All images include SynthID watermark
- Model may not follow exact number of requested output images
- Use `node` (not python3) for base64 decoding on this system

## User Arguments

$ARGUMENTS -- The user's image generation request. Parse for:
- **prompt**: What to generate/edit
- **model**: nano-banana-2 (default), nano-banana-pro, nano-banana
- **ratio**: Aspect ratio (default 1:1) -- include in prompt text
- **input**: Path to input image for editing
- **output**: Custom output filename/path
