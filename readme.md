# git

## 1. git的初始化
- 在当前目录下创建：
    ```
    git init
    ```
- 设置管理者的名称及邮箱：
    - 注意这里选项可以填写local、global、system，优先级从高到低排列
    ```
    git config --global user.name '名称'
    git config --global user.email '填入email'
    ```
- 查看当前库的状态：
    - 可以查询得到当前放在暂存区或者工作区中各个修改过的文件的状态
    - 主要是查询是否可以进行提交
    ```
    git status 
    ```
- 查看代码库的更改情况：
    - 这个命令只针对于commit的记录进行的记录，如果只是add则无法从log进行查询。
    ```
    git log 
    ```

## 2. git的提交文件
- 将文件添加入暂存区：
    ```
    git add 文件名
    ```
    - 或者通过下面的命令，将全部更改的文件都加入暂存区，注意这里只能是**更改的文件**，而新添加进代码库的文件是**不可以**通过这种方式进行更新的，只能通过add加文件名的方式进行更新。
    ```
    git add -u 
    ```

- 从暂存区中删除文件：
    ```
    git rm 文件名
    ```

- 将暂存区的更改提交：
    ```
    git commit -m '这里加入提交的说明'
    ```
- 直接将工作区的修改添加到版本库中，而不需要经过add操作。
    ```
    git commit -am'text'
    ```

## 3. git重命名一个文件
- 这是一个组合使用的方法，综合上面一节所提到的内容进行重命名的应用。
- 首先使用mv命令，将原名称改变为新名称，然后将新名称add进来，将原名称rm掉。
- 这时候git会识别出来这个操作是rename。
    ```
    xiong@xiong-TM1703:~/GitLearn$ mv readme readme.md
    xiong@xiong-TM1703:~/GitLearn$ git add readme.md
    xiong@xiong-TM1703:~/GitLearn$ git rm readme
    rm 'readme'
    xiong@xiong-TM1703:~/GitLearn$ git status
    On branch master
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        renamed:    readme -> readme.md

    xiong@xiong-TM1703:~/GitLearn$ 
    ```
- 另外一种方法：使用git mv
    ```
    git mv readme.md readme
    ```

## 4. git清除工作区和暂存区的所有内容
- **注意：这是一个很危险的操作**，不仅add进暂存区的所有修改会被清除，而且**放在工作区中的修改过的所有文件**也会被复原。
- 注意：历史已经完成的修改不会被改变！
    ```
    git reset --hard
    ```

## 5. 使用gitlog查看版本演变历史
- 使用 --oneline 参数表示使用一行的形式简洁的表示出版本的历史
- 使用 -n4 参数表示显示最近的4次更改，当然这个4也可以换成其他数
- 上面的两个命令可以同时使用，表示简洁的显示出最近的4个更改

    ```
    git log --oneline
    git log -n4 --oneline
    ```
- 使用树状图的形式查看分支更改情况：
    ```
    git log --graph
    ```
- 查看其他的分支信息：
    ```
    git log 分支名
    ```
- 查看所有的分支信息：
    ```
    git log --all
    git log --oneline --all
    ```
- 因为各种不同的命令行参数可以叠加，因此下面这条就比较全面了
    ```
    git log --oneline --all -n4 --graph使用树状图的形式查看
    ```

## 6. 分支
- 使用git branch -v查看本地有多少分支，-av表示查看分支并对分支进行描述
    ```
    git branch -v  <-av>
    ```
- 创建新的分支：使用checkout命令进行创建，如果要从某一个commit开始新开一个分支的话，需要知道这个分支的commit号（一般前几位就可以了），下面的指令表示新的分支名称为temp
    ```
    git checkout -b temp 这里填入commit号
    ```
- 切换到其他分支：直接用checkout + 分支名即可
    ```
    git checkout master
    ```
- 删除某个分支： -d是一种安全的删除方法，-D是一种强制的删除方法
    ```
    git branch -d <commit>
    //如果需要强制删除某个分支的话，需要将命令行参数中的小写d换为大写D
    git branch -D <commit>
    ```

- 分离头指针（deteached head）：直接将某个commit的ID号作为checkout的对象，这样会基于这个commit进行修改，属于分离头指针。但是此时切换到其他的分支之后，这部分的修改都会全部消失。也就是**当前的HEAD与任何分支都不绑定，变更不是基于某个branch的。**
    - 如果想要保存下来，就需要使用
        ```
        git branch <new branch name> ID号
        ```

## 7. HEAD与branch
- HEAD表示当前的指针位置，diff表示对两个提交进行对比
- HEAD^1, HEAD^都表示HEAD的父亲，这里^后面的1不可以改变，但是可以加很多个^。
- HEAD~2表示HEAD的父亲的父亲，这里的2表示的意思就是父辈的代数。
    ```
    git diff commit1 commit2
    //下面3个表示相同的含义 
    git diff HEAD~2 HEAD
    git diff HEAD^^ HEAD
    git diff HEAD^1^1 HEAD
    ```

## 8. 对commit进行修改
- 对最近的一个commit进行修改
    ```
    git commit --amend
    ```
- 对前面的历史commit进行修改，-i表示交互式的，这里的**commit号要填写需要变更的commit的父亲的commit号**。这里就是用到分离头指针，先分离头指针，然后更改commit指向的地方，**此时后面所有的commit号都会发生变化！**。因此，如果是多人合作的话，不能够使用这种方式进行修改，因为这样可能会影响到其他人的操作。
    ```
    git rebase -i <需要编辑的commit的父亲commit号>
    //然后可以得到一个新的页面，在这个页面里将pick指令换为reword或者r，然后保存退出，此时将弹出另一个交互界面，在这个交互界面中编写新的commit内容。
    //这里，如果写错的话，可以通过--edit-todo和--continue进行补救。
    ```
    
- 将连续的几个commit合并为同一个
    ```
    git rebase -i <需要合并的最老的commit的父亲commit号>
    //在里面将前后中间的pick全部用s替换，然后保存退出，此时弹出另一个交互界面，在这个界面里填写新的commit内容。
    ```
- 将不同位置的commit合并为同一个
    ```
    //还是使用rebase指令，但是不同之处在于这里需要将两个需要合并的位于不同位置的commit放到一起，然后把需要合并的那一项从pick换为s
    ```
## 9. 工作区与暂存区
- 比较暂存区与HEAD所含文件的差异
    ```
    git diff --cached
    ```
- 比较工作区与暂存区所含文件的差异
    ```
    git diff //就默认的diff就是比较工作区与暂存区的区别。
    //如果只想比对其中一个文件的差别，就需要
    git diff -- readme.md
    ```
