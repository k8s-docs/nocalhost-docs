---
title: Pattern
---

## 描述

如果某些文件应与其他设备同步或不同步, 配置相应`SyncFilePattern`/`IgnoreFilePattern`.
所有模式均相对于文件夹根 (进入`DevMode`时选择的文件夹).

!!! danger "`IgnoreFilePattern`的优先级高于`SyncFilePattern`"

    因此，如果您的模式都涵盖了同一文件，则该文件将被忽略。

## 模式语法

- **`foo`(常规文件名)** 与自己匹配, 即模式`foo`匹配文件`foo`, `subdir/foo` as well as any directory named `foo`. Spaces are treated as regular characters, except for leading and trailing spaces, which are automatically trimmed.

- **`*`(星号)** matches zero or more characters in a filename, but does not match the directory separator. `te*ne` matches `telephone`, `subdir/telephone` but not `tele/phone`.

- **` ** `(双星号)** matches as above, but also directory separators. `te\*\*ne`matches`telephone`, `subdir/telephone`and`tele/sub/dir/phone`.

- **`?`(问号)** matches a single character that is not the directory separator. `te??st` matches `tebest` but not `teb/st` or `test`.

- **`[]`(方括号)** denote a character range: `[a-z]` matches any lower case character.

- **`{}`(大括号)** denote a set of comma separated alternatives: `{banana,pineapple}` matches either `banana` or `pineapple`.

- **`\`(反斜线)** “escapes” a special character so that it loses its special meaning. For example, `\{banana\}` matches `{banana}` exactly and does not denote a set of alternatives as above. _Escaped characters are not supported on Windows._

- **`/`、`./`(斜杠)** 仅在文件夹根部开始的匹配模式。 `/foo` 或 `./foo` 匹配 `foo` 但不匹配 `subdir/foo`.

- **`(?i)`(不敏感)** 前缀开头的模式可以实现对案例不敏感的模式匹配。 `(?i)test` 匹配 `test`, `TEST` 和 `tEsT`. The `(?i)` prefix can be combined with other patterns, for example the pattern `(?i)picture*.png` indicates that `Picture1.PNG` should be synchronized. On Mac OS and Windows, patterns are always case-insensitive.

!!! info "可以按任何顺序指定前缀 (如 “(?i){foo,bar}/\*/bar”), 但不能单一的括号 (不是 “{foo,(?i),bar}/\*/bar”)."

## 例子

给定目录布局：

```shell
.DS_Store
foo
foofoo
bar/
    baz
    quux
    quuz
bar2/
    baz
    frobble
My Pictures/
    Img15.PNG
nocalhost/
    hello
    test/
    team/
```

以及以下配置：

```yaml
SyncFilePattern:
  - frobble
  - quuz
  - ./nocalhost

IgnoreFilePattern:
  - foo
  - *2
  - qu*
  - (?i)my pictures
  - nocalhost/t**
```

`IgnoreFilePattern`的优先级高于`SyncFilePattern`，最终结果变为：

```shell
foo           # ignored, matches IgnoreFilePattern "foo"
foofoo        # synced, does not match IgnoreFilePattern "foo", but would match "foo*" or "*foo"
bar/          # synced, no such config, so synced
    baz       # synced, no such config, so synced
    quux      # ignored, matches IgnoreFilePattern "qu*"
    quuz      # ignored, though specify the SyncFilePattern "quuz",but matches higher level IgnoreFilePattern "qu*"
bar2/         # ignored, matches IgnoreFilePattern "*2"
    baz       # ignored, due to dir parent being ignored
    frobble   # ignored, due to dir parent being ignored
My Pictures/  # ignored, matched IgnoreFilePattern case insensitive "(?i)my pictures" pattern
    Img15.PNG # ignored, due to parent being ignored
nocalhost/    # synced, no such config, so synced
    hello     # synced, no such config, so synced
    test/     # ignored, matches IgnoreFilePattern "nocalhost/t**"
    team/     # ignored, matches IgnoreFilePattern "nocalhost/t**"
```
