## [Git 删除分支指南](https://blog.csdn.net/m0_69824302/article/details/137170659)

在 Git 中，删除分支是清理代码库、保持分支列表整洁的重要步骤。分支删除分为**本地分支**和**远程分支**两种情况。

**示例：删除本地与远程分支**

```bash
# 1. 删除本地分支（已合并）
git branch -d feature/login

# 2. 强制删除本地分支（未合并）
git branch -D feature/login

# 3. 删除远程分支
git push origin --delete feature/login
```

**删除本地分支**

- 使用 *git branch -d <branch>* 删除已合并到当前分支的本地分支。
- 如果该分支未合并且你确定不再需要，可用 *git branch -D <branch>* 强制删除。
- 注意：不能删除当前所在的分支，需先 *git checkout* 切换到其他分支。

**示例**

```bash
# 切换到 master 分支
git checkout master

# 删除已合并的 test 分支
git branch -d test

# 强制删除未合并的 dev 分支
git branch -D dev
```

**删除远程分支**

- 使用 *git push <remote> --delete <branch>* 删除远程仓库中的指定分支。
- 常见 *<remote>* 名称为 *origin*。
- 删除后可用 *git fetch -p* 清理本地的远程分支引用。

**示例**
```bash
# 删除远程 origin 上的 feature/api 分支
git push origin --delete feature/api

# 同步更新本地远程分支列表
git fetch -p
```

**注意事项**

- **谨慎使用强制删除**：*-D* 会直接移除未合并的提交，无法恢复。
- 删除远程分支不会影响已存在的本地副本，需手动删除对应本地分支。
- 在多人协作中，建议先确认团队成员不再使用该分支再执行删除操作。