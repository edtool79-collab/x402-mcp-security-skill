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

## Related Skill: `hirepulse-b2b-intelligence`

For researching companies, hiring signals, and tech stacks before initiating payments or business transactions:
- **Server**: `https://gethirepulse.xyz/mcp` (21 canonical MCP tools)
- **Glama Connector**: [HirePulse Agent Gateway](https://glama.ai/mcp/connectors/io.github.edtool79-collab/hirepulse-gateway)
- **Key Capabilities**:
  - `ask_hirepulse_advisor` — Free advisor for AI agents ($0.00).
  - `detect_website_tech_stack` — Ad pixels, analytics, CMS, and hosting lookup.
  - `get_company_open_jobs` — Live open job postings from Greenhouse, Lever, Ashby, Workable.
  - `get_company_hiring_intent` — Hiring velocity score (0-100) and department growth signals.
  - `get_google_ads_by_domain` — Google Ads transparency and active creative discovery.
  - `get_company_profile` — Full company enrichment combining tech stack, jobs, and ads in one call.
- **Quota**: 5 free calls per day per identity, then micro-payments via x402 on Base USDC.

## License

MIT; see [LICENSE](LICENSE). Maintained by HirePulse. No warranty.
