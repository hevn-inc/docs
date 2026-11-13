# HEVN documentation

Source of the HEVN documentation site, built with [Mintlify](https://mintlify.com). Pages are MDX files with YAML frontmatter.

## Layout

| Path | Contents |
| --- | --- |
| `general/` | How the platform is built |
| `remote-transfers/` | API keys for automating your account |
| `whitelabel/` | Integration guides and reference for the Platform API |
| `api-reference/` | Overview page of the REST API tab |
| `snippets/` | Code shared between pages |
| `openapi.json` | The Platform API contract, source of the REST API tab. Generated from the backend schemas: edit field descriptions there |
| `docs.json` | Navigation, redirects and site settings |

## Preview locally

```bash
npm i -g mint
mint dev
```

Run `mint dev` in the repository root, next to `docs.json`. The preview opens at `http://localhost:3000`.

Run `mint broken-links` before you open a pull request.

## Writing rules

Read [`AGENTS.md`](AGENTS.md) before you edit a page. It sets the terminology, page shape, banned patterns and fintech vocabulary.

## Publishing

Changes deploy when they reach the default branch.
