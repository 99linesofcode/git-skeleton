# ARCHITECTURE.md

The architecture document for this repository, following the
[architecture.md](https://architecture.md) schema — built so an agent (or a
new colleague) can comprehend the repository from this file alone. This is a
shared-configuration starter, not a service: its "architecture" is the small
set of files it contributes and the merge mechanism by which consumers take
them. Fill every section; update it in the same change that alters the
architecture it describes.

## 1. Project Structure

Flat by design. The repository is the four shared configuration files plus
the README that explains them, and the agent docs every repository carries
(`AGENTS.md`, `GLOSSARY.md`, `docs/`) — there is no `src/`, no build, and no
language toolchain.

```
git-skeleton/
├── .editorconfig      # cross-editor whitespace/encoding contract
├── .prettierrc        # shared Prettier defaults
├── .gitignore         # ignore rules every repo shares (secrets, caches, build output)
├── .ignore            # ignore rules for non-git tooling (devshell, lockfiles, vendored trees)
├── README.md          # how to consume, and the ecosystem rules
├── AGENTS.md          # agent entry point: tracker and domain docs
├── GLOSSARY.md        # the skeleton's own terms (consumers drop it on merge)
├── docs/adr/          # decisions every consumer inherits
├── docs/agents/       # tracker and domain-doc conventions for agents
└── LICENSE
```

**Where the logic lives.** There is no logic: each file is a declarative
contract. The one piece of "behaviour" is the consumption mechanism — a
consumer adds this repo as a remote and merges it, so a merge
conflict is the signal that a repo has deliberately diverged from the shared
default.

## 2. High-Level System Diagram

```
  git-skeleton (this repo)                 consumer repo
  ┌───────────────────────┐               ┌───────────────────────┐
  │ .editorconfig         │   git fetch   │ <project files>       │
  │ .prettierrc           │ ────────────► │ .editorconfig         │
  │ .gitignore            │   git merge   │ .prettierrc           │
  │ .ignore               │   git-skeleton/main                   │
  │ README.md             │               │ .gitignore / .ignore  │
  └───────────────────────┘               └───────────────────────┘
              ▲
              │ built upon by
   ┌──────────┴──────────────────────────────┐
   │ node-skeleton, laravel-skeleton,        │
   │ laravel-package-skeleton, .github-php…  │
   └─────────────────────────────────────────┘
```

## 3. Core Components

| Component | Responsibility |
|---|---|
| `.editorconfig` | Encoding, line endings, indentation (4 spaces default; 2 for YAML; 4 for `compose.yaml`), trailing-whitespace and final-newline rules; markdown exempt from trailing-whitespace trimming |
| `.prettierrc` | Shared Prettier defaults (`semi`, `singleQuote`, `trailingComma: all`) |
| `.gitignore` | Ignore rules every repo shares: `.env` files, `/.direnv`, `node_modules/`, `vendor/`, build output, editor/IDE dirs, test artifacts, Laravel/Playwright caches |
| `.ignore` | Rules for non-git tooling (e.g. ripgrep): `flake.lock`, lockfiles, `_ide_helper.php`, and a `vendor/` allow-list carve-out for `laravel/framework` |
| `README.md` | The consumption recipe and the ecosystem lockfile convention |

### Ports & adapters

Not applicable. This repo owns no ports and no adapters; it is a set of
configuration files, not a component in a running system.

## 4. Data Stores

None. No database, no persistent state.

## 5. External Integrations / APIs

- **GitHub** — the repository is consumed over git (a remote named
  `git-skeleton`, then a merge). No runtime API integration.
- The `.gitignore`/`.ignore` rules name ecosystem artifacts (npm/pnpm/bun
  lockfiles, Composer `vendor/`, Laravel and Playwright caches) but the repo
  itself integrates nothing.

## 6. Deployment & Infrastructure

- **Consumption** (the only "deployment"):
  `/instantiate` runs it; by hand:
  1. `git init`
  2. `git remote add git-skeleton git@github.com:99linesofcode/git-skeleton.git`
  3. `git fetch git-skeleton`
  4. `git merge --allow-unrelated-histories git-skeleton/main`

  Updates flow the same way (`git fetch git-skeleton && git merge
  git-skeleton/main`); a merge conflict is a **divergence point**, resolved
  by keeping the consuming repo's override where it differs from the shared
  default.
