# Ideal Postcodes Skills

A collection of skills for AI coding agents (Claude Code, Cursor, Codex) providing guidance for integrating Ideal Postcodes APIs, widgets, and CLI.

## Install

```bash
npx skills add ideal-postcodes/skills
```

Or configure a plugin manually in your Claude Code settings.

## Available Skills

| Skill | Description |
|---|---|
| [`ideal-postcodes-address-finder`](./skills/ideal-postcodes-address-finder) | Address autocomplete widget — suggestions as the user types, populates form fields on selection (`@ideal-postcodes/address-finder`) |
| [`ideal-postcodes-postcode-lookup`](./skills/ideal-postcodes-postcode-lookup) | Postcode search widget — postcode in, address dropdown out, fills the form (`@ideal-postcodes/postcode-lookup`) |
| [`ideal-postcodes-react`](./skills/ideal-postcodes-react) | React address autocomplete component — `<AddressFinder>` for React/Next.js, auto-populates fields on selection (`@ideal-postcodes/react`) |
| [`ideal-postcodes-api-integration`](./skills/ideal-postcodes-api-integration) | Direct API integration — auth, `/postcodes`, `/addresses`, `/udprn`, `/autocomplete`, client libraries, error handling |
| [`ideal-postcodes-cli`](./skills/ideal-postcodes-cli) | Drive the API from the terminal — manage keys, cleanse addresses, resolve from partial queries via the `idpc` CLI |

Each skill includes quickstarts, critical gotchas, and reference documentation tailored for agents. For comprehensive API reference, see [docs.ideal-postcodes.co.uk](https://docs.ideal-postcodes.co.uk).

## Source

These skills are authored in the private [`ideal-postcodes/atlas`](https://github.com/ideal-postcodes/atlas) monorepo under `packages/skills-ideal/` and mirrored to this public repo. To contribute:

1. **Report issues** here on GitHub
2. **Submit PRs** here; maintainers will cherry-pick into atlas
3. **Reach out** at support@ideal-postcodes.co.uk if you have questions

## License

SEE LICENSE IN LICENSE
