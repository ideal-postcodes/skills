# Agent Skills

Distribution repo for Ideal Postcodes skills. Skills are authored in the private `ideal-postcodes/atlas` monorepo and mirrored here.

## Source of Truth

The private `ideal-postcodes/atlas` monorepo is the canonical source:

- **Path:** `atlas/packages/skills-ideal/`
- **Authoring:** see `CLAUDE.md` in that directory
- **Version:** managed via release-please alongside other packages

## This Repo

This public mirror at `ideal-postcodes/skills`:

- Tracks the latest release of all skills from atlas
- Is distribution-only (tagged releases for agent tooling to discover)
- Accepts issues and PRs from the community

## Contributing

To contribute changes:

1. **File an issue** here describing the problem or suggestion
2. **Propose a PR** with changes to skill files
3. A maintainer will review, cherry-pick the changes into atlas, and merge them back here on the next release

We keep the cherry-pick workflow (rather than direct mirroring) to maintain a clear edit path and protect the private atlas repo.

## License

SEE LICENSE IN LICENSE
