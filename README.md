<p align="center">
  <img src="./assets/saleseq-logo-black.svg" width="72" alt="SalesEQ">
</p>

<h1 align="center">SalesEQ for AI agents</h1>

<p align="center">
  Your meetings, in the conversation.<br>
  Official plugin for Claude Code, Cursor, and Codex.
</p>

---

SalesEQ records, transcribes, and analyzes the meetings your team runs. This plugin connects the
[SalesEQ MCP server](https://www.saleseq.ai/docs/ai-assistants/overview) to your coding agent so it
can search across every transcript, pull summaries, read exact wording, and answer "who said what,
and when" — without leaving the conversation.

## Install

**Claude Code**

```bash
claude plugin marketplace add SalesEQ/plugins
claude plugin install saleseq@saleseq
```

**Cursor**

```
/add-plugin saleseq
```

**Codex** — install from the plugin directory, or point Codex at this repository.

On first use your client opens a browser window to sign in to SalesEQ and authorize access. You
need a SalesEQ account at [app.saleseq.ai](https://app.saleseq.ai) with at least one meeting
recorded or uploaded.

Not using one of these clients? Any MCP-compatible agent can connect directly to
`https://mcp.saleseq.ai` — see [Connect a custom MCP client](https://www.saleseq.ai/docs/ai-assistants/connect-custom-mcp).

## What you get

### Tools

Six read-only tools. None of them modify SalesEQ data, send notifications, write to your CRM, or
make external requests on your behalf.

| Tool | What it does |
| --- | --- |
| `search_meetings` | Search transcripts by topic, phrase, or speaker. Returns best-matching meetings with snippets and playback offsets. |
| `list_meetings` | List accessible meetings with title, participants, duration, and platform. Filter by creator, participant, speaker, or duration. |
| `get_meeting_summary` | The generated summary of a meeting — far cheaper than reading a full transcript. |
| `get_transcript` | A meeting's transcript, optionally sliced to a time range so long meetings don't blow out context. |
| `get_meeting_frame` | A still frame from the recording around a timestamp. Audio-only and transcript-only meetings have none. |
| `get_org_context` | Resolve a teammate's name or email to their SalesEQ account before searching their meetings. |

Tools return only what your account can already reach: meetings you own, meetings shared with you
directly, and meetings shared with a group you belong to. Full permission model in the
[overview](https://www.saleseq.ai/docs/ai-assistants/overview).

### Skills

| Skill | What it does |
| --- | --- |
| `saleseq-docs` | Answers questions about how SalesEQ works — recording, sharing, permissions, integrations, billing — by fetching the live docs instead of guessing. |

## Try it

```
What did we commit to on my last call with Acme?
Summarize every customer call I had this week.
Find where pricing came up across my meetings this month.
Who raised the security question, and what did we say?
How do auto-share rules work?
```

## Repository layout

```
.claude-plugin/
  marketplace.json     # so `claude plugin marketplace add SalesEQ/plugins` works
  plugin.json
.cursor-plugin/plugin.json
.codex-plugin/plugin.json
.mcp.json              # shared by all three clients
skills/saleseq-docs/
assets/
```

The plugin lives at the repository root, so the marketplace entry uses `"source": "./"`. All three
client manifests share one `.mcp.json` and one `skills/` directory — only the metadata differs.

## Releasing

`.claude-plugin/plugin.json` deliberately **omits** `version`. Claude Code then falls back to the
git commit SHA, so every push to `main` is picked up as a new version with no manual bump. If you
add a `version` there, Claude Code pins to that string and existing installs stop updating until
it changes.

The Cursor and Codex manifests do carry `version`. Bump both together when you cut a release.

Validate before pushing:

```bash
claude plugin validate .
```

## Links

- [Documentation](https://www.saleseq.ai/docs)
- [AI assistants overview](https://www.saleseq.ai/docs/ai-assistants/overview)
- [Troubleshooting](https://www.saleseq.ai/docs/ai-assistants/troubleshooting)
- [app.saleseq.ai](https://app.saleseq.ai)

## Support

Questions or a bug in the plugin: [open an issue](https://github.com/SalesEQ/plugins/issues).
Anything account-related: support@saleseq.ai.

## License

MIT — see [LICENSE](./LICENSE).
