---
categories: [tech, Dev]
tags: [git]
---

### check

```shell
git remote -v
> origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
> origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
```

### add remote upstream

If your fork don't have upstream repository, you should add it:
```shell
git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
```

Then recheck.
```shell
git remote -v
```


### merge upstream to your fork
```shell
git fetch upstream
git merge upstream/master
```


### update your fork
```shell
git push origin master
```
