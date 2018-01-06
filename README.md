![oneflow](https://user-images.githubusercontent.com/544444/32192165-aad6f8e4-bdb3-11e7-89ea-c28c20fcd04c.png)

## Overview

Helpful commands to help with git oneflow, based on this [blogpost](http://endoflineblog.com/oneflow-a-git-branching-model-and-workflow) in a response to gitflow.

The tooling in this repo is based on feature branch scenario 3 (rebase + no ff), with the additional feature of `issue` branches. While identical to feature branches, they've basically got some additional syntactic sugar to easily identify the bug id and auto link and resolve it when pushing to origin. A small variation has been added for the feature branches. Instead of merging to master, they will follow the same flow and open the git repo url so you can start a pull request instead.

## Install

Simple installation procedure, which should place git-flow and other scripts to /usr/local/bin dir.
```
wget --no-check-certificate -q -O - https://github.com/euknyaz/oneflow/raw/master/git-oneflow-update | sudo bash
```
## Usage

To start using one flow you can simply type 
```sh
$ git flow
``` 
and read usage info:
```
usage: git flow <subcommand>

Available subcommands are:
   help - help info.
   feature <name> - create feature branches.
   release <version> [startingCommit] - create release branch for <version> based on <statingCommit>.
   hotfix <version> [tag] - Create hotfix version based on provided tag or latest tag.
   issue <issue-id> - create issue branch for issue-id.
   finish  [annotation] - finish work on feature/release/hotfix branches.
   latest - checkout master branch.
   latest-tag - checkout latest tag from master branch.
   open - open browser with git project url.

Try 'git flow <subcommand> help' for details.
``` 

- Starting a workflow
```sh
 $ git flow feature <name>
 $ git flow issue <id>
 $ git flow release  <version> [startingCommit]
 $ git flow hotfix <version> [tag]
 ```
 
 - Ending that workflow
 ```sh
 $ git flow finish [annotation]
 ```
 
 - Useful tooling during said workflow
 ```sh
 $ git latest
 $ git latest-tag
 
 $ git open
 
 $ git oneflow-update
 $ git oneflow-version
```

You can pass `--h` for a short readme, as follows:

```sh
$ git hotfix --h
```

_( Where <> is required, [] is optional )_


## Use Cases

* I need to create my first **major release** v1.0.0
```bash
git flow release 1.0.0

# i'm in release/1.0.0 branch now
# with tag v1.0.0-rc (release candidate)

# I do some work with release
git commit -m "First commit" [some-file]
# git describe version: v1.0.0-rc-1-g<commitid1-id>
git commit -m "Second commit" [some-file]
# git describe version: v1.0.0-rc-2-g<commitid2-id>
git cherry-pick [some-master-commit-id]
# git describe version: v1.0.0-rc-3-g<commitid3-id>

git flow finish
# Added tag v1.0.0
# My changes merged to master (with tag v1.0.0)
# Switched to master
# Asked if I want to remove release/1.0.0 branch
# It's ok to remove as soon as we have tag in master and can restore at any time
```

* I need to create **minor release** v1.1.0
```bash
# creating release from v1.0.0 tag
git flow release 1.1.0 v1.0.0

# i'm in release/1.1.0 branch now
# with tag v1.1.0-rc (release candidate)

# I do some work with release
git commit -m "First commit" [some-file]
# git describe version: v1.1.0-rc-1-g<commitd1-id>
git commit -m "Second commit" [some-file]
# git describe version: v1.1.0-rc-1-g<commitd1-id>
git cherry-pick [some-master-commit-id]
# git describe version: v1.1.0-rc-1-g<commitd1-id>

git flow finish
# Added tag v1.1.0
# My changes merged to master (with tag v1.1.0)
# Switched to master
# Asked if I want to remove release/1.1.0 branch
# It's ok to remove as soon as we have tag in master and can restore at any time
```

* I need to create **hotfix** v1.1.1 (the last number represents id of hotfix)
```bash
# creating release from v1.1.0 tag
git flow hotfix 1.1.1 v1.1.0

# i'm in release/1.1.1 branch now
# but with previous version tag v1.1.0

# I do some work with release
git commit -m "First commit" [some-file]
# git describe version: v1.1.0-1-g<commitid1-id>
git commit -m "Second commit" [some-file]
# git describe version: v1.1.0-2-g<commitid2-id>
git cherry-pick [some-master-commit-id]
# git describe version: v1.1.0-3-g<commitid3-id>

git flow finish
# Added tag v1.1.1
# My changes merged to master (with tag v1.1.1)
# Switched to master
# Asked if I want to remove release/1.1.1 branch
# It's ok to remove as soon as we have tag in master and can restore at any time
```

* I need to create **feature** "payouts"
```bash
# creating feature from master branch always
git flow feature payouts

# i'm in feature/payouts branch now
# current build version calculated as v1.1.1-payouts (v1.1.1 is latest tag in master)
# I have tag v1.1.1-payouts added to the first commit in branch

# I do some work with release
git commit -m "First commit" [some-file]
# git describe version: v1.1.1-payouts-1-g<commitid1-id>
git commit -m "Second commit" [some-file]
# git describe version: v1.1.1-payouts-2-g<commitid2-id>
git cherry-pick [some-master-commit-id]
# git describe version: v1.1.1-payouts-3-g<commitid3-id>

git flow finish
# Running rebase -i master
# Asked what I'd prefer merge or pull-request
# My changes merged to master (without a new tag)
# Switched to master
# Asked if I want to remove feature/payouts branch
# It's ok to remove as soon as we merge record in master and can restore at any time
```
