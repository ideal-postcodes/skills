# Ideal Postcodes Skills

A collection of skills for AI coding agents (Claude Code, Cursor, Codex) providing guidance for integrating Ideal Postcodes and AddressZen APIs and widgets.

## Install

```bash
npx skills add ideal-postcodes/skills
```

Or configure a plugin manually in your Claude Code settings.

## Available Skills

| Skill | Description |
|---|---|
| [`address-finder`](./skills/address-finder) | Address autocomplete widget (@ideal-postcodes/address-finder, @addresszen/address-lookup) |
| [`postcode-lookup`](./skills/postcode-lookup) | Postcode search widget (@ideal-postcodes/postcode-lookup) |
| [`api`](./skills/api) | Direct API integration (HTTP, SDK, Node.js, browser) |

## About These Skills

Skills include quickstarts, critical gotchas, and reference documentation tailored for agents. For comprehensive API reference, see [docs.ideal-postcodes.co.uk](https://docs.ideal-postcodes.co.uk).

## Source

These skills are authored in the private [`ideal-postcodes/atlas`](https://github.com/ideal-postcodes/atlas) monorepo under `packages/skills-ideal/` and mirrored to this public repo. To contribute:

1. **Report issues** here on GitHub
2. **Submit PRs** here; maintainers will cherry-pick into atlas
3. **Reach out** at support@ideal-postcodes.co.uk if you have questions

## License

SEE LICENSE IN LICENSE
