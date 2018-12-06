# pr
`pr` is a command line tool for creating, updating, accepting, and landing pull github pull requests.

# Installation

# Basic Usage

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