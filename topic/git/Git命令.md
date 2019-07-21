### 用 git 命令行上传本地代码到github

1. 进入本地的项目目录，右键“Git Bash here”,调出git命令行界面，然后输入

    ```
    git init
    ```

2. 就是将目录下的所有文件添加到缓存区，也可以将“.”换成具体的文件名

    ```
    git add .
    
    git add newfile.txt
    ```
3. 将上面缓存区的文件提交到本地仓库

    ```
    git commit -m "注释"
    ```
4. 在 Github 上创建自己的本地仓库 Repository
5. 将本地仓库与 Github 关联

    ```
    git remote add origin https://github.com/qiuyk/blog.git
    ```
6.  上传到 Github 之前先 pull 一下远程仓库，执行如下命令
    ```
    git pull origin master
    ```
7. 上传代码到 Github 远程仓库

    ```
    git push -u origin master
    ```

### 将修改的文件提交

1. 不管修改/删除/添加文件后，都可以通过git status查看当前状态
    ```
    git status
    ```
2. 如果有修改的文件
    ```
    git add file.txt
    ```
3. 提交
    ```
    git commit -m "修改"
    ```
4. 推送到远程仓库
    ```
    git push 
    或
    git push -u origin master
    ```
    
### 获取远程仓库最新代码

1. 查看远程仓库
    ```
    git remote -v
    ```
2. 拉取远程分支
    ```
    //拉取master分支到本地master分支
    git fetch origin master
    ```
    **注意：不建议使用pull拉取最新代码，因为pull拉取下来后会自动和本地分支合并**
