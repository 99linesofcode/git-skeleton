# git-skeleton

The shared configuration every repository starts from, and the way repositories take it and its updates.

## Language

**Skeleton**:
A starter repository whose history other repositories merge, so they inherit its files and every later change to them.
_Avoid_: template, boilerplate, starter kit

**Ecosystem skeleton**:
A skeleton that merges git-skeleton and adds one language's toolchain, such as `node-skeleton` or `laravel-skeleton`.
_Avoid_: stack skeleton, language skeleton

**Consumer**:
A repository that has merged a skeleton and takes its updates by merging it again.
_Avoid_: child, downstream, fork

**Shared default**:
A setting a skeleton ships for every consumer, which a consumer keeps unless it deliberately overrides it.

**Divergence point**:
A merge conflict between a skeleton update and a consumer's override of a shared default; the consumer resolves it by keeping its override.
