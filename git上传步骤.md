## 1. 在 GitHub 上创建一个仓库

- 登录 [GitHub](https://github.com) 网站
- 点击右上角「+」按钮，选择「New repository」
- 输入仓库名，比如 `my-project`
- 选择公开（Public）或私有（Private）
- 不要初始化 README（后续会从本地上传）
- 点击「Create repository」

---

## 2. 本地项目初始化 Git（如果还没初始化）

```bash
cd /path/to/your/project
git init
```

------

## 3. 添加远程仓库地址

把下面的地址换成你 GitHub 创建的仓库地址：

```bash
git remote add origin https://github.com/你的用户名/仓库名.git
```

或者使用 SSH（需要配置 SSH key）：

```bash
git remote add origin git@github.com:你的用户名/仓库名.git
```

------

## 4. 添加文件并提交

```bash
git add .
git commit -m "首次提交"
```

------

## 5. 推送到远程仓库

```bash
git push -u origin main
```

注意：有些仓库默认主分支是 `master`，如果报错，可以用

```bash
git push -u origin master
```

------

## 6. 之后如果只想上传修改：

```bash
git add .
git commit -m "更新说明"
git push
```

----

## 7.强制合并

```bash
git checkout main #切换到main分支
git merge master --allow-unrelated-histories #强制合并master分支到main
```

----

## 8.删除分支

```bash
git branch -d master
```

