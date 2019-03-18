---
categories: [tech, Dev]
tags: [git]
---

# hub

We should use the command line tool **hub** of github

[download hub here](https://github.com/github/hub/releases)

# pull request usage

```
Usage: hub pull-request [-focp] [-b <BASE>] [-h <HEAD>] [-r <REVIEWERS> ] [-a <ASSIGNEES>] [-M <MILESTONE>] [-l <LABELS>]
       hub pull-request -m <MESSAGE> [--edit]
       hub pull-request -F <FILE> [--edit]
       hub pull-request -i <ISSUE>
```
For instance:
```
hub pull-request -b <remote origin>:<branch> -h <local branch>
hub pull-request -b rustlang-cn:master -h master
```

# text editor

If we haven't set and text editor, terminal may report:
```
error using text editor for pull request message
```
How to set:
```shell
git config --global core.editor subl
```
You can use subl, vim or others.
