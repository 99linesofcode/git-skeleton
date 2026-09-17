# git-skeleton

The starting point for my Git repositories. It provides the common configuration
files (`.editorconfig`, `.prettierrc`, `.gitignore`, `.ignore`) that every
repository shares.

Some ecosystems and projects have their own dedicated skeleton repositories
that build upon this one and might serve as a better jumping off point. You can
find those at https://github.com/99linesofcode?tab=repositories&q=skeleton.

## How to use

1. `git init`
2. `git remote add origin <REPOSITORY>`
3. `git remote add git-skeleton git@github.com:99linesofcode/git-skeleton.git`
4. `git fetch git-skeleton`
5. `git rebase git-skeleton/main`

Updates flow the same way: `git fetch git-skeleton && git rebase git-skeleton/main`.
Conflicts on rebase are the divergence points — resolve them by keeping your
repo's override where it differs from the shared default.

## Ecosystems

The shared config is ecosystem-neutral. Package manager lockfiles are always
tracked, never ignored — each repository tracks exactly the one matching its
package manager:

- `package-lock.json` (npm)
- `pnpm-lock.yaml` (pnpm)
- `bun.lock` (bun)

Bun needs no ignore rules of its own: its cache lives outside the repository
and `node_modules/` is already covered. When an ecosystem grows repo-level
artifacts, their ignore rules land here — repos that don't use the ecosystem
simply never have matching files, so the rules stay inert.

## Contributing

Please review the [Contribution Guidelines](https://github.com/99linesofcode/.github/blob/main/.github/CONTRIBUTING.md).

## Code of conduct

In order to ensure that the community is welcoming to all, please review and abide by the [Code of Conduct](https://github.com/99linesofcode/.github?tab=coc-ov-file).

## Security vulnerabilities

Please review the [security policy](https://github.com/99linesofcode/.github?tab=security-ov-file) on how to report security vulnerabilities.

## License

This software is open source and licensed under the [MIT license](https://github.com/99linesofcode/.github?tab=MIT-1-ov-file).
