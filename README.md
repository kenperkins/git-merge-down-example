# Git-Merge-Down-Example

This repo is an example of what a [merge-down strategy]([https://www.agileconnection.com/article/picking-right-branch-merge-strategy](https://www.agileconnection.com/article/picking-right-branch-merge-strategy)) looks like with strict [SemVer]([https://semver.org/](https://semver.org/)), with multiple concurrent versions and branches that reinforce that strategy.

There are a few assumptions in a merge down strategy:

1. All commits should target master by default
   * Exceptions are when a targeted bug fix only applies to an older version, and thus must be directly applied
1. Commits that are applicable (backwards compatible features and bug fixes) are cherry picked from master appropriatley

This example has 2 major version branches, as well as vNext (Master), and a number of minors and patches for each:

* `master` (vNext)
* `2.x`
* `1.x`

In this example, `0.x` is an unsupported release and will get no additional changes merged down, so there are no branches, and only tags for these releases.

At any given point, a commit to master may result in a new major, but that is contextual based on the changes in that commit.

Each Major has a few minor version branches:

* `2.0.x`
* `2.1.x`
* `1.0.x`
* `1.1.x`
* `1.2.x`

There are no branches for patch level changes as they are a point in time and as such are only represented by a tag.
