# git学习

## 1.0 提炼

1.  git commit

    ~~~shell
    git commit
    ~~~

2.  git branch

    ~~~shell
    #创建bugFix分支并切换到此分支
    git branch bugFix
    git checkout bugFix
    #简写
    git checkout -b bugFix
    ~~~

3.  git merge

    ~~~shell
    git checkout -b new
    git commit
    git checkout main
    git commit
    git merge new
    ~~~

4.  git rebase

    ~~~shell
    git checkout -b new
    git commit
    git checkout main
    git commit
    git checkout new
    git rebase main
    ~~~

5.  分离HEAD

    ~~~bash
    git checkout commitId
    ~~~

6.  相对引用（^）

    ~~~bash
    git checkout onebranch^
    #或
    git checkout onebranch
    git checkout HEAD^
    ~~~

7.  相对引用2（~）

    ~~~bash
    #branch -f 参数可以让分支指向另外一个提交
    git branch -f main HEAD~3
    ~~~

8.  撤销变更

    ~~~bash
    #git reset 通过把分支记录回退几个提交记录来实现撤销改动。
    git reset HEAD^
    #对于大家一起使用的远程分支是无效的,需要使用revert
    git revert HEAD
    
    ~~~

9.  git cherry-pick

    ~~~bash
    #可以将一些提交复制到当前所在（HEAD）下面
    git cherry-pick <提交号>...
    git cherry-pick c3 c2 c5
    ~~~

10.  交互式rebase

     ~~~bash
     git rebase -i HEAD~4
     ~~~

11.  Git Tag

     ~~~bash
     git tag v0 c1
     ~~~

12.  多次rebase

     ~~~bash
     git rebase main bugFix
     git rebase side anothor
     ~~~

13.  两个父节点

     ~~~bash
     #操作符后面的数字与 ~ 后面的不同，并不是用来指定向上返回几代，而是指定合并提交记录的某个父提交
     git checkout main^2
     ~~~

14.  纠缠不清的分支

     ~~~bash
     #切换到分支one
     git checkout one
     #提交按需排序
     git cherry-pick C4 C3 C2
     #切换到分支two
     git checkout two
     git cherry-pick C5 C4 C3 C2
     git branch -f three c2
     ~~~

15.  Git Clone

     ~~~bash
     #在本地创建一个远程仓库的拷贝
     git clone
     ~~~

16.  远程分支

     ~~~bash
     #远程分支反映了远程仓库(在你上次和它通信时)的状态
     git checkout origin/main
     ~~~

17.  Git Fetch

     ~~~bash
     #Git 远程仓库相当的操作实际可以归纳为两点：向远程仓库传输数据以及从远程仓库获取数据。
     #从远程仓库获取数据 git fetch
     #从远程仓库下载本地仓库中缺失的提交记录
     #更新远程分支指针(如 o/main)
     #git fetch 并不会改变你本地仓库的状态。它不会更新你的 main 分支，也不会修改你磁盘上的文件。
     git fetch
     #git fetch 后可以合并远程分支
     git cherry-pick o/main
     git rebase o/main
     git merge o/main
     #git fetch 参数
     git fetch origin foo
     git fetch origin <source>:<destination>
     ~~~

18.  Git Pull

     ~~~bash
     #git pull 等效于
     #git fetch
     #git merge origin/main
     git pull
     #git pull 参数
     git pull origin foo 
     #相当于 
     git fetch origin foo; git merge o/foo
     
     
     ~~~

19.  Git Push

     ~~~bash
     #git push 负责将你的变更上传到指定的远程仓库，并在远程仓库上合并你的新提交记录
     git push
     #Git Push 的参数
     git push <remote> <place>
     #git push 的参数2
     git push origin <source>:<destination>
     ~~~

20.  偏离的提交历史

     ~~~bash
     #困难往往来自远程库提交历史偏高
     #当远程仓库的代码更改后先要拉取合并后再push
     #等同于 git pull --rebase
     git fetch
     git rebase origin/main
     git push
     #等同于 git pull
     git fetch
     git merge origin/main
     git push
     ~~~

