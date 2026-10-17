下面按 Markdown 笔记的形式整理，既包含 Git 的核心知识，也把你提出的问题和对应答案纳入笔记，方便以后复习。

# Git 基础操作学习笔记

## 一、Git 是什么？

Git 是一个**版本控制工具**，可以记录项目文件的修改历史，方便查看不同版本之间的差异，以及在需要时恢复以前的版本。

例如，编写一个 Python 程序：

- 第一次提交：完成基础代码。
- 第二次提交：新增功能 B。
- 第三次提交：新增功能 C。

Git 可以保存这三次提交的版本记录。

需要区分：

- **保存文件**：将代码保存到电脑。
- **Git 提交**：将文件的某个版本记录到 Git 历史中。
- **推送代码**：将本地的提交上传到 GitHub。

## 二、初始化 Git 仓库

### 1. `git init` 有什么用？

```
git init
```

作用：初始化当前文件夹，使其成为 Git 仓库。

执行后，Git 会自动创建隐藏的 `.git` 文件夹，用来保存版本控制所需的信息。

注意：

- `git init` 只完成初始化，不会自动记录每次修改。
- `.git` 文件夹不需要手动管理。
- `git init` 不会自动将项目上传到 GitHub。

### 2. 为什么初始化后还不能直接查看修改历史？

因为初始化只是建立了版本管理环境，并没有创建新的提交记录。

例如：

1. 执行 `git init`。
2. 修改 `index.html`。
3. 按 `Ctrl + S` 保存文件。

此时，文件已经保存到电脑，但还没有因此产生新的 Git 提交记录。

## 三、保存修改与提交版本

### 1. `Ctrl + S`、`git add` 和 `git commit` 有什么区别？

| 操作           | 作用                |
| ------------ | ----------------- |
| `Ctrl + S`   | 将编辑器中的修改保存到电脑文件   |
| `git add`    | 将指定文件的变化放入暂存区     |
| `git commit` | 将暂存区中的内容创建为一条提交记录 |

### 2. 必须先保存文件，再执行 `git add` 和 `git commit` 吗？

严格来说，先按 `Ctrl + S` 并不是 Git 的绝对要求。

但是，Git 通常根据磁盘上已经保存的文件内容来暂存修改，而不是直接读取编辑器中尚未保存的内容。

因此，初学时推荐按以下顺序操作：

```
# 第一步：在 VS Code 中按 Ctrl + S

# 第二步：暂存修改
git add -A

# 第三步：提交版本
git commit -m "修改网页标题"
```

这样可以避免误以为尚未保存的代码已经被提交。

### 3. `git add index.html` 是什么意思？

```
git add index.html
```

表示将 `index.html` 的变化放入暂存区。

**问题：我输入 git add 的时候，Git 也没让我选择啊？**

答案：因为你已经在命令中指定了文件名。Git 会按照命令执行，不需要再弹出选择界面。

例如：

```
git add index.html
```

暂存指定文件。

```
git add style.css
```

暂存另一个文件。

```
git add .
```

暂存当前目录下符合条件的变化。

### 4. `git add -A` 是什么意思？

```
git add -A
```

`-A` 表示暂存仓库范围内的所有变化，包括：

- 已跟踪文件的修改。
- 已跟踪文件的删除。
- 符合条件的新文件。

通常，被 `.gitignore` 规则忽略的文件不会被添加。

### 5. `git commit -m` 是什么意思？

```
git commit -m "feat: login-register"
```

各部分含义：

- `git commit`：创建一条提交记录。
- `-m`：表示后面跟着提交说明。
- `"feat: login-register"`：本次提交的描述文字。

提交说明应该尽量清楚地描述本次修改，例如：

```
git commit -m "feat: add login feature"
git commit -m "fix: correct page layout"
git commit -m "docs: update README"
```

### 6. 提交说明会在哪里显示？

提交说明会出现在：

- VS Code 的提交历史中。
- GitHub 仓库的提交历史中（提交成功推送后）。

它不会自动显示在博客网页上。

### 7. 每次修改都需要提交吗？

不需要每修改一行代码就提交。

可以在完成一个相对完整的功能或修改阶段后，再执行：

```
git add -A
git commit -m "本次修改说明"
```

每次成功提交都会产生一条提交记录。

例如：

```
第一次提交：first try
第二次提交：B
第三次提交：C
```

如果 B 和 C 都分别提交成功，Git 通常会保留这两条记录。GitHub 文件列表显示的最新提交说明不一定代表之前的提交消失了。

