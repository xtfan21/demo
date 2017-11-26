# Git的4个阶段的撤销更改
工作中会遇到需要撤销更改的情况，知道有一个git reset --hard的指令能回退，由于用的不频繁所以总忘记在什么情况具体结合什么指令使用。这篇文章从基本概念到检查修改到撤销修改循序渐进地介绍了在各种情况下的需要用到的git指令。


正文从这里开始
正常情况下，我们的工作流就是 3 个步骤：

工作区---暂缓区---本地仓库---远程仓库

git add .   把所有文件放入 暂存区；
git commit -m "comment"  把所有文件从 暂存区 提交进 本地仓库；
git push 把所有文件从 本地仓库 推送进 远程仓库。

4个区
git 之所以令人费解，主要是它相比于 svn 等等传统的版本管理工具，多引入了一个 暂存区 ( Stage )的概念，就因为多了这一个概念，而使很多人疑惑。其实，在初学者来说，每个区具体怎么工作的，我们完全不需要关心，而只要知道有这么 4 个区就够了：

工作区( Working Area )

暂存区( Stage )

本地仓库( Local Repository )

远程仓库( Remote Repository )

5种状态
以上 4 个区，进入每一个区成功之后会产生一个状态，再加上最初始的一个状态，一共是 5 种状态。以下我们把这 5 种状态分别命名为：

未修改( Origin )

已修改( Modified )

已暂存( Staged )

已提交( Committed )

已推送( Pushed )

##一、检查修改
1、已修改，未暂存
git diff
2、已暂存，未提交
git diff --cached
3、已提交，未推送
git diff master origin/master
##二、撤销修改
1、已修改，未暂存
如果我们只是在编辑器里修改了文件，但还没有执行 git add . ，这时候我们的文件还在 工作区 ，并没有进入 暂存区 ，我们可以用：

git checkout .
或者

git reset --hard
2、已暂存，未提交
你已经执行了 git add . ，但还没有执行 git commit -m “comment” 。这时候你意识到了错误，想要撤销，你可以执行：

git reset
git checkout .
或者

git reset --hard
3、已提交，未推送
你的手太快，你既执行了 git add . ，又执行了 git commit ，这时候你的代码已经进入了你的 本地仓库 ，然而你后悔了，怎么办？不要着急，还有办法。

git reset --hard origin/master
还是这个 git reset —hard 命令，只不过这次多了一个参数 origin/master ，正如我们上面讲过的， origin/master 代表 远程仓库 ，既然你已经污染了你的 本地仓库 ，那么就从 远程仓库 把代码取回来吧。
4、已推送
很不幸，你的手实在是太快了，你既 git add 了，又 git commit 了，并且还 git push 了，这时你的代码已经进入 远程仓库 。如果你想恢复的话，还好，由于你的 本地仓库 和 远程仓库 是等价的，你只需要先恢复 本地仓库 ，再强制 push 到 远程仓库 就好了：

git reset --hard HEAD^
git push -f
总结：以上 4 种状态的撤销我们都用到了同一个命令 git reset —hard ，前 2 种状态的用法甚至完全一样，所以只要掌握了 git reset —hard 这个命令的用法，从此你再也不用担心提交错误了。