21.  锁定的main

     ~~~bash
     #如果在一个大的合作团队中工作，很有可能main分支被锁定了
     #应新建一个分支，推送（push）这个分支，再申请pull request
     #当你忘记并直接提交了给main的情况下，如何处理
     #reset你的main分支和远程服务器保持一致
     git reset --hard origin/main
     #新建分支推送
     git checkout -b feature c2
     git push origin feature
     ~~~

22.  推送主分支

     ~~~bash
     #将我们的工作 rebase 到远程分支的最新提交记录
     git pull --rebase
     #分支推送到远程
     git push
     ~~~

23.  合并远程仓库

     ~~~bash
     #Rebase 使你的提交树变得很干净
     #merge 保留所有提交记录
     git checkout main
     git pull origin main
     git merge side1
     git merge side2
     git merge side3
     git push origin master
     ~~~

24.  远程跟踪

     ~~~bash
     #pull操作时, 提交记录会被先下载到 o/main 上，之后再合并到本地的 main 分支。隐含的合并目标由这个关联确定的。
     #push操作时, 我们把工作从 main 推到远程仓库中的 main 分支(同时会更新远程分支 o/main) 。这个推送的目的地也是由这种关联确定的！
     #直接了当地讲，main 和 o/main 的关联关系就是由分支的“remote tracking”属性决定的
     git checkout -b totallyNotMain o/main
     git checkout -b side origin/main
     ~~~

25.  没有source的source

     ~~~bash
     git push origin :bar #会删除远程仓库的分支
     git fetch origin :bar #会在本地创建一个新分支
     ~~~

26.  如何向两个不同的远程仓库推送

     ~~~bash
     #方法一：添加远程仓库
     git remote add k1 http://xxx.xxx.xx
     git remote add k2 http://xxx.xxx.xx
     git push k1 main:main
     git push k2 main:main
     #方法二：git remote set-url
     git remote set-url --add oginin http://xxx.xx.xx
     ~~~

27.  ssh key

     ~~~bash
     #在指定位置生成密钥
     ssh-keygen -t rsa -C "your_secondemail@email.com" -f ~/.ssh/second-rsa
     # 常用的选项
-b：指定密钥长度；
     -C：添加注释；用于为指定注释，可以是任何内容，通常使用自己的邮件名作为注释。
     -f：指定用来保存密钥的文件名；
     -t：指定要创建的密钥类型（加密方式）
     -e：读取openssh的私钥或者公钥文件；
     -i：读取未加密的ssh-v2兼容的私钥/公钥文件，然后在标准输出设备上显示openssh兼容的私钥/公钥；
     -l：显示公钥文件的指纹数据；
     -N：提供一个新密语；
     -P：提供（旧）密语；
     -q：静默模式；
     #把公钥推送到服务器
     ssh-copy-id -i .ssh/king.pub root@1.1.1.1 -p 10086
     ~~~
     
28.  不同密钥访问不同git服务器

     >   1.  我的电脑 -> 右键管理 -> 服务 -> OpenSSH ->自动开启 
     >
     >   2.  GIT BASH 创建~/.brashrc 
     >
     >       ~~~bash
     >       env=~/.ssh/agent.env
     >       agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }
     >       agent_start () {
     >           (umask 077; ssh-agent >| "$env")
     >           . "$env" >| /dev/null ; }
     >       agent_load_env
     >       # agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2= agent not running
     >       agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)
     >       if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
     >           agent_start
     >           ssh-add ~/.ssh/one_rsa # 添加你自己的私钥
     >       elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
     >           ssh-add ~/.ssh/one_rsa # 添加你自己的私钥
     >       fi
     >       unset env
     >       ~~~
     >
     >   3.  ~~~bash
     >       #检查密钥是否被加载
     >       ssh-add -l
     >       #添加密钥到ssh-agent
     >       ssh-add ~/.ssh/private_key
     >       ~~~
     >
     >   4.  ~~~bash
     >       ssh-keygen -f king -C king
     >       ssh-keygen -f github549942844 -C king2046@github
     >       ssh-keygen -f gitee13456367274 -C king@gitee
     >       
     >       ~~~
     >
     >   5.  ~~~bash
     >       touch ~/.ssh/config
     >       #debugtalk
     >       	Host debugtalk
     >       	    HostName github.com
     >       	    User git
     >       	    IdentityFile ~/.ssh/id_rsa
     >       
     >       # DJI
     >       	Host djileolee
     >       	    HostName github.com
     >       	    User git
     >       	    IdentityFile ~/.ssh/dji_id_rsa
     >       ~~~