- **Ecosystem layers**: `node-skeleton`, `laravel-skeleton`,
  `laravel-package-skeleton`, `.github-php` build on this one and may serve
  as a better jumping-off point for their ecosystem.
- **CI/CD**: none in this repo.
- **Monitoring/logging**: none.

## 7. Security Considerations

- `.gitignore` excludes `.env`, `.env.backup` and `.env.production` so
  secrets are not committed; consumers keep their real values in untracked
  files.
- `auth.json`, `vendor/` and build output are ignored — no credentials or
  generated artifacts are tracked by default.
- No runtime attack surface: the repo contains configuration only.

## 8. Development & Testing Environment

- **Local setup**: none required. Edit the shared file and commit; consumers
  pick it up on their next merge.
- **Testing**: none. The shared config is inert; correctness is observed in
  consumers as a clean merge (or a deliberate conflict).
- **Code quality / gates**: there is no linter or formatter *runner* here
  (the files are the configuration for consumers' tools). The effective gate
  is the **merge**: an incompatible shared change surfaces as a conflict at
  the consumer, so a breaking default cannot silently propagate.
- `.prettierrc` carries empty `plugins`/`overrides` arrays; per-ecosystem
  skeletons extend it rather than this repo growing ecosystem branches.

## 9. Future Considerations / Roadmap

**Deliberate non-goals:**

- **Ecosystem-neutral.** No language toolchain, package manager, or build
  lives here; ecosystem specifics belong in the ecosystem skeleton.
- **Lockfiles are always tracked, never ignored.** Each repo tracks exactly
  the one matching its package manager (`package-lock.json`, `pnpm-lock.yaml`
  or `bun.lock`); bun needs no ignore rule of its own because its cache lives
  outside the repo and `node_modules/` is already covered.
- **Rules stay inert until used.** When an ecosystem grows repo-level
  artifacts, their ignore rules land here; a repo that does not use the
  ecosystem simply never has matching files.
- **No per-project overrides here.** A divergence is resolved in the consumer
  repo, at the merge, not by adding a project branch to the skeleton.

**Known debt / open items**: the `vendor/` allow-list carve-out in `.ignore`
is Laravel-specific; it is deliberately inert for non-PHP consumers.

## 10. Project Identification

Project Name: git-skeleton

Repository URL: https://github.com/99linesofcode/git-skeleton

Primary Contact/Team: Jordy Schreuders (99linesofcode)

Date of Last Update: 2026-11-09

## 11. Glossary / Acronyms

The domain terms live in [`GLOSSARY.md`](GLOSSARY.md); decisions live in
[`docs/adr/`](docs/adr/).

- **Lockfile** — the package manager's pinned dependency record
  (`package-lock.json`, `pnpm-lock.yaml`, `bun.lock`); always tracked.
- **`.ignore`** — ignore rules for non-git tooling (ripgrep, etc.), distinct
  from `.gitignore`.

## 12. Conventions & Boundaries

The house standards this repository expresses and the consumers inherit.

- **The four files are the contract**: `.editorconfig`, `.prettierrc`,
  `.gitignore`, `.ignore`. A consumer merges them; it does not copy
  them by hand.
- **Merge, don't rebase or fork**: updates arrive via
  `git fetch git-skeleton && git merge git-skeleton/main`
  ([ADR 0001](docs/adr/0001-merge-the-skeleton-never-rebase.md)). Resolve a
  conflict by keeping the consumer's override where it deliberately differs
  from the shared default — the conflict is the record of that divergence.
- **Lockfiles are tracked, never ignored** — one per package manager,
  matching the repo.
- **Ecosystem rules stay inert**: an ignore rule for an unused ecosystem
  matches nothing and is harmless; add rules here only for artifacts that
  can appear at a repo root.
- **No secrets**: `.env*` files are ignored; `.env.example` is the template
  consumers commit.
- **Documentation surface**: `README.md` carries the consumption recipe and
  the ecosystem convention; a change to a shared file is documented there.
