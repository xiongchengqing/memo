---
title: git bisect
date: '2019-01-14'
spoiler: Simple way to find the issue where it is
---

Sometimes we want to locate a line of code which introduced a wired bug, but don't know which commit the bad code was introduced at the first place. Sometimes we want to know a short life cycle feature in the code base, but don't know what the code looks like, and the only impression of the feature is some broken words about the feature. Nomarlly we can solve it stupidly check from the first commit to the lastest one. THAT IS TOO SLOW, MAN! Imagine that if your codebase has hundreds of commits. You will do it all day to find the exact commit indtroduced the bug/feature at the first place.

Well, the good news is `git` gives us an easy way to do it now. If you know how binary search works, the `git bisect` is there for you.

First, you need to define a query arrange like this. For me there are total **1549** commits in current branch. It's 😱 to find buggy codes from commit to commit.

![A real project's commit count](https://img.xiongc.com/jszealer/v83di.png)

Scary it is, right?

```bash
git bisect start START_COMMIT END_COMMIT
```

when you run that git cmd, an intermediate commit will be checkedout. Okay, start to find the code involved bugs/features. Often START_COMMIT is HEAD and END_COMMIT is the initial commit.

If you don't want to scroll down to the bottom of terminal grep the initial commit. Here is quick cmd to get it.

```bash
 git rev-list --max-parents=0 HEAD --oneline
```

It looks like this when you run the `git bisect start`.

![switch to the intermediate commit](https://img.xiongc.com/jszealer/5w6qv.png)

Okay then start to review yor code, assuming that you were not luckily found it. Mark it as `good`, it indicates this version of code works as expected. Mark it as `bad`, congrats you, bad code is there.

```bash
git bisect good
```

![mark good to switch to the intermediate commnit between last commit and HEAD](https://img.xiongc.com/jszealer/0hlo0.png)

You may noticed the content in the parentheses, `roughly 1 step` means max steps you probably take to find the tagrget code. Bingo! Finally I catch the bad code. Bug/feature finding job is finished, then reset to origin commit where before the `git bisect`.

```bash
git bisect reset
```

![reset to before start bisect](https://img.xiongc.com/jszealer/ksgcd.png)

Now, you may wonder if bug catching is not smoothly as what I was showing.  Below graph is more clearly for you understand what is going on when you mark it as `good/bad`.