## 1.1 关于版本控制

-   版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。

-   本地版本控制

    <img src="https://git-scm.com/book/en/v2/images/local.png" alt="本地版本控制图解" style="width:50%;" />

-   集中化的版本控制系统

    <img src="https://git-scm.com/book/en/v2/images/centralized.png" alt="集中化的版本控制图解" style="width:50%;" />

-   分布式版本控制系统

    <img src="https://git-scm.com/book/en/v2/images/distributed.png" alt="分布式版本控制图解" style="width:50%;"/>

## 1.2 Git简史

​	2005年，开发BitKeeper的商业公司同Linux内核开源社区的合作结束，不再提供免费使用，迫使 Linus Torvalds 开发自己的版本管理系统Git。

## 1.3 Git是什么

1.  直接记录快照，而非差异比较（Git对待数据更像是一个快照流）

2.  近乎所有操作都是本地执行

3.  Git保证完整性

4.  Git一般只添加数据

5.  `三种状态`

    · 已提交（committed）

      已提交表示数据已经安全地保存在本地数据库中

    · 已修改（modified）

      已修改表示修改了文件，但还没保存到数据库中

    · 已暂存（staged）

      已暂存表示对一个已经修改的文件的当前版本做了标记

      使之包含在下次提交的快照中

6.  工作目录、暂存区域、Git仓库

    

    ![工作区、暂存区以及 Git 目录。](https://git-scm.com/book/en/v2/images/areas.png)

    1.  工作区是对项目的某个版本独立提取出来的内容。这些从Git仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。
    2.  暂存区是一个文件，保存了下次将要提交的文件列表信息，一般在Git仓库目录中。按照Git的术语叫做“索引”，不过一般说法还是叫“暂存区”。
    3.  Git仓库目录是Git用来保存项目的元数据和对象数据库的地方。这是Git中最重要的部分，从其它计算机克隆仓库时，复制的就是这里的数据。

7.  基本的Git工作流程

    1.  在工作区中修改文件。
    2.  将你想要下次提交的更改选择性地暂存
    3.  提交更新，找到暂存区的文件，将快照永久性存储到Git目录

## 1.4 初次运行Git前的配置

1.  配置用户名和邮件地址

    ~~~bash
    git config --global user.name  'xxx'
    git config --global user.email 'xx@xx.com'
    #当你想在不同目录使用不同用户信息的时候运行不带 --global 配置命令选项
    ~~~

2.  文本编辑器

    ~~~bash
    git config --global core.editor "'D:/开发软件/Sublime Text/sublime_text.exe'"
    #当 Git 需要你输入信息时会调用它
    ~~~

3.  获取帮助

    ~~~bash
    git add -h
    git help add
    ~~~



## 2.1 获取Git仓库

### 获取git项目仓库

1.  将尚未进行版本控制的本地目录转换为 Git 仓库；

    ~~~bash
    git init
    ~~~

2.  从其它服务器 **克隆** 一个已存在的 Git 仓库。

    ~~~bash
    git clone https://github.com/xxx/xxx
    ~~~

## 2.2 记录每次更新到仓库

1.  检查当前文件状态

    ~~~bash
    git status
    
    git status -s
    
    ~~~

2.  忽略文件

    ~~~bash
    cat .gitignore
    
    #忽略所有以.o或者.a结尾的文件
    *.[oa]
    
    #忽略所有名字以波浪符结尾的文件
    *~
    
    # 忽略所有的 .a 文件
    *.a
    
    # 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
    !lib.a
    
    # 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
    /TODO
    
    # 忽略任何目录下名为 build 的文件夹
    build/
    
    # 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
    doc/*.txt
    
    # 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
    doc/**/*.pdf
    
    ~~~

3.  查看已暂存和未暂存的修改

    ~~~bash
    #比较的是工作目录中当前文件和暂存区域快照之间的差异
    #修改之后还没有暂存起来的变化内容。
    git diff
    
    #查看已暂存的将要添加到下次提交里的内容
    #这条命令将比对已暂存文件与最后一次提交的文件差异
    git diff --staged
    
    ~~~

4.  提交更新

    ~~~bash
    git commit -m 'xxx'
    
    #跳过使用暂存区域提交
    git commit -a -m 'xxx'
    
    ~~~

5.  移除文件

    ~~~bash
    git rm x.x #从已跟踪文件清单中移除(从暂存区域移除)
    
    git rm --cached XXX #把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中
    
    ~~~

6.  移动文件

    ~~~bash
    git mv file_from file_to
    ~~~

## 2.3 查看提交历史

~~~bash
git log
git log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
~~~

## 2.4 撤销操作

~~~bash
git commit --amend
#重新提交，第二次提交会代替第一次提交
git restore --staged xxx
#取消暂存的文件

~~~

## 2.5 远程仓库的使用

~~~ bash
#查看远程仓库
git remote
git remote -v
#添加远程仓库
git remote add <shortname> <url>
#从远程仓库抓取与拉取
git fetch <remote> #只会将数据下载到本地，需要手动合并
git pull <remote> #自动抓取并合并
#推送到远程仓库
git push origin master
#查看某个远程仓库
git remote show origin
#远程仓库的重命名与移除
git remote rename yname nname
git remote remove nname

~~~

## 打标签

~~~bash
#已示重要，为某个提交打上标签
git tag
#git 支持两种标签
#轻量标签很像一个不会改变的分支——它只是某个特定提交的引用。
git tag v0.0.1
#附注标签是存储在 Git 数据库中的一个完整对象， 它们是可以被校验的
git tag -a v1.4 -m "my version 1.4"
#后期补打标签
git tag -a v1.2 9fcev
#共享标签
git push origin v1.5
#删除标签
git tag -d v1.5
#删除远程标签
git push origin :refs/tags/v1.4-lw
git push origin --delete <tagname>
#检出标签
git checkout 2.0.0
git checkout -b version2 v2.0.0
~~~

## Git别名

~~~bash
#别名可以简化复杂的命令
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status

~~~

## 分支模型

>   Git 的分支，其实本质上仅仅是指向提交对象的可变指针。

~~~bash
#创建分支
git branch branchname
#切换分支
git checkout branchname
#合并命令
git checkout -b branchname
~~~

## 分支的新建与合并

~~~bash
#新需求创建一个分支
git checkout -b iss53
#遇到突发bug修复，迁换回主分支创建修复分支
git checkout master
git checkout -b hotfix
#合并分支
git merge hotfix
#删除分支
git branch -d hotfix
#分支冲突解决
~~~

## 分支管理

~~~bash
#获取分支列表
git branch
#获取列表并查看最后那一次提交
git branch -v
#查看已经合并的分支
git branch --merged
##查看未合并的分支
git branch --no-merged
~~~

## 分支的开发工作流

>   长期分支:
>
>   比如只在 `master` 分支上保留完全稳定的代码——有可能仅仅是已经发布或即将发布的代码。
>
>   一些名为 `develop` 或者 `next` 的平行分支，被用来做后续开发或者测试稳定性

![趋于稳定分支的工作流（“silo”）视图。](https://git-scm.com/book/en/v2/images/lr-branches-2.png)

>   主题分支:主题分支是一种短期分支，它被用来实现单一特性或其相关工作.

![拥有多个主题分支的提交历史。](https://git-scm.com/book/en/v2/images/topic-branches-1.png)

## 远程分支

~~~bash
#推送
git push origin serverfix
git push origin serverfix:awesomebranch
#合并分支
git merge origin/serverfix
#跟踪分支
git checkout -b serverfix origin/serverfix
git checkout --track origin/serverfix
git checkout -b sf origin/serverfix
#删除远程分支
git push origin --delete serverfix
~~~

## 变基

>   变基（rebase）:提交到某一分支上的所有修改都移至另一分支上。
>
>   原理：首先找到这两个分支的最近共同祖先C2，然后对比当前分支相对于该祖先的历次提交，提取相应的修改并存为临时文件，然后将当前分支指向目标基底C3，最后以此将之前另存为临时文件的修改依序应用。
>
>   变基使得提交历史更加整洁
>
>   只对尚未推送或分享给别人的本地修改执行变基操作清理历史，从不对已推送至别处的提交执行变基操作

~~~bash
git checkout exp
git rebase master
git checkout master
git merge exp
~~~

