---
layout: post
title: Git - Commit Message Guidelines 提交消息準則
category: architecture
tags: [git]
---

```console
type (scope): subject
BLANK LINE
body
BLANK LINE
footer
```

<br>

subject & body:
- First line is 50 characters or less
- Then a blank line
- Remaining text should be wrapped at 72 characters

Ref: [50/72 formatting](https://stackoverflow.com/questions/2290016/git-commit-messages-50-72-formatting)

## Header

### type(scope): subject

type:
- feat: A new feature
- fix: A bug fix
- docs: Documentation only changes
- style: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
- refactor: A code change that neither fixes a bug nor adds a feature
- perf: A code change that improves performance
- test: Adding missing tests or correcting existing tests
- build Changes that affect the build system, CI configuration or external dependencies (example scopes: gulp, broccoli, npm)
- chore: Other changes that don't modify src or test files

### scope

The scope should be the name of the npm package affected (as perceived by the person reading the changelog generated from commit messages).

none/empty: `style`, `test`, `refactor`, `docs`

### subject

add ~.md

## Body

commit reason, why change

## Footer

- 不兼容的变动：与上一个版本不兼容，则 Footer 部分以 `BREAKING CHANGE` 开头
- 关闭 Issue：commit 针对某个 issue，在 Footer 中可以写上 `Closes #123`

## Reference

- [angular/CONTRIBUTING.md at master · angular/angular](https://github.com/angular/angular/blob/master/CONTRIBUTING.md)
- [Git Commit message 编写规范与那些年我写过的奇怪 message - 忘归](http://jalan.space/2019/04/24/2019/git-commit-message/)
- [Semantic commit type when remove something - Stack Overflow](https://bit.ly/2ETksNT)

---