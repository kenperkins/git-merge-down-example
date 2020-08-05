# Git-Merge-Down-Example

This repo is an example of what a [merge-down strategy]([https://www.agileconnection.com/article/picking-right-branch-merge-strategy](https://www.agileconnection.com/article/picking-right-branch-merge-strategy)) looks like with strict [SemVer]([https://semver.org/](https://semver.org/)), with multiple concurrent versions and branches that reinforce that strategy.

There are a few assumptions in a merge down strategy:

1. All commits should target master by default
   * Exceptions are when a targeted bug fix only applies to an older version, and thus must be directly applied
1. Commits that are applicable (backwards compatible features and bug fixes) are cherry picked from master appropriatley

This example has 2 major versions, as well as vNext (Master), and a number of minors and patches for each:

* `master` (vNext)
* `2.x`
* `1.x`

In this example, `0.x` is an unsupported release and will get no additional changes merged down, so there are no branches, and only tags for these releases.

At any given point, a commit to master may result in a new major, but that is contextual based on the changes in that commit.

Each Major has a few minor version branches:

* `2.1.x`
* `2.0.x`
* `1.2.x`
* `1.1.x`
* `1.0.x`

There are no branches for patch level changes as they are a point in time and as such are only represented by a tag.

### Open Questions

1. When do you create the release branch?
	1. At the time of the release? This would be basically `git tag -am "Release 1.0.0` and then `git checkout -b 1.0.x` and pushing both tags and branches.
		* This enables setting up protected branch rules in github, at the inception of the release, which is a good standard practice
	3. At the time of taking a new change for the next release from said branch? I.e. `1.0.1` in the above example.
		* This would be `git checkout 1.0.0` and then `git checkout -b 1.0.x` and pushing the branch.
		* If you don't create the release branch at the time of the initial release, you'll have to audit every commit that goes to master and whether it's a breaking change or not, and before it merges create the branch, or retroactively checkout the tag and create a branch from that.
2. Should `RELEASENOTES` include only those from the branch in question? or all versions. For example, if release `1.1.0` is made after release `1.0.1`, technically release `1.1.0` has no knowledge of `1.0.1` but would plausibly include any of those fixes at the time of that release.
3. Do you need major version branches? i.e. `1.x` and `2.x`?
	* If you don't have one, and you want to add a new feature to an older release major, you'll have to checkout the branch and then create a branch from that before you tag. For example:
		* `git checkout 1.2.x`
		* `git checkout -b 1.3.x`
		* `git cherry-pick <some feature commit from master>`
	* An alternative is to not allow new backwards-compatible feature work to older major versions. For example, once breaking changes have merged to master, you're locking the minor versions of the previous release:
		* Master is effectively `2.0.x`, and as a result, `1.2.x` is the highest minor version that `1.x` will ever have. This removes the need for major version branches, but does place an expectation on understanding if master is a major+1 or minor+1 from the most recent tag on master.
