<p align="center">
  <img src="assets/logo-400.png" width="120" alt="Sooveryn">
</p>

# Sooveryn MCP

**Sooveryn is a hosted MCP server. Its code is not public: this repository only holds its documentation and configuration examples.**

Lasting memory and an AI team for Claude Code.

Sooveryn gives Claude Code a team of AI personas that remembers your project. Soov, the partner and team lead, keeps your decisions and how you work in a lasting memory. He finds them again by words and by meaning, from one session to the next and on every machine.

On paid plans, he consults his teammates (Néo for security, Vera to challenge your ideas, Iris for design, Maria for SEO) or has them debate before deciding, and you can create your own personas and skills.

- Website: https://www.sooveryn.com
- Server: `https://mcp.sooveryn.com/mcp` (streamable HTTP, API key in a header)
- MCP Registry (registry.modelcontextprotocol.io): `com.sooveryn/sooveryn`
- Works with Claude Code, VS Code, Orca, Pi, and MCP clients that accept a remote HTTP server with a key

## Get started

1. Create a free account at https://www.sooveryn.com and confirm your email address.
2. Create an API key on the API keys page (https://www.sooveryn.com/me/keys). One key per machine is best.
3. Add the configuration of your client, below. Your key acts for your account and reads your memory: keep it out of anything that goes to git.
4. In a session, say **Soov Init**. The agent writes a `sooveryn.md` file at the root of the project, keeps it out of git, and loads it at every session through an `@sooveryn.md` line in `CLAUDE.md`. From then on, each session starts with Soov and his memory of the project.

### Claude Code and Orca

Put your key in an environment variable named `SOOVERYN_API_KEY`, set where Claude Code starts (your shell profile, for example). Then add `.mcp.json` at the root of the project ([example](examples/.mcp.json)). Claude Code replaces `${SOOVERYN_API_KEY}` when it loads the file, so the file holds no secret:

```json
{
  "mcpServers": {
    "sooveryn": {
      "type": "http",
      "url": "https://mcp.sooveryn.com/mcp",
      "headers": {
        "Authorization": "Bearer ${SOOVERYN_API_KEY}"
      }
    }
  }
}
```

Orca runs Claude Code, which reads this file.

### VS Code

`.vscode/mcp.json` ([example](examples/.vscode/mcp.json)). VS Code ignores the headers of a `.mcp.json`, so it needs its own file, under the `servers` key. It asks for your key once, hides it while you type, and stores it securely:

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "sooveryn-key",
      "description": "Sooveryn API key",
      "password": true
    }
  ],
  "servers": {
    "sooveryn": {
      "type": "http",
      "url": "https://mcp.sooveryn.com/mcp",
      "headers": {
        "Authorization": "Bearer ${input:sooveryn-key}"
      }
    }
  }
}
```

### Pi

Install its MCP extension. Pi then reads the same `.mcp.json` as Claude Code.

```bash
pi install npm:pi-mcp-adapter
```

If Pi does not pick the key up from the environment variable, write the key itself in the file, as below.

### Other MCP clients

| Setting | Value |
|---|---|
| Transport | Streamable HTTP |
| URL | `https://mcp.sooveryn.com/mcp` |
| Header | `Authorization: Bearer <YOUR-API-KEY>` |
| Optional header | `X-Project-Scope: <project-name>` |

Without `X-Project-Scope`, the agent passes the project name on each call, as `sooveryn.md` tells it.

### If the key is written in the file

The API keys page of the website gives these files with your key written in. Keep them out of git, in `.gitignore`:

```gitignore
.mcp.json
.vscode/mcp.json
```

A key committed once stays in the history, even after you delete the file: revoke it on the API keys page and create a new one.

## Tools

| Tool | What it does |
|---|---|
| `init` | Sets Sooveryn up in the current project, when you say "Soov Init". |
| `activate` | Starts a persona: its role, its rules, and the part of its memory that fits the session. |
| `search_memory` | Searches a persona's memory, by words and by meaning. |
| `write_event` | Records what happened. |
| `update_semantic` | Creates or updates a named fact sheet. |
| `update_procedural` | Creates or updates a named working rule. |
| `write_previous_context` | Leaves a short handover for the next session on the project. |
| `clear_previous_context` | Clears that handover once it has been applied. |
| `start_debate` | Opens a debate between two to four personas, after checking the plan and its quota, and returns the protocol to follow. |
| `list_skills` | Lists the skills the account can use: the catalogue and your own. |
| `get_skill` | Returns the instructions of a skill. |
| `stats` | Counts a persona's memory entries. |
| `reindex` | Rebuilds the cache of a persona's memory index. |

## Memory and privacy

- Three kinds of memory: what happened, what is known, and how to work. Each project has its own; a general memory shared across projects comes with Pro.
- The memory keeps what the personas write into it. You see all of it on the website, and a copy of your data is available on request, on every plan.
- Memory contents are encrypted in the database with a key per account, itself protected by a master key kept outside the database. Because the server holds the master key, Sooveryn can technically decrypt your memory to serve you: this encryption mainly protects against theft or leaks of the database and its backups. It is not end-to-end encryption.
- Some information stays unencrypted, so the service can sort and filter: identifiers, the persona and type of each memory, project names, counters and dates, your account data, and the personas and skills you create.
- The servers are in France. When a persona is activated, the relevant part of your memory goes to your AI client, which sends it to your AI provider. With the Consolidation option (Pro), extracts of your memory are sent to Anthropic, in the United States, to propose consolidations.
- Details: [privacy policy](https://www.sooveryn.com/privacy) and [terms](https://www.sooveryn.com/terms).

## Good to know

- What is written into memory comes back at later sessions. Text that an agent copied from an untrusted source (a web page, an issue, a file) can steer later sessions: look through your memory on the website from time to time.
- A `.incognito` file at the root of a project stops memory writes for the session. The agent applies it, not the server, which never sees your files.

## Plans

Free: Soov alone, on one project, with 300 calls and 50 memory writes a day. Paid plans open the whole team, debates, and your own personas and skills; Pro adds the general memory and memory consolidation. Prices and details: https://www.sooveryn.com/#pricing.

## Support and security

- Support: support@sooveryn.com
- To report a vulnerability, see [SECURITY.md](SECURITY.md). Never in a public issue.
- Issues on this repository are welcome for mistakes in the documentation. Never paste your key, your configuration file or your memory in one: this page is public.

## License

The documentation and examples in this repository are under the [MIT license](LICENSE). The Sooveryn service, its code, its name and its logo are not.

Sooveryn is published by Cyril Pierron EI, trading as PANDAPROG, France. Legal notice: https://www.sooveryn.com/legal.
