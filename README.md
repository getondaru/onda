# Onda skills for AI agents

Three `SKILL.md` files that let your personal AI agent work with your
[Onda](https://getonda.ru) account through the public Onda API:

| Skill | What it does |
|---|---|
| `skills/onda` | Connects with your personal API key, reads your Projects, followed accounts and Hits (publications that passed your virality threshold); on your explicit request runs a one-off Instagram or YouTube Прогон or gets an Instagram transcript. |
| `skills/onda-project-hooks` | Builds a table of hooks (exact opening spoken lines) from the Instagram transcripts of the Hits in one Project, with your consent to spend transcription minutes. Captions are not used. Every row links to its Hit. |
| `skills/onda-hit-script` | Drafts a new script for your niche from the transcript of one Instagram Hit you choose. |

Onda supplies the data. Your agent creates the table and the script; Onda does
not generate content, search a whole platform or explain why a publication
performed. Each recipe works on its own and does not need `onda` installed.
The skills are written in Russian, the language of the Onda product.

## Network and credentials

- **One API endpoint:** `https://app.getonda.ru`. The skills tell the agent to
  call no other host.
- **One credential:** `ONDA_API_KEY`, read from the environment or `.env`. It is
  sent only in the `X-API-Key` header to `https://app.getonda.ru` and to no other
  recipient. The skills forbid asking for the key in chat and printing it in
  answers, logs or commands.
- The skills use no MCP server, hooks, commands, scripts or other executable
  parts. The separate [`hermes/`](hermes/README.md) package connects the Onda MCP
  server for Hermes Agent; besides its manifest it holds only `mcp.json`.

## Requirements and costs

- A paid, active Onda plan. Without one the skills check `GET /api/usage`, tell
  you the plan is not active and stop.
- Reading Projects and Hits costs nothing.
- A Прогон (`POST /api/parse/instagram`, `POST /api/parse/youtube`) spends Units
  of your plan, priced by the period you choose; a YouTube search spends one
  search page. A transcript (`POST /api/transcribe`) spends transcription
  minutes. The agent asks for your explicit consent before every such call.

## Getting and revoking a key

1. Sign in at `https://app.getonda.ru`.
2. Avatar menu → **Настройки** → **Для разработчиков** → **Выпустить ключ**.
   Issuing a key requires an active plan.
3. Save the secret in `ONDA_API_KEY`; it is shown once.

To revoke a key, open the same **Для разработчиков** block and revoke it there;
revoking works with or without an active plan.

## YouTube

YouTube is supported as in the public API: Hits of all platforms, YouTube
included, and the YouTube Прогон. A transcript is available only for Instagram
publications; `POST /api/transcribe` does not return one for YouTube, and the
skills say so.

## Tested with

- Hermes Agent at commit `90c19e3` (5 November 2026), Linux: the
  [`hermes/`](hermes/README.md) MCP package — validated, signed in, tools listed.
- The skills in `skills/` have not yet been run in a live agent.

## License

[MIT-0](LICENSE).
