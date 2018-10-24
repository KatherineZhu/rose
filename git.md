## Git

### Find diff commit between branch and author

```
git rev-list master..develop --reverse --no-merges --author="Jonathan Bai" --author="Katherine Zhu" --author="Zach Zhang" --author="Judith Huang"
```

- `rev-list` can replace by `log` also
- append `| xargs -n 1 git cherry-pick` can do multiple cherry-pick，should use `--reverse` option
- `--no-merges` will ignore merge commit

### Show git log history for a sub directory

```
git log -- {path}
```
