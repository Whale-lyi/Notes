# Git及GitHub使用
## Git部分

- ### Git中文件的三种状态
    - #### Modified 已修改
        - 修改了文件，但还没保存到数据库中

    - #### Staged 已暂存
        - 对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中

    - #### Committed 已提交
        - 数据已安全的保存在本地数据库中

- ### Git使用与操作命令
    - #### 配置
        - git config --global user.name "name"
        - git config --global user.email "email"
        - git config --list

    - #### 新建Git仓库
        - git init

    - #### 添加文件到暂存区
        - git add \<filename>
        - git add .
        - gti add -A
    
    - #### 提交暂存区的修改
        - git commit -m \<commit message>
    
    - #### 查看git仓库状态
        - git status
    
    - #### 查看提交日志
        - git log (键入:q退出)

    - #### 版本切换
        - git checkout \<某版本的代号前几位,可区分即可/master(最新版本)>

    - #### 新建分支
        - git checkout -b \<branch name>
    
    - #### 查看分支信息
        - git branch
    
    - #### 合并分支
        - git merge (第一行结尾添加auto)

## GitHub部分
- ### SSH-key与身份校验
    - #### SSH-key生成
        - ssh-keygen -t rsa -C "your email"
        - cat id_rsa.pub
    
    - #### 关联SSH-key到GitHub账户
- ### Git与GitHub交互常用命令
    - #### 克隆Git仓库到本地,让自己能够查看或修改项目
        - git clone
    - #### 拉取远程仓库数据并合并到当前分支
        - git pull (= git fetch + git merge)
    - #### 推送本地仓库的commit记录到远程仓库
        - git push