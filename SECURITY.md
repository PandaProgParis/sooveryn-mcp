# Security

Sooveryn is a hosted service. Its code is not in this repository, which only holds documentation and configuration examples.

## Scope

- https://www.sooveryn.com, the website and account pages
- https://mcp.sooveryn.com, the MCP server
- https://api.sooveryn.com, the API

Any other host is out of scope, even at the same address.

## Reporting a vulnerability

Report it privately, never in a public issue:

- with the **Report a vulnerability** button of this repository's Security tab;
- or by email to support@sooveryn.com, with "Security" in the subject.

Say what you found, how to reproduce it, and what it exposes.

We acknowledge every report within 5 working days and keep you informed until it is fixed. Please wait for the fix, or 90 days after your report, before disclosing it, and agree on the date with us.

## Rules

- Test only with accounts you own.
- If you reach data that is not yours, stop there: do not keep it, do not share it, and tell us what you accessed.
- Do not change or delete data that is not yours.
- No automated scanning, load or denial-of-service testing, spam, social engineering, or physical attacks.

There is no bug bounty program.

## Your API key

Your API key acts for your account and reads your memory. Keep it out of anything that goes to git: the [README](README.md) shows how to load it from an environment variable (Claude Code) or a secure prompt (VS Code). If a key leaks, even once in a git history, revoke it on the API keys page (https://www.sooveryn.com/me/keys) and create a new one.
