# Git 学习笔记

1. Git工作区域

       * 工作区（添加、编辑、修改文件等动作）
       * 暂存区（暂存已经修改的文件，最后统一提交到git仓库中）
       * Git仓库（最终确定的文件保存到仓库，成为一个新的版本，并且对他人可见）

2. 指令操作

       * 查看状态： git status
       * 工作区提交到暂存区：git add [filename]
       * 暂存区提交到仓库： git commit -m "提交描述"

3. 基本信息设置

       * 设置用户名：git config --global user.name 'name'
       * 设置用户名邮箱：git config --global user.email 'name@name.com'

4. 本地仓库初始化及仓库创建和操作

   1. 初始化一个新的Git仓库
      * 新建文件夹
      * 在文件内初始化git（创建git仓库）
      *   cd test     git init
   2. 向仓库中添加文件
      * 创建文件 touch name
      * 添加到暂存区  git add name
      * 提交到仓库  git commit -m ‘描述’
   3. 修改仓库文件
      * 修改文件  vi
      * 添加到暂存区
      * 提交到仓库
   4. 删除仓库文件
      * 删除本地文件 rm -rf name
      * 从Git中删除 git rm name
      * 提交操作 git commit -m ‘描述’

5. Git管理远程仓库

   1. 远程仓库的作用：备份，实现代码共享，集中化管理
   2. 将本地仓库提交到远程仓库：git push
   3. Git克隆操作
      * 将远程仓库（github对应的项目）复制到本地
      * 代码：git clone 仓库地址
   4. 步骤：克隆远程仓库，修改或添加，添加到暂存区，提交到本地仓库，push到远程仓库

6. Github Pages 搭建网站

   * 访问：http://用户名.github.io

   * 搭建步骤

     * 创建个人站点 --> 新建仓库（仓库名必须是【**用户名.github.io**】）
     * 在仓库下新建index.html

     