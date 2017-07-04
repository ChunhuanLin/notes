## 配置
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

## 添加文件


`git add filename` 把修改过的文件添加到git管理仓库，就可以对这个文件进行管理啦，但还不是提交    
`git commit -m "message"` 将文件提交给git  


## 时光机穿梭

`git status` 查看仓库状态，看到哪些文件被修改了    
`git diff filename` 查看某个文件上次修改的情况  

* 版本回退  
  - `git log --graph --pretty=oneline`  
	显示历史提交情况，`-- graph`显示图形结构，后面的参数表示简化显示结果  
  - `git reset --hard HEAD^`  
	HEAD表示当前版本的哈希码，HEAD^表示上一个版本，HEAD^^表示上上个版本，以此类推  
	也可以用HEAD~100来表示往上100个版本  
  - 从过去再次回到未来  
	回溯之后，`git status`无法看到“未来”那个版本的`commit id`了。这时候，如果还能查到他的id，那么就可以使用`git reset --hard commit-id`回到未来。commit id不用全输，只要输前几位就可以了  
  - `git reflog`  
	历史命令的所有记录。可以通过这个方式来找到上面说的“未来版本”的commit id  


* 工作区和暂存区  
  - 工作区  
	就是电脑里能看到的目录  
  - 版本库  
	就是隐藏目录`.git`  
	![.git](./images/git1.jpeg)  
	+ stage  
		`git add`的文件会先存放在stage里面  
	+ 分支  
		首先有一个默认分支`master`，`git commit`即将文件添加到分支中  
	+ HEAD  
		一个指针，指向当前分支    

* 管理修改  
  - git管理的是修改，而不是文件
  - `git diff HEAD -- filename`  
	查看工作区和版本库里面的最新版本的区别  

* 撤销修改  
  - `git checkout -- filename`  
	让文件回到最近一次`git commit`或者`git add`时的状态，即回到git有记录的最新一次状态，撤销在那之后的在工作区的修改  
	> checkout的 ***- -*** 很重要，没有他就表示切换分支  

* 删除文件  
  - 把文件从版本库里删除  
	```
	git rm filename  
	git commit -m 'message'  
	```
  - 误删工作区里的文件，从版本库恢复  
	checkout即可  

## 远程仓库
### 添加远程库
* 设置github  
  1. 创建SSH Key  
	在~/.ssh下看有没有`id_rsa`和`id_rsa.pub`这两个文件，前者是私钥，后者是公钥，如果没有则使用下面命令创建：  
	`$ ssh-keygen -t rsa -C "youremail@example.com"`  
  2. 将公钥添加到github  

* 添加远程库  
  - 配置本地远程库
	1. 在github创建一个与本地仓库名字一致的仓库，记其地址为`remote_repo`  
	2. 本次仓库下执行` git remote add origin remote_repo`  
  - 将本地仓库所有内容推送到远程库  
	`git push -u origin master`  
	这个命令实际上是把当前分支`master`推送到远程的`master`分支。第一次推送加上`-u`参数，以后的提交可以不用`-u`了  

* SSH警告  
  第一次使用`clone`或者`push`时，会得到一个警告，这是为了验证github服务器的key，输入`yes`即可  

### 从远程库克隆
* `git clone remote_repo`

## 分支管理

### 创建与合并分支
* 创建并切换到分支  
	`git checkout -b dev` dev是新建分支的名字  

	> 上述命令相当于：  
	> git branch dev  
	> git checkout dev  

* 查看当前分支  
	`git branch`  

* 合并分支  
	`git checkout master` 将当前分支切到主分支  
	`git merge dev` 将dev分支merge到当前分支，即主分支上  
 	`git branch -d dev` 合并后就可以删掉dev分支了  


### 解决冲突

* 产生冲突的情况  
	当发生下面这种状态时就会产生冲突：
	![git2.png](./images/git2.png)


* 查看冲突  
	`git status` 可以看到有哪些文件产生了冲突  

* 解决冲突  
	直接查看修改后的文件，它里面会标记不同分支冲突的内容，我们将其修改之后，再提交即可。
