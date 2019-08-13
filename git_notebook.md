git笔记



## 设定

设置或查看(不加后面的 `XXX` 即可)**user**:`git config --global user.name XXX   `

设置或查看(不加后面的 `XXX` 即可)**email**:`git config --global email.name XXX   `





## 文件

##### 文件的添加与上传

添加文件：`git add XXX`

​	可一次添加多个文件；	

上传文件：`git commit -m 'XXX'`

​	`XXX`为上传内容的说明

注:

1.  若对文件修改后，需从新添加`add`及上传`commit`。

2. `git add`是将文件储存至*暂存区*



##### 查看文件状态及版本

查看目前版本:`git log`

恢复某版本:`git reset --hard HEAD^`或`git reset --hard XXX`

​	其中前者`HEAD^`表恢复上一个版本，若为`HEAD^^`表示恢复上上个版本或写成`HEAD~2`，以此类推；

​	后者`XXX`表示为每个版本号,一般写前几位即可

查看历史文件操作记录:`git reflog`:以免恢复上一个版本后，无法通过`git log`找到最新版本的文件



查看当前文件夹的状态:`git status` 

​	可获得消息为：某某某文件已被修改尚未添加，某某某文件尚未上传等等；

​	将忽略其中的文件夹。

查看修好内容:`git diff XXX`

查看工作区与版本库最新文件的区别:`git diff HEAD -- XXX`



##### 文件恢复及救命

文件在工作区的修改全部撤销：`git checkout -- XXX`

> 一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
>
> 一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。



撤销已进行的`git add`:`git reset HEAD XXX`

若文件已经`commit`，选择版本回退`git reset --hard XXX`

PS：`XXX`指的是哈希值，可通过`git log`来查找对应版本的哈希值，一般只需要前面几个即可



文件删除：

1. 本地删除文件`XXX`
2. 添加删除：`git rm XXX`
3. 提交删除：`git commit -m 'information for this commit'`

若本地错误删除文件，通过`git checkout -- XXX`恢复文件

> `git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。





## 关于GitHub

 添加*SHH key*:`ssh-keygen -t rsa -C "youremail@example.com"`

> 可先在用户目录下查看是否有`.shh`，以及该文件下是否有`id_rsa`和`id_rsa.pub`文件；
>
> `id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人；
>
> 
>
> 若新添加：
>
> 登陆GitHub，打开“Account settings”，“SSH Keys”页面：
>
> 然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容即可
>
> 通过`ssh-keygen -t rsa -C "youremail@example.com"`会新建一个SHH.



告诉git 添加到哪个GitHub上。

```
git remote add origin git@github.com:XXX/XXX.git
```

上传到GIthub上，主支

```
git push -u origin master
```

暂时未上传成功。



### 从远程库删除某些文件但保留本地的文件

有时我们会误把一些不必要的文件（如目标文件、log 日志等）提交并推送到了远程库，现在希望从远程库删除这些文件但保留本地的文件，可以像这样 执行：

```bash
git rm -r --cached dir1
git rm --cached dir2/*.pyc
git commit -m "remove irrelated files"
git push origin branch1
```





# 补充

1. 创建`.gitgnore`文件：`touch .gitgnore`

2. 关于提交的忽略‘规则’，

   ```
   #               表示此为注释,将被Git忽略
   *.a             表示忽略所有 .a 结尾的文件
   !lib.a          表示但lib.a除外
   /TODO           表示仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
   build/          表示忽略 build/目录下的所有文件，过滤整个build文件夹；
   doc/*.txt       表示会忽略doc/notes.txt但不包括 doc/server/arch.txt
    
   bin/:           表示忽略当前路径下的bin文件夹，该文件夹下的所有内容都会被忽略，不忽略 bin 文件
   /bin:           表示忽略根目录下的bin文件
   /*.c:           表示忽略cat.c，不忽略 build/cat.c
   debug/*.obj:    表示忽略debug/io.obj，不忽略 debug/common/io.obj和tools/debug/io.obj
   **/foo:         表示忽略/foo,a/foo,a/b/foo等
   a/**/b:         表示忽略a/b, a/x/b,a/x/y/b等
   !/bin/run.sh    表示不忽略bin目录下的run.sh文件
   *.log:          表示忽略所有 .log 文件
   config.php:     表示忽略当前路径的 config.php 文件
    
   /mtk/           表示过滤整个文件夹
   *.zip           表示过滤所有.zip文件
   /mtk/do.c       表示过滤某个具体文件
    
   被过滤掉的文件就不会出现在git仓库中（gitlab或github）了，当然本地库中还有，只是push的时候不会上传。
    
   需要注意的是，gitignore还可以指定要将哪些文件添加到版本管理中，如下：
   !*.zip
   !/mtk/one.txt
    
   唯一的区别就是规则开头多了一个感叹号，Git会将满足这类规则的文件添加到版本管理中。为什么要有两种规则呢？
   想象一个场景：假如我们只需要管理/mtk/目录中的one.txt文件，这个目录中的其他文件都不需要管理，那么.gitignore规则应写为：：
   /mtk/*
   !/mtk/one.txt
    
   假设我们只有过滤规则，而没有添加规则，那么我们就需要把/mtk/目录下除了one.txt以外的所有文件都写出来！
   注意上面的/mtk/*不能写为/mtk/，否则父目录被前面的规则排除掉了，one.txt文件虽然加了!过滤规则，也不会生效！
    
   ----------------------------------------------------------------------------------
   还有一些规则如下：
   fd1/*
   说明：忽略目录 fd1 下的全部内容；注意，不管是根目录下的 /fd1/ 目录，还是某个子目录 /child/fd1/ 目录，都会被忽略；
    
   /fd1/*
   说明：忽略根目录下的 /fd1/ 目录的全部内容；
    
   /*
   !.gitignore
   !/fw/ 
   /fw/*
   !/fw/bin/
   !/fw/sf/
   说明：忽略全部内容，但是不忽略 .gitignore 文件、根目录下的 /fw/bin/ 和 /fw/sf/ 目录；注意要先对bin/的父目录使用!规则，使其不被排除。
   ```

    from:https://www.cnblogs.com/kevingrace/p/5690241.html

   