## 四、分支的概念与操作

### 1. 什么是分支？

分支可以理解为项目的一条独立开发线。

例如：

- `main`：主分支。
- `master`：另一个分支名称。
- `B`：自己创建的功能分支。

不同分支可以有不同的提交历史和文件内容。

### 2. `main` 和 `master` 是什么关系？

`main` 和 `master` 都是普通的分支名称，没有谁天然更高级。

它们可能同时存在于同一个仓库中。

注意：

- 两个分支不会自动同步。
- 修改 `master` 不代表 `main` 也会发生变化。
- `main` 旁边的 `default` 表示它是 GitHub 仓库的默认分支，并不是特殊的分支类型。

### 3. 为什么会出现 `master` 和 `main` 不一致？

不同 Git 环境可能使用不同的默认分支名称。

例如，初始化后第一次提交时，本地分支是 `master`，但推送时输入：

```
git push -u origin main
```

就可能出现：

```
error: src refspec main does not match any
```

原因可能是本地不存在 `main` 分支。

可以先查看分支：

```
git branch
```

如果当前分支是 `master`，而且远程仓库地址设置正确，可以使用：

```
git push -u origin master
```

### 4. `git branch` 是什么？

```
git branch
```

查看本地分支。

```
git branch B
```

创建一个名为 `B` 的新分支。

**问题：创建分支后，之后的修改会自动进入新分支吗？**

答案：不会。`git branch B` 只负责创建分支，不会自动切换过去。

例如：

1. 当前位于 `main`。
2. 执行 `git branch B`。
3. 仍然位于 `main`。
4. 此时提交的修改仍然会记录在 `main` 上。

需要切换后，才能在 `B` 上继续开发。

### 5. `git checkout` 是什么？

```
git checkout B
```

切换到 `B` 分支。

```
git checkout main
```

切换到 `main` 分支。

```
git checkout master
```

切换到 `master` 分支。

新版本 Git 也可以使用：

```
git switch B
git switch main
```

`git checkout` 还有其他用途，但这里先掌握分支切换即可。

### 6. 修改和提交会记录在哪个分支？

通常，**新提交会接在提交时当前所在的分支上**。

例如：

```
git checkout B
git add -A
git commit -m "modify B"
```

这次提交会记录在 `B` 分支上。

需要注意，工作区和暂存区的修改不一定只属于某一个分支；如果切换分支时存在未提交的修改，Git 可能保留它们，也可能因冲突或覆盖风险而拒绝切换。

### 7. 如何删除分支？

```
git checkout main
git branch -d B
```

先切换到其他分支，再删除 `B`。

- `-d`：安全删除分支；如果分支还有未合并的提交，Git 通常会拒绝删除。
- `-D`：强制删除分支，即使其中存在尚未合并的提交，也可能将其从分支引用中移除。

强制删除前，应确认是否还需要其中的提交。

## 五、分支合并

### 1. `git merge` 是什么？

`git merge` 用于将一个分支的提交历史合并到当前分支。

例如，将 `B` 合并到 `main`：

```
git checkout main
git merge B
```

### 2. 合并前需要先 `add` 和 `commit` 吗？

推荐先提交修改，再合并。

如果 `B` 上的修改已经提交，就不需要为了合并而重复执行 `add` 和 `commit`。

例如：

```
git checkout B
git add -A
git commit -m "finish feature B"

git checkout main
git merge B
```

### 3. 必须先切换到被合并的分支吗？

不需要。

**要先切换到接收合并内容的分支。**

例如，要把 `B` 合并到 `main`：

```
git checkout main
git merge B
```

这里：

- `git checkout main`：切换到接收修改的分支。
- `git merge B`：把 `B` 合并到当前分支 `main`。

如果你在 `B` 分支执行 `git merge main`，方向就反过来了：尝试把 `main` 合并到 `B`。

## 六、GitHub 远程仓库

### 1. `git remote add origin` 是什么？

```
git remote add origin https://github.com/用户名/仓库名.git
```

作用：给本地 Git 仓库添加远程仓库地址。

- `git remote add`：添加远程仓库。
- `origin`：给远程仓库起的名称，通常使用这个默认名称。
- URL：GitHub 仓库的实际地址。

这一步只建立关联，不会上传代码。

### 2. 每个新仓库都要重新关联吗？

每个独立的本地 Git 仓库，通常都需要设置自己的远程仓库地址。

