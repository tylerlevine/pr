# `pr`
`pr` is a CLI tool for creating, updating, approving, and landing github pull requests.

# Installation

## Install Dependencies
Before installing, ensure that the following binaries are available:
* git
* hub

These packages can be installed from most package managers if needed (eg, brew, apt, etc).

### Mac OS X
On Mac OS X, one additional package is required:
* gnu-getopt

To install, run `brew install gnu-getopt`.

## Install `pr`

Run the following command to install `pr` into `/usr/local/bin`:

`wget -q -O /usr/local/bin/pr https://raw.githubusercontent.com/tylerlevine/pr/master/pr`

Make it executable:

`chmod u+x /usr/local/bin/pr`

Now running the `pr` command should print the usage information:

```
pr <command> [<args>]

Commands
--------

* pr sync
  Create or update a github pull request from a local branch

* pr merge
  Merge a github pull request which has been approved

* pr approve [--nopush] [--tag-only]
  Approve a github pull request and create review tag

* pr show [--raw]
  Show review tags for the current HEAD

* pr verify
  Verify that the current HEAD has at least the minimum number of valid review tags

* pr version
  Print the current version
```

# Basic Usage
The first step in making a new change is to create a new feature branch where you will be working.

```
git checkout -b my-feature
```

Then you can make a few commits to the `my-feature` branch to implement your change.

## Creating a pull request
Once you are ready to create a pull request from this branch for review, run the `pr sync` command.

This will prompt you for the pull request message, then push your branch to the remote repository, and create a pull request against `master`.

```
$ pr sync
No open pull request found for local branch my-feature
Create a new pull request? [Y/n]
Creating new PR for branch my-feature...
Updating out of date remote branch origin/my-feature...
Pull request successfully created: https://github.com/BitGo/shared-review-flow-test-repo/pull/6
```

You can then send the pull request URL to another engineer for review.

## Approving a pull request

When someone has sent you a pull request URL for review, the first step is looking at the changes in github. Once you are ready to approve the pull request,
run the approve command and pass the pull request number.

```
pr approve 12
```

This will mark the pull request as approved, create a review tag, and push the review tag to the remote repository.

## Merging a pull request

Once you have your pull request approved by another engineer, you can run the merge command to incorporate your change into the upstream branch.

First, checkout the branch you want to merge:
```
git checkout my-feature
```

Then run the `pr merge` command to merge the local branch into the upstream branch (usually master).

```
$ pr merge
Merging branch my-feature (pull request #3) into branch master...
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
From github.com:BitGo/shared-review-flow-test-repo
 * branch            master     -> FETCH_HEAD
Already up to date.
Updating ffcd477..b38eb54
Fast-forward
Squash commit -- not updating HEAD
 README.md | 1 +
 1 file changed, 1 insertion(+)
[master a87b310] Make change to readme
 Date: Mon Dec 10 15:40:12 2018 -0800
 1 file changed, 1 insertion(+)
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 1018 bytes | 1018.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:BitGo/shared-review-flow-test-repo.git
 - [deleted]         my-feature
   ffcd477..a87b310  master -> master
Deleted branch my-feature (was b38eb54).
```

This will squash all the commits on the `my-feature` branch, and then rebase the squashed commit on the latest version of `master`. You will have a chance to edit the resulting commit message in your editor before the commit is made.

Additionally, the review tags which were applied to the `my-feature` branch will be copied onto the new commit in `master`. The new version of `master` is then pushed to the remote repository, and the `my-feature` branch is deleted from both the remote and local repositories.

# Command Reference

## `pr sync`
Create or update a github pull request from a local branch.

## `pr merge`
Merge a branch for a github pull request which has been approved.

## `pr approve <pr number>`
Approve a github pull request and create review tag.

### Arguments
#### `<pr number>`: pull request number to approve

### Options
#### --nopush
Do not push the review signature to the remote repository.

#### --tag-only
Skip marking the pull request as approved in github. This will only cause a review tag to be created and pushed to the remote repository (unless `--nopush` is used, in which case the review tag will only exist in your local repository).

## `pr show`
Show review tag(s) for the current HEAD.

### Options
**--noheader**
Skip printing headers and labels. This may be useful if consuming review tags programatically.


## `pr verify`
Verify review signature(s) for a commit range.

### Options
**--min-count**
Minimum number of review signatures required for approveance of a commit range. Defaults to 1.

## `pr version`
Show `pr` version information.

# Supported Shells

`pr` currently supports both bash and zsh.

# License

Apache 2.0