# Handrail packs

The policy packs that [Handrail](https://handrail.bitey.ai) installs for coding agents —
security, privacy and, increasingly, conventions. **This repository is where packs are
written, reviewed and versioned.**

| | |
|---|---|
| Browse | [handrail.bitey.ai/#packs](https://handrail.bitey.ai/#packs), or `handrail list` |
| Install a pack | `handrail enable <pack>` |
| Validate your changes | `handrail check .` |

Today each Handrail release embeds a snapshot of these packs. The upcoming
`handrail update` / `handrail upgrade` fetch newer versions from here, signed and verified,
without a new release of the tool.

## Layout

```
packs/<id>/
├─ pack.toml                   metadata, tier, per-agent spec, tradeoffs, limits
├─ rules.md                    instructions for the agent (English)
├─ claude-code/
│  ├─ settings.json            merged as a Claude Code managed-settings.d fragment
│  └─ hooks/*.sh               optional
└─ tests/*.cases               hook test vectors: name <TAB> expected exit <TAB> JSON input
profiles/<name>.toml           named sets of packs
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). In short: one pack per pull request, `handrail check .`
must pass, only claim `enforced` for what the agent actually enforces, and keep `limits`
honest. Packs with executable hooks get a stricter review.

## License

[Apache-2.0](LICENSE). See [NOTICE](NOTICE).
