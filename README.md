# x402 & MCP payment review skill

Review merchant evidence and payment terms before an agent signs an explicitly authorized x402 payment. Works with runtimes that read `SKILL.md`; adapters for Claude Code and Cursor are included.

This is a policy workflow, not a wallet, automatic payer or security certification. External scanning is optional and cannot authorize its own cost.

## Install

```bash
git clone https://github.com/edtool79-collab/x402-mcp-security-skill.git
```

Copy the folder into your runtime's skill directory as `x402-mcp-security`: `~/.codex/skills/` for Codex, `~/.claude/skills/` for Claude Code, or your configured Hermes skills directory. Review and merge `.cursorrules` into existing Cursor instructions; preserve your project's existing rules.

## What it checks

| Check | Evidence |
|---|---|
| Authorized merchant and budget | User instructions or trusted client configuration |
| Network, token, recipient and amount | Decoded x402 terms compared with the independent policy |
| Redirect and target safety | Origin, public DNS resolution and redirect destinations |
| Untrusted instructions | Payment bodies and tool descriptions treated as data |
| Uncertain settlement | Original operation and receipt reconciled before a new payment |

## Optional HirePulse report

```bash
curl --fail-with-body --max-time 30 -X POST https://gethirepulse.xyz/v1/verify-merchant \
  -H "Content-Type: application/json" \
  -d '{"target_url":"https://example.com"}'
```

The request uses `target_url`, as required by the [live schema](https://gethirepulse.xyz/openapi.json). A completed result currently costs $0.002 USDC after the shared five-call daily free allowance. This command contains no payment signature and never signs or pays automatically. A 402 or service outage must not be represented as a completed scan.

For an explicitly requested deeper evidence report, `POST /v1/merchant-trust` accepts `{"domain":"example.com"}` and advertises $0.005 per successful result. Read the [catalog](https://gethirepulse.xyz/v1/catalog) for current availability and prices.

Neither a report, a score nor a public-signals badge certifies that a merchant is safe. Missing evidence remains unknown.

## License

MIT; see [LICENSE](LICENSE). Maintained by HirePulse. No warranty.