但如果你已经在同一台电脑上完成 GitHub 身份验证，通常不需要每个仓库都重新登录。

### 3. `git push -u origin main` 是什么？

```
git push -u origin main
```

作用：将本地 `main` 分支上的提交推送到名为 `origin` 的远程仓库，并设置上游分支关联。

- `git push`：推送提交。
- `-u`：设置上游分支关联。
- `origin`：远程仓库名称。
- `main`：本地要推送的分支。

如果本地分支叫 `master`，则可以使用：

```
git push -u origin master
```

第一次成功设置上游关联后，后续通常可以直接使用：

```
git push
```

### 4. `git push` 和 `git commit` 有什么区别？

| 命令           | 作用           |
| ------------ | ------------ |
| `git commit` | 在本地创建提交记录    |
| `git push`   | 将本地提交上传到远程仓库 |

如果只执行 `git commit`，提交记录可能还没有上传到 GitHub。

### 5. 为什么 GitHub 只显示了最新的提交说明？

GitHub 仓库首页通常展示当前分支的最新提交信息。

如果想查看过去的提交，例如 `B` 和 `C`：

1. 先确认当前查看的是正确分支。
2. 点击文件列表附近的提交记录数量。
3. 查看提交历史。

如果两次修改都分别成功提交并推送，通常可以在对应分支的提交历史中看到它们。

### 6. `Compare & pull request` 是什么？

这是 GitHub 上的比较与拉取请求入口。

它可以比较两个分支的差异，并发起合并请求。

例如，请求将 `master` 分支合并到 `main` 分支。

个人练习项目并不一定需要使用它。如果只是上传代码，不需要为了推送而创建 Pull Request。

## 七、克隆远程仓库

### 1. `git clone` 是什么？

```
git clone https://github.com/用户名/仓库名.git
```

作用：将远程仓库复制到本地电脑。

Git 通常会：

- 创建一个以仓库名命名的文件夹。
- 下载仓库中的文件。
- 获取相关 Git 版本历史。
- 配置远程仓库地址，通常命名为 `origin`。

### 2. 克隆后下载到哪里？

**默认下载到你执行命令时所在的目录下。**

例如，终端当前目录是：

```
E:\code\git-learn
```

执行：

```
git clone https://github.com/Julian-yulijin/Lotia.git
```

通常会得到：

```
E:\code\git-learn\
├── learn.py
└── Lotia\
    ├── ...
    └── .git\
```

`Lotia` 是新克隆下来的项目，不会自动与 `git-learn` 合并。

### 3. 什么时候需要 `git clone`？

- 换电脑后，想继续开发之前的项目。
- 想下载别人的 GitHub 项目进行学习。
- 本地项目丢失，但远程仓库仍然存在。
- 想在本地获得一个远程仓库的副本。

如果已经有同一个项目的本地副本，通常不需要再次克隆到同一个位置。

### 4. `git clone`、`git pull` 和 `git push` 的区别

| 命令          | 作用             |
| ----------- | -------------- |
| `git clone` | 初次获取远程仓库的本地副本  |
| `git pull`  | 获取远程更新并整合到当前分支 |
| `git push`  | 将本地提交上传到远程仓库   |

## 八、推荐记忆的完整流程

### 第一次创建并上传项目

```
git init
git add -A
git commit -m "first commit"
git remote add origin https://github.com/用户名/仓库名.git
git push -u origin master
```

这里假设本地分支名称为 `master`，并且远程仓库允许直接推送。如果本地分支叫 `main`，就使用 `main`。

### 日常修改并上传

```
# 先在 VS Code 中按 Ctrl + S

git add -A
git commit -m "描述本次修改"
git push
```

### 创建分支、开发并合并

```
# 创建新分支
git branch B

# 切换到新分支
git checkout B

# 修改代码后暂存、提交
git add -A
git commit -m "完成B功能"

# 切换回接收合并的分支
git checkout main

# 将B合并到main
git merge B
```

### 最重要的几个概念

1. `git init`：初始化仓库。
2. `git add`：暂存修改。
3. `git commit`：创建版本记录。
4. `git branch`：查看或创建分支。
5. `git checkout`：切换分支。
6. `git merge`：将指定分支合并到当前分支。
7. `git remote add`：关联远程仓库。
8. `git push`：上传本地提交。
9. `git clone`：将远程仓库复制到本地。
10. `git pull`：获取并整合远程更新。