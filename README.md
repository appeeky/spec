# Appeeky Spec

Your app's App Store and Google Play listing as files in your repo: checked against the store rules, diffed before it goes live, pushed only after a merge, and measured. Run by you or by your coding agent.

- Website: [spec.appeeky.com](https://spec.appeeky.com)
- CLI: [`@appeeky/spec` on npm](https://www.npmjs.com/package/@appeeky/spec)
- Docs: [docs.appeeky.com/docs/spec](https://docs.appeeky.com/docs/spec)
- Agent skills: [appeeky/skills](https://github.com/appeeky/skills)

```bash
npx @appeeky/spec init
```

Or let your coding agent do it: `npx skills add appeeky/skills`, then ask it to "Set up Appeeky Spec for this app."

## What is in this repo

This repo holds the parts of Appeeky Spec you use from your own repo or server. They are copied here from each release.

| Path | What it is |
| --- | --- |
| `action.yml` | The GitHub Action for pull requests: `uses: appeeky/spec@v0` |
| `examples/weekly/` | The weekly co-founder review: a GitHub Actions workflow, a cron script and the review prompt |
| `docker/docker-compose.yml`, `docker/.env.example` | Run the daily, push and weekly routines on your own server with `ghcr.io/appeeky/appeeky-spec` |

### The pull request check

`npx @appeeky/spec init --ci` writes it for you. By hand:

```yaml
# .github/workflows/listing-check.yml
on: pull_request
permissions:
  contents: read
  pull-requests: write
jobs:
  spec:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with: { fetch-depth: 0 }
      - uses: appeeky/spec@v0
```

It comments what the listing changes do to keyword coverage and fails on check errors.

Inputs: `working-directory` (the folder with `appeeky.spec.yaml`), `base` (the ref to compare with), `comment` (`true`), `version` (the `@appeeky/spec` version, `latest`), `github-token`.

### Your own server

Put `docker/docker-compose.yml` and a `.env` filled from `docker/.env.example` in one folder, then `docker compose up -d`. See [self-hosting](https://docs.appeeky.com/docs/spec-self-hosting), or install the `appeeky-spec-server` skill and let your agent set it up.

## License

[FSL-1.1-MIT](LICENSE).
