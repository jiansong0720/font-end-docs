# Git Tool

[TOC]

## 1. commit

### 1.1 [Commitizen](https://github.com/commitizen/cz-cli)

> commit 帮助生成符合规范的commit

- 全局安装

  ```
  npm install -g commitizen
  ```

- 项目安装

  ```
  commitizen init cz-conventional-changelog --save --save-exact
  ```

- 使用

  ```
  git commit 替换为 git cz
  ```

- 效果

  ```
  git cz

  Line 1 will be cropped at 100 characters. All other lines will be wrapped after 100 characters.

  ? Select the type of change that you're committing: (Use arrow keys)
  > feat:     A new feature
    fix:      A bug fix
    docs:     Documentation only changes
    style:    Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
    refactor: A code change that neither fixes a bug nor adds a feature
    perf:     A code change that improves performance
    test:     Adding missing tests or correcting existing tests
  (Move up and down to reveal more choices)
  ```

  ​

### 1.2 [validate-commit-msg](https://github.com/conventional-changelog-archived-repos/validate-commit-msg) 

> 校验commit 是否符合规范

- 安装

```
npm install --save-dev validate-commit-msg
```

- 配置

```
新增 .vmrc 配置文件
git-hook
```

- 效果

```
git commit 'jartto:fix bug'
INVALID COMMIT MSG: does not match "<type>(<scope>): <subject>" !
jartto:fix bug
```



### 1.3 [Conventional Changelog](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fconventional-changelog%2Fconventional-changelog) 

> 生成commit change log

- 安装

```
npm install -g conventional-changelog-cli
```

- 生成

```
conventional-changelog -p angular -i CHANGELOG.md -w
```