* 查看冲突解决的情况  
	`git log --graph --pretty=oneline --abbrev-commit`  

### 分支管理策略

* `git merge --no-ff -m 'message' dev`  
	`--no-ff`：通常，不使用这个参数，Git会使用`Fast Forward`策略，master会直接指向dev分支，如果删除了这个分支，那么会丢掉分支的信息。而使用这个参数，则会在merge时产生一个新的commit，也因此要`-m`信息，这样，就可以从分支历史上看到分支信息。因此，通常要使用`--no-ff`参数  
* 分支策略  
	`master`分支应该是非常稳定的，**仅用来发布新的版本**  
	`dev`分支是不稳定的，大家都在dev上干活，合并也都在`dev`上合并 
	![git3.png](./images/git3.png)  

### Bug分支 
  經常有這樣的事情發生，當你正在進行專案中某一部分的工作，裡面的東西處於一個比較雜亂的狀態，而你想轉到其他分支上進行一些工作。問題是，你不想只為了待會要回到這個工作點，就把做到一半的工作進行提交。解決這個問題的辦法就是`git stash`命令。
- `git stash`  
    暂存当前工作现场，而不用进行提交，这样就可以在保存现在工作之后，切到别的分支以处理bug或做其他工作  
- `git stash list`  
    切回原来的工作分支后，可以使用这个命令查看储藏了哪些工作现场  
    '''
    $ git stash list
    stash@{0}: WIP on dev: 6224937 add merge
    '''
- `git stash pop`   
    恢复并删除stash  
    > pop实际上是执行了这两步:  
    > git slash apply stash@{0} 将stash恢复到工作现场  
    > git stash drop 删除stash  

### Feature分支
  添加新功能时就新建一个feature分支，在上面开发，避免搞乱主分支（dev分支）  
  假设此时feature分支需要销毁（老板说，不要这个功能了），而他还没有merge到dev分支，这时，由于还没有merge，因此不能用`-d`参数进行销毁，而要使用`-D`参数进行强制销毁：  
  `git branch -D feature`  

### 多人协作

#### 添加合作者

1. 新建一个Repository
2. 进入Repository的Settings
3. 在Collaborators里就可以添加合作者了
4. 将生成的地址发给你的合作者，合作者选择是否同意
5. 接下来就可以愉快的合作开发了

#### 进行多人协作
  当你从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。
  * `git remote -v`  
  查看远程库信息，将会显示可以`fetch`或者`push`的`origin`地址，如果没有推送权限，就抓不到`push`地址  

  * 推送分支  
  `git push origin <branch>` 将分支推送到远程  
    然而，并不是所有分支都需要推送，一般来说只有需要与别人合作的分支才有必要推送到远程，如`master`分支、`dev`分支，但如果是修复小bug的分支则无必要  

  * 抓取分支  
  `clone`只能得到主分支，要抓取别的分支，要先clone，再使用以下命令：  
  `git checkout -b dev origin/dev`  
  然后就可以对这个分支进行操作啦，还可以时不时地`push`到远程   

  * 冲突解决  
  你的小伙伴已经向origin/dev分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送，那么就会出现冲突  
  **解决办法**  
  抓取远程`origin/dev`最新的提交，然后在本地处理好冲突再推送到远程，步骤如下：  
    1. 指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接：  
      `git branch --set-upstream dev origin/dev`  
    2. pull  
      `git pull`  
    3. 解决冲突  
      与本地解决冲突的方法一样，解决完后，再提交
    4. push到远程  
      `git push origin dev`  


## 标签

* `git tag <name>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id  
* `git tag -a <tagname> -m "blablabla..."` 可以指定标签信息  
* `git tag -s <tagname> -m "blablabla..."`可以用PGP签名标签  
* `git tag`可以查看所有标签   
* `git push origin <tagname>` 可以推送一个本地标签；
* `git push origin --tags` 可以推送全部未推送过的本地标签；
* `git tag -d <tagname>` 可以删除一个本地标签；
* `git push origin :refs/tags/<tagname>` 可以删除一个远程标签。

	
