# Contributing

Contributions are welcome — bug reports, feature requests, and pull requests.

## Setup

```sh
git clone https://github.com/aohana182/CVAI.git
cd CVAI
npm install
cp .env.example .env   # fill in OAuth, DB, n8n values
npm run dev
```

## Workflow

1. Branch from `main`: `git checkout -b feat/your-feature`
2. Make changes
3. Commit with [Conventional Commits](https://www.conventionalcommits.org): `feat(scope): description`
4. Open a PR against `main`

## Commit format

```
type(scope): subject

- What changed
- Why it matters
- How verified
```

Types: `feat`, `fix`, `chore`, `docs`, `refactor`, `test`

## Code style

```sh
npm run lint
```
