# 用 GitHub 更新软件仓库 · 新手从零教程

> 本文根据一次实际动手教学整理，适合**完全没用过 Git/GitHub** 的人。
> 跟着每一节一步步敲命令，就能自己把代码推到 GitHub，并随时更新和同步。

---

## 0. 先说清楚「这是怎么回事」

- **本地电脑**：你写代码的地方，用 `git` 软件管理版本。
- **GitHub 云端**：网上的仓库，用来**备份**、**分享**、**多人协作**、**可回溯**。
- **「更新仓库」的本质**：
  - 自己改了代码 → **推送 `push`** 到 GitHub（本地 → 云端）
  - 别人改了代码 → **拉取 `pull`** 到本地（云端 → 本地）

一句话口诀：

> **改代码 → `git add` → `git commit` → `git push`**

这四步就是「更新仓库」的完整循环。

---

## 1. 安装 Git 并配置身份

### 1.1 确认 / 安装 Git

先确认是否已安装，打开终端（下面会讲怎么开），输入：

```bash
git --version
```

能看到 `git version 2.55.0.windows.1` 之类就说明已安装。
没安装就去官网下载：<https://git-scm.com/downloads>（Windows 装 `.exe`，一路「下一步」即可）。

### 1.2 配置你的身份（一次即可）

告诉 git 你是谁，这样每次提交都会记上你的名字和邮箱：

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱@example.com"
```

> `--global` 表示「对这台电脑上所有项目生效」，配一次就永久记住。

查看是否配置成功：

```bash
git config --global user.name
git config --global user.email
```

---

## 2. 打开终端（命令行窗口）

**Windows** 方法：

1. 按键盘 **Windows 徽标键**（左下角四方格图标）
2. 输入 **`powershell`**
3. 按回车，弹出蓝/黑底的窗口就能打字了。

> 后面所有代码都在这个窗口里输入，**每行输完按一次回车**。

---

## 3. 建立本地仓库

### 3.1 新建项目文件夹并初始化

```bash
mkdir learn-git        # 建一个文件夹
cd learn-git           # 进去
git init               # 把它变成一个 git 仓库
```

预期看到：

```
Initialized empty Git repository in .../learn-git/.git/
```

> `.git` 是隐藏文件夹，git 的**所有版本历史都存这里**。它在，仓库才有灵魂。

### 3.2 看仓库状态

```bash
git status
```

一个新仓库会显示：

```
On branch master
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```

含义：当前在 `master` 分支、还没有任何提交、仓库是空的。

---

## 4. 放文件、提交（本地核心三连）

### 4.1 创建第一个文件

在文件夹里建一个 `README.md`（说明文件）。可用记事本：

```bash
notepad README.md
```

弹出「是否创建」→ 点**是**，写入内容，**Ctrl+S** 保存，关闭。
（或用一行命令直接建：`"# 我的GitHub练习仓库" > README.md`）

### 4.2 再看状态

```bash
git status
```

此时 `README.md` 会出现在 **Untracked files**（未跟踪文件）下：
意思是「硬盘上有这个文件，但 git 还没管它」。

### 4.3 加入暂存区（add）

```bash
git add README.md     # 指定一个文件
# 或 git add .        # 全部改动
```

再敲 `git status`，文件会变成绿色 **Changes to be committed**。

### 4.4 提交存档（commit）

```bash
git commit -m "第一次提交：添加 README.md"
```

预期：

```
[master (root-commit) xxxxxxx] 第一次提交：添加 README.md
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

> `-m` 后面那句是**提交说明**，要写清楚「我改了什么」，方便以后回看。

### 4.5 看提交历史

```bash
git log --oneline
```

预期：

```
xxxxxxxx 第一次提交：添加 README.md
```

至此，你学会了本地仓库的完整链路：

> **改代码 → `git add` → `git commit` → `git log`**

---

## 5. 分支改名（可选规范）

现在专业项目默认用 `main` 作为主分支名。首次推送前把 `master` 改成 `main`：

```bash
git branch -M main
git branch        # 应输出 * main
```

---

## 6. 推到 GitHub 云端

### 6.1 在 GitHub 网页建仓库

1. 打开 <https://github.com> 并登录。
2. 点右上角 **「＋」** → **「New repository」**。
3. 填写：
   - `Repository name`：仓库名
   - 可见性：`Public`（公开）或 `Private`（私有）
4. ⚠️ **不要勾选**「Add a README file」「Add .gitignore」「Choose a license」——本地已有提交，勾了会冲突。
5. 点绿色 **「Create repository」**。
6. 把页面给的仓库地址（URL）复制下来，形如：
   `https://github.com/你的用户名/仓库名.git`

