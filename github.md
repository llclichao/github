## 一、Git 是什么，为什么要用它

Git 是一个**版本控制系统**。它记录你每次对文件的修改，让你可以：
- 回退到任意历史版本
- 查看某次改动改了什么
- 多人协作时合并各自的修改
- 把本地代码推送到 GitHub/Gitee 等远程仓库

Git 是**本地工具**，GitHub 是**远程托管平台**。你先用 Git 在本地管理版本，再把提交推送到 GitHub。

---

## 二、安装与初始配置（一次性）

安装 Git 后，打开 Git Bash，配置身份信息（每次提交都会带上这个身份）：

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱@example.com"
```

可选：设置默认分支名为 `main`（GitHub 现在的默认标准）：

```bash
git config --global init.defaultBranch main
```

查看当前配置：

```bash
git config --list
```

---

## 三、Git 的三个区域与核心概念

理解这三个区域，Git 的命令就不会乱：

| 区域         | 含义                            | 对应命令     |
| ------------ | ------------------------------- | ------------ |
| **工作区**   | 你实际编辑文件的文件夹          | 你直接改文件 |
| **暂存区**   | 挑选出来、准备提交的改动        | `git add`    |
| **本地仓库** | 已提交的历史记录（`.git` 目录） | `git commit` |

远程仓库（GitHub）是第四个位置，用 `git push` 把本地仓库的提交送上去。

**核心流程**：工作区 → `git add` → 暂存区 → `git commit` → 本地仓库 → `git push` → 远程仓库。

---

## 四、从零创建一个本地 Git 仓库

```bash
cd ~/Desktop/你的项目        # 进入项目文件夹
git init                     # 初始化，生成隐藏的 .git 目录
```

`git init` 后，这个文件夹就成为 Git 仓库。**Git 命令必须在仓库目录或其子目录内执行**，否则会报 `fatal: not a git repository`。

查看仓库状态：

```bash
git status
```
- 红色文件：有改动但未加入暂存区
- 绿色文件：已加入暂存区，等待提交
- `nothing to commit, working tree clean`：没有未提交的改动

---

## 五、添加、提交、查看历史

**1. 添加文件到暂存区**
```bash
git add .              # 当前目录及子目录所有改动
git add 文件名          # 只加指定文件
git add -A             # 整个仓库所有改动
```

**2. 提交到本地仓库**
```bash
git commit -m "提交说明"
```
`-m` 后面写清楚这次改了什么，比如 `"修复登录页面的校验逻辑"`，不要只写 `"update"`。

**3. 查看提交历史**
```bash
git log                # 完整历史
git log --oneline      # 一行一条，简洁
git log --oneline -5   # 只看最近 5 条
```

**4. 查看某次提交改了什么**
```bash
git show 提交编号
```

---

## 六、撤销与回退（常用）

| 场景                                   | 命令                             |
| -------------------------------------- | -------------------------------- |
| 撤销工作区某个文件的修改（未 add）     | `git restore 文件名`             |
| 把文件从暂存区撤出（已 add 未 commit） | `git restore --staged 文件名`    |
| 修改最近一次提交的说明                 | `git commit --amend -m "新说明"` |
| 回退到某个历史提交（保留改动）         | `git reset --soft 提交编号`      |
| 回退到某个历史提交（丢弃改动，谨慎）   | `git reset --hard 提交编号`      |

`--hard` 会丢弃未提交的改动，执行前确认没有需要保留的内容。

---

## 七、分支基础

分支让你在不影响主线的同时开发新功能。

```bash
git branch                 # 查看本地分支，* 表示当前分支
git branch 新分支名         # 创建分支
git checkout 分支名         # 切换分支
git checkout -b 新分支名    # 创建并切换
git merge 分支名            # 把指定分支合并到当前分支
git branch -d 分支名        # 删除已合并的分支
```

日常单人开发，初期可以只用 `main`/`master` 一条分支；需要试验新功能时再开分支。

---

## 八、关联远程仓库并推送（对接 GitHub）

**1. 在 GitHub 创建空仓库**：不要勾选 README、.gitignore、license，保持远程为空，避免推送冲突。

**2. 关联远程地址**
```bash
git remote add origin https://github.com/用户名/仓库名.git
git remote -v              # 确认关联成功
```

**3. 首次推送**
```bash
git push -u origin main
```
- 本地分支是 `master` 就写 `master`
- `-u` 设置上游跟踪，以后只需 `git push`

**4. 首次认证**：HTTPS 方式会要求登录。GitHub 不支持登录密码，需要用 **Personal Access Token（PAT）** 作为密码，或在浏览器弹窗中授权。

**5. 验证**：刷新 GitHub 仓库页面，文件应已出现。

---

## 九、日常协作流程（记住这几条）

**每天开始工作前**，先拉取远程最新改动：
```bash
git pull
```

**修改文件后**：
```bash
git add .
git commit -m "描述这次改了什么"
git push
```

因为已用 `-u` 建立跟踪，`git push` 和 `git pull` 不用再写 `origin main`。

**查看远程仓库地址**：
```bash
git remote -v
```

**克隆别人的仓库到本地**：
```bash
git clone https://github.com/用户名/仓库名.git
```

---

## 十、`.gitignore`：排除不需要上传的文件

在仓库根目录创建 `.gitignore` 文件，每行写一个规则，例如：

```
*.o
*.log
build/
node_modules/
.env
```

被忽略的文件不会出现在 `git status` 中，也不会被 `git add .` 加入。**上传前务必检查**：密钥、密码、编译产物、大文件不应进入仓库。

---

## 十一、常见报错速查

| 报错                                  | 原因                 | 解决                                |
| ------------------------------------- | -------------------- | ----------------------------------- |
| `fatal: not a git repository`         | 当前目录不是仓库     | `cd` 进仓库目录，或先 `git init`    |
| `src refspec main does not match any` | 分支名不匹配         | `git branch` 看实际分支，推对应名字 |
| `Could not resolve host: github.com`  | DNS 解析失败         | 检查 hosts 文件、DNS、代理          |
| `Failed to connect to github.com:443` | 网络连接超时         | 检查网络、配 Git 代理、重试         |
| `non-fast-forward` / `fetch first`    | 远程有本地没有的提交 | 先 `git pull` 合并再推              |
| 认证失败                              | 用了登录密码         | 改用 PAT 或浏览器授权               |
| 单文件超过 100MB                      | GitHub 限制          | 用 Git LFS 或排除该文件             |

---

## 十二、最小必会命令清单

如果只想记住最核心的，就这几条：

```bash
git init                     # 初始化仓库
git status                   # 看状态
git add .                    # 加改动
git commit -m "说明"          # 提交
git remote add origin <URL>  # 关联远程
git push -u origin main      # 首次推送
git push                     # 以后推送
git pull                     # 拉取远程更新
git log --oneline            # 看历史
git clone <URL>              # 克隆仓库
```

---

**一句话总结**：Git 负责本地版本管理（`add` → `commit`），GitHub 负责远程托管（`remote add` → `push`）。先 `init` 建仓库，改完 `add` + `commit`，关联远程后 `push -u` 一次，以后日常就是 `pull` → 改文件 → `add` → `commit` → `push`。