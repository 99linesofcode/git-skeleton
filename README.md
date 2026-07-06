# git-skeleton

This repository is the starting point for my Git repositories. It allows me to configure and update common configuration files across all my repositories using such tools as [multi-gitter](https://github.com/lindell/multi-gitter).

Some ecosystems and projects have their own dedicated skeleton repositories that build upon this one and might serve as a better jumping off point. You can find those at https://github.com/99linesofcode?tab=repositories&q=skeleton.

## How to use

1. Git initialize `git init`;
1. Add origin `git remote add origin <REPOSITORY>`
1. Add the skeleton as a secondary remote `git remote add git-skeleton git@github.com:99linesofcode/git-skeleton.git`;
1. Update local repository using `git fetch skeleton`;
1. Commit changes to the package repository `git rebase skeleton/main`.

## Contributing

Please review the [Contribution Guidelines](https://github.com/99linesofcode/.github/blob/main/.github/CONTRIBUTING.md).

## Code of conduct

In order to ensure that the community is welcoming to all, please review and abide by the [Code of Conduct](https://github.com/99linesofcode/.github?tab=coc-ov-file).

## Security vulnerabilities

Please review the [security policy](https://github.com/99linesofcode/.github?tab=security-ov-file) on how to report security vulnerabilities.

## License

This software is open source and licensed under the [MIT license](https://github.com/99linesofcode/.github?tab=MIT-1-ov-file).
