# FreeJev skill

Use FreeJev from an AI agent with the product's existing authorization and usage rules.

[Website](https://freejev.org/) · [Agent setup](https://freejev.org/docs/agents)

## Install

With Node.js 22.20 or newer and npm available, install the skill using the skills CLI:

```sh
npx skills add freejev/skills --skill freejev
```

Select your supported agent and installation scope in the installer. Restart your agent session after installation. This installs instructions; it does not connect MCP, install the product CLI, sign you in or grant access. Follow [Agent setup](https://freejev.org/docs/agents) to connect the product.

Alternatively, use the existing npm installer:

```sh
npx freejev-client skill install --target codex
# Or use --target grok
```

Choose one installer for this skill so two copies do not drift. For CLI usage, install `freejev-client` separately and follow the setup documentation. Credentials belong in your secret store, never in this repository.

## Updates

For an installation managed by the skills CLI, use `npx skills update freejev`. Choose the same project or global scope used during installation. Review any local edits before updating. For the npm installer, rerun its install command; it protects modified files.

## Usage and support

1 credit = 1,000 actual input tokens. All channels share your account credits. Output tokens are free.

Installing the skill is free. Product calls follow the site's account, authorization and billing rules. For setup and product support, see [the documentation](https://freejev.org/docs/agents).

Skill source version: 0.3.0. MIT licensed; see [LICENSE](LICENSE).
