# `pr`
`pr` is a compatible command line tool for creating, updating, accepting, and landing pull github pull requests in a verifiably secure way.

`pr` is a bash script which currently supports both bash and zsh.

# Installation

## Install Dependencies
Before installing, ensure that the following binaries are available:
* git
* openssl
* xargs
* gpg

These packages can be installed from most package managers if needed (eg, brew, apt, etc).

### Mac OS X
On Mac OS X, a few additional packages are required:
* gnu-getopt
* gdate

To install these, run `brew install gnu-getopt coreutils`.

## Install `pr`

Run the following command to install `pr` into `/usr/local/bin`:

`wget -q -O /usr/local/bin/pr https://raw.githubusercontent.com/tylerlevine/pr/master/pr`

Make it executable:

`chmod u+x /usr/local/bin/pr`

Now running the `pr` command should print the usage information:

```
$ pr
pr <command> [<args>]

Commands
--------

* pr init
Setup your repository for usage with pr

* pr sync
Create or update a github pull request from a branch

* pr merge
Merge a github pull request which has been accepted

* pr accept
Accept a github pull request and apply review signature

* pr show
Show review signatures for the current HEAD

* pr verify
Verify that the current HEAD has at least one valid review signature relative to master

* pr import
Import public keys from .reviewers into local keychain

* pr version
Report the version number
```

# Basic Usage
The first step in making a new change is to create a new feature branch where you will be working.

```
git checkout -b my-feature
```

Then you can make a few commits to the `my-feature` branch to implement your change.

## Creating a pull request
Once you are ready to create a pull request from this branch for review, run the sync command.

```
pr sync
```

This will prompt you for the pull request message, then push your branch to the remote repository, and create a pull request against `master`.

You can then send the pull request URL to another engineer for review.

## Accepting a pull request

When someone has sent you a pull request URL for review, the first step is looking at the changes in github. Once you are ready to accept the pull request,
run the accept command and pass the pull request number.

```
pr accept 12
```

This will create a review signature, push the review signature to the remote repository, and mark the pull request as accepted with a comment.

## Merging a pull request

Once you have your pull request accepted by another engineer, you can run the merge command to incorporate your change into the upstream branch.

```
pr merge
```

This will squash all the commits on the `my-feature` branch, and then rebase the squashed commit on the latest version of `master`. You will have a change to edit the resulting commit message in your editor before the commit is made.

Additionally, the review signatures which were applied to the `my-feature` branch will be copied onto the new commit in `master`. The new version of `master` is then pushed to the remote repository, and the `my-feature` branch is deleted from both the remote and local repositories.

# Command Reference

## `pr init`
Setup local repository for usage with `pr`.

## `pr sync`
Create or update a github pull request from a local branch.

## `pr merge`
Merge a github pull request which has been accepted.

## `pr accept`
Accept a github pull request and apply a review signature.

### Options
#### --key
Specify which gpg key should be used when creating review signature.

#### --no-push
Do not push the review signature to the remote repository.

#### --upstream
Override the upstream branch of the pull request. Defaults to the target branch which the pull request will be merged into.

#### --sigonly
Skip marking the pull request as accepted in github. This will only cause a review signature to be created and pushed to the remote repository (unless `--no-push` is used, in which case the review signature will only exist in your local repository).

## `pr show`
Print review signature(s) for a commit range.

### Options
**--raw**
Skip printing headers and labels. This may be useful if consuming review signatures programatically.

**--from**
Specify the ref which represents the beginning of the commit range. Defaults to the pull request base branch.

**--to**
Specify the ref which represents the end of the commit range. Defaults to `HEAD`.

## `pr verify`
Verify review signature(s) for a commit range.

### Options
**--min-count**
Minimum number of review signatures required for acceptance of a commit range. Defaults to 1.

**--from**
Specify the ref which represents the beginning of the commit range. Defaults to `HEAD^`.

**--to**
Specify the ref which represents the end of the commit range. Defaults to `HEAD`.

**--trustdb**
Specify the gpg trustdb which review signatures should be evaluated against. Defaults to `.reviewers`.

## `pr import`
Import public keys from repository trustdb into local gpg keychain.

### Options
**--trustdb**
Specify the gpg trustdb from which reviewer public keys should be imported. Defaults to `.reviewers`.

**--keyserver**
Specify the gpg keyserver from which reviewer public keys should be retrieved. Defaults to `hkp://keyserver.ubuntu.com:80`.

## `pr version`
Show `pr` version information.

# License

Apache 2.0