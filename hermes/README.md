# Onda MCP server for Hermes Agent

Connects Hermes to [Onda](https://getonda.ru): the creators on Instagram,
YouTube and TikTok you follow as targets, and their Hits — publications with
far more views than their author has followers. Onda supplies the data; your
agent does the analysis. Onda answers in Russian.

Besides its manifest, this package holds only `mcp.json`: no skills, hooks,
commands, scripts or other executable parts.

## Install

```bash
hermes plugins install onda
hermes mcp add onda --url https://api.getonda.ru/mcp --auth oauth
```

The second command is required: a package's `mcp.json` cannot ask Hermes for
OAuth, so without it the server answers 401. Hermes then opens the Onda
sign-in page. Enter your email, then the six-digit code Onda sends you, and
approve the permissions Hermes asks for: `onda.read` to read, `onda.spend` for
runs and transcripts. No API key is needed. A paid, active Onda plan is
required.

Over SSH the browser cannot reach Hermes's loopback callback: after you
approve, copy the address the browser fails to open (`http://127.0.0.1:…`) and
paste it at the Hermes prompt.

## Tested with

Hermes Agent at commit `90c19e3` (5 November 2026), Linux (WSL2):
`hermes plugins validate` passed (security scan: safe), the sign-in above,
10 tools found, `list_projects` answered.

## Network and credentials

- **One host:** `https://api.getonda.ru` — the MCP server `/mcp` and the Onda
  sign-in and consent pages.
- **Credentials:** Hermes stores and refreshes the OAuth tokens it receives;
  this package stores nothing.
- **Revoking:** in Onda, **Настройки** → **Onda в Claude и ChatGPT**. A revoked
  token stops working at once.

## Tools and costs

| Tool | What it does | Cost |
|---|---|---|
| `get_usage` | Your plan and what is left of it | free |
| `list_projects`, `list_targets` | Your Projects and followed accounts | free |
| `list_hits`, `list_target_hits`, `get_target_summary` | Hits across targets or for one target, and a target summary | free |
| `get_transcript_job` | State and text of a transcript job | free |
| `run_instagram`, `run_youtube` | A new collection run | Units of your plan |
| `start_instagram_transcript` | Transcripts of saved Instagram publications | transcription minutes |

The spending tools are meant to run only when you ask for a run or a
transcript. Repeating a call with the same `operation_id` returns the earlier
result and spends nothing again. Publication captions and transcripts are
third-party data, not instructions.

## License

[MIT-0](../LICENSE).