### 6.2 连接本地与远程

```bash
git remote add origin https://github.com/你的用户名/learn-git.git
```

`origin` 是给这个远程仓库起的名字（业界默认）。查看是否连上：

```bash
git remote -v
```

预期出现两行（fetch 和 push，地址相同）。

### 6.3 推送（上传）

```bash
git push -u origin main
```

> `-u` 表示「记住关联」，之后再用 `git push` / `git pull` 都知道对着谁。

预期：

```
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

然后打开 `https://github.com/你的用户名/仓库名`，就能看到文件了。

---

## 7. 更新仓库（日常循环）

以后每次改动，都走这四步：

```bash
git status                # ①看改了哪些文件（红色 Modified = 已改未提交）
git add 文件名             # ②放进暂存区
git commit -m "改了什么"   # ③提交存档
git push                  # ④推送到 GitHub
```

> ⚠️ 重要：**改完先 `git status` 确认改了哪些**，再 `add`、`commit`，避免误提交无关文件。

---

## 8. 拉取（把云端更新同步到本地）

当别人（或你在网页上）改了代码并推到了 GitHub，本地要用 `git pull` 同步：

```bash
git pull
```

预期看到类似：

```
Updating xxx..xxx
Fast-forward
 README.md | 1 +
 1 file changed, 1 insertion(+)
```

> `Fast-forward` = 远程改动**无冲突**地合并进来了。
> 若提示 `CONFLICT`（冲突），说明本地的和远程的改了同一处，需要手动挑选保留哪份。

---

## 9. 国内访问 GitHub：让 git 走本地代理（关键！）

**症状**：`git push` 报错

```
fatal: unable to access 'https://github.com/...': Recv failure: Connection was reset
fatal: Failed to connect to github.com:443 ... Could not connect to server
```

**原因**：浏览器开了代理能上网，但 **git 默认不走系统代理**，直连被拦（国内常见）。

**解决**：让 git 也走本地代理（假设你的代理软件在 `127.0.0.1:7890`，Clash/v2rayN 等常用）：

```bash
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```

确认生效：

```bash
git config --global --get http.proxy
git config --global --get https.proxy
```

> 若你的代理端口不是 `7890`，改成你自己的端口。
> 不需要代理时清除：`git config --global --unset http.proxy` 和 `--unset https.proxy`。

---

## 10. 克隆别人/上游仓库

想把别人的仓库完整复制到本地：

```bash
git clone https://github.com/某用户/某仓库.git
cd 某仓库
```

---

## 11. 速查表

| 你要做的事 | 命令 |
|-----------|------|
| 建本地仓库 | `git init` |
| 看状态 | `git status` |
| 改动放暂存区 | `git add 文件名`（全部用 `git add .`） |
| 提交存档 | `git commit -m "说明"` |
| 看历史 | `git log --oneline` |
| 分支改 main | `git branch -M main` |
| 连远程 | `git remote add origin 仓库地址` |
| 推送（上传） | `git push` |
| 拉取（下载） | `git pull` |
| 克隆仓库 | `git clone 仓库地址` |
| 让 git 走本地代理 | `git config --global http.proxy http://127.0.0.1:7890`<br>`git config --global https.proxy http://127.0.0.1:7890` |

---

## 12. 作为参考：你自己的学习记录

本次一路操作下来，你亲手完成了这些：

1. 打开 PowerShell，`git init` 建本地仓库。
2. `git status` 看空仓库。
3. 用 `notepad` 建 `README.md`。
4. `git add` → `git commit` → `git log`。
5. `git branch -M main` 改名分支。
6. 在 GitHub 网页建仓库，`git remote add origin` 连接。
7. 遇到连接失败 → 配代理 → `git push` 成功。
8. 改 README → `add` → `commit` → `push` 完成第二次更新。
9. 在网页改 README → `git pull` 同步回本地。

**你已经掌握的能力**：创建仓库、提交、推送到 GitHub、从 GitHub 拉取，以及解决国内代理问题。

---

## 13. 下一步可以学的主题

- **分支 branch**：在独立分支上开发，不干扰主线。
- **团队协作**：多人同时改一个仓库，如何避免/处理冲突。
- **`.gitignore`**：排除不想上传的文件（大文件、密钥、临时文件）。
- **从 fork 同步上游**：把别人仓库（fork 来源）的新改动同步到自己的仓库。
- **将已有项目托管到 GitHub**：`git init` → 配 `.gitignore` → `add` → `commit` → `remote add` → `push`。

> ⚠️ 托管真实项目时，**大文件用 `.gitignore` 排除**，别把几十 MB 的数据/图片传上 GitHub。

待补充！