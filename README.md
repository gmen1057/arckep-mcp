# Arckép MCP Server (`arckep.ru`)

Official remote [Model Context Protocol (MCP)](https://modelcontextprotocol.io) server for **[Arckép](https://arckep.ru)** (`Arckep`) — multimodal AI studio & API for generating images, video, cinema effects, speech & voice cloning, transcriptions, 3D models, PPTX/PDF slide decks, Word/Excel/PDF documents, marketplace product cards, and LLM completions from a single pay-as-you-go RUB balance.

- **Endpoint:** `https://arckep.ru/api/v1/mcp`
- **Transport:** MCP Streamable HTTP
- **Authentication:** OAuth 2.1 (browser consent flow) or `Authorization: Bearer ark_live_...` API key
- **Registry Name:** `ru.arckep/mcp`
- **Developer Portal & Pricing:** [https://arckep.ru/developer](https://arckep.ru/developer) · [MCP Guide](https://arckep.ru/developer/docs#mcp)

---

## Quick Connect

### Option 1 — One-Click OAuth 2.1 (Recommended for Desktop & IDE Agents)

Any MCP client supporting Streamable HTTP + OAuth 2.1 discovery only needs the server URL:

```text
https://arckep.ru/api/v1/mcp
```

Your client automatically discovers OAuth metadata, opens the Arckép consent screen in your browser, and creates a dedicated revocable key (`MCP: <client name>`) under **Settings → API**. Approving OAuth never triggers a paid generation by itself, and you can revoke access anytime at [https://arckep.ru/settings?tab=api](https://arckep.ru/settings?tab=api).

#### Claude Code

```bash
claude mcp add --transport http arckep https://arckep.ru/api/v1/mcp
```

#### VS Code (`.vscode/mcp.json`)

```json
{
  "servers": {
    "arckep": {
      "type": "http",
      "url": "https://arckep.ru/api/v1/mcp"
    }
  }
}
```

#### Cursor (`.cursor/mcp.json`)

```json
{
  "mcpServers": {
    "arckep": {
      "url": "https://arckep.ru/api/v1/mcp"
    }
  }
}
```

---

### Option 2 — API Key (`Bearer`) for Headless Agents & CI

Create a scoped API key in **[Settings → API](https://arckep.ru/settings?tab=api)** and pass it in the `Authorization` header (store the key in your client's secret store or environment variables, never in committed config files):

```bash
claude mcp add --transport http arckep https://arckep.ru/api/v1/mcp \
  --header "Authorization: Bearer ${ARCKEP_API_KEY}"
```

```json
{
  "mcpServers": {
    "arckep": {
      "url": "https://arckep.ru/api/v1/mcp",
      "headers": {
        "Authorization": "Bearer ${env:ARCKEP_API_KEY}"
      }
    }
  }
}
```

---

## Capabilities & Tool Families

| Category | Tools | Highlights |
| --- | --- | --- |
| **Catalog, Balance & Usage** | `list_models`, `get_balance`, `get_usage` | Live model catalog with capabilities, parameters, RUB pricing, current balance, and spending breakdown |
| **Images** | `estimate_image`, `create_image`, `list_images`, `get_image` | Text-to-image & image editing with reference photos, aspect ratios, up to 4K resolution |
| **Video & Cinema** | `estimate_video`, `create_video`, `list_videos`, `get_video`, `*_higgsfield_*`, `cancel_higgsfield_video` | Text/image/reference-to-video, native audio, cinema camera presets, motion transfer, object swap, style restyle |
| **Speech (TTS) & Voice Cloning** | `estimate_speech`, `create_speech`, `list_speech`, `get_speech`, `list_speech_voices`, `list_my_speech_voices`, `design_speech_voice`, `replicate_speech_voice`, `delete_speech_voice` | Single-voice & two-speaker dialogue WAV synthesis, custom voice design from text description, consent-verified voice cloning |
| **Speech-to-Text (STT)** | `estimate_transcription`, `create_transcription` | Audio transcription with speaker diarization and timestamps |
| **3D Models** | `estimate_3d`, `create_3d`, `list_3d`, `get_3d`, `download_3d_stl` | Text/image-to-3D generation (`glb`, `fbx`, `obj`, `usdz`) + printable binary `STL` export |
| **Slide Decks (PPTX / PDF)** | `estimate_presentation`, `create_presentation`, `list_presentations`, `get_presentation`, `revise_presentation`, `cancel_presentation` | Multi-slide presentation generation and iterative slide revision with PPTX, PDF, and slide previews |
| **Documents (DOCX / XLSX / PDF)** | `estimate_document`, `create_document`, `list_documents`, `get_document`, `revise_document`, `cancel_document` | Structured Word reports, Excel spreadsheets with formulas, and PDFs with iterative revisions |
| **Marketplace Cards** | `estimate_marketplace_cards`, `create_marketplace_cards`, `list_marketplace_cards`, `get_marketplace_cards` | Complete e-commerce infographic card packs for Wildberries & Ozon from product photos |
| **Media Upload** | `upload_media` | Upload reference images, audio, video, or source documents (up to 10 MiB over MCP; up to 25 MB over HTTP REST) |
| **Chat, Decisions & Batch** | `chat_completion`, `decide`, `create_chat_batch`, `list_chat_batches`, `get_chat_batch`, `get_chat_batch_results`, `cancel_chat_batch` | Multimodal LLM calls, multi-model consensus decisions, and asynchronous batch processing |

### Built-in Cost & Safety Controls

- **Free price quotes first:** Every paid tool has a free `estimate_*` counterpart returning exact or typical RUB cost before execution.
- **Spend caps (`max_cost_rub`):** Paid media creation tools accept `max_cost_rub` and refuse execution before charging if the quote exceeds your cap.
- **Idempotency (`idempotency_key`):** Safe retries without double charges.
- **Scoped API keys:** Restrict keys to specific permissions (`images:write`, `videos:write`, `audio:write`, `presentations:write`, `documents:write`, `models3d:write`, `chat:write`, `read`).

---

## На русском — быстрое подключение к Arckép MCP

Публичный MCP-сервер **[Arckép](https://arckep.ru)** даёт ИИ-агентам (Claude Code, Claude Desktop, Cursor, VS Code и др.) прямой доступ к генерации изображений, видео, озвучки и клонированию голоса, расшифровке аудио, 3D-моделям, презентациям PPTX/PDF, документам Word/Excel/PDF, карточкам для Wildberries и Ozon и языковым моделям с единого рублёвого баланса без VPN.

- **Адрес MCP:** `https://arckep.ru/api/v1/mcp`
- **Вход через браузер (OAuth 2.1):** укажите только URL `https://arckep.ru/api/v1/mcp` — агент сам откроет окно подтверждения на `arckep.ru` и создаст отдельный ключ в разделе **Настройки → API** (отозвать доступ можно в любой момент удалением ключа).
- **Вход по ключу (`Bearer`):** выпустите ключ с нужными правами на странице [`https://arckep.ru/settings?tab=api`](https://arckep.ru/settings?tab=api) и передайте заголовок `Authorization: Bearer ark_live_...`.
- **Полная документация и каталог моделей с ценами:** [`https://arckep.ru/developer`](https://arckep.ru/developer) и [`https://arckep.ru/developer/docs#mcp`](https://arckep.ru/developer/docs#mcp).

---

## Support & Security

- **Documentation:** [https://arckep.ru/developer/docs](https://arckep.ru/developer/docs)
- **Support & Security Contact:** `support@arckep.ru` (see [SECURITY.md](./SECURITY.md))
- **License:** Documentation and configuration examples in this repository are licensed under [MIT](./LICENSE). Use of the hosted Arckép API and MCP service is governed by the [Arckép Terms of Service](https://arckep.ru/legal).
