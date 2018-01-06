![oneflow](https://user-images.githubusercontent.com/544444/32192165-aad6f8e4-bdb3-11e7-89ea-c28c20fcd04c.png)

## Overview

Helpful commands to help with git oneflow, based on this [blogpost](http://endoflineblog.com/oneflow-a-git-branching-model-and-workflow) in a response to gitflow.

The tooling in this repo is based on feature branch scenario 3 (rebase + no ff), with the additional feature of `issue` branches. While identical to feature branches, they've basically got some additional syntactic sugar to easily identify the bug id and auto link and resolve it when pushing to origin. A small variation has been added for the feature branches. Instead of merging to master, they will follow the same flow and open the git repo url so you can start a pull request instead.

## Install

Simple installation procedure, which should place git-flow and other scripts to /usr/local/bin dir.
```
wget --no-check-certificate -q -O - https://github.com/euknyaz/oneflow/raw/master/git-oneflow-update.sh | sudo bash
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
