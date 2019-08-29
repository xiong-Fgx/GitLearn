# 使用github
## 1. 生成SSH-key
- 首先确认有没有ssh-key：在主目录下进入.ssh文件中，如果没有sshkey的话，可以创建一个
    ```
    cd .ssh/
    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    eval "$(ssh-agent -s)"
    ssh-add ~/.ssh/id_rsa
    sudo apt-get install xclip
    xclip -sel clip < ~/.ssh/id_rsa.pub
    ```
- 然后将公钥复制到github的公钥里面，这样每次通过这台电脑进行push的时候就不需要每次都输入密码了

## 2. 与github仓库的repo关联
- 使用git remote对远端的repo进行操作
    ```
    git remote add <remote的名称> <从github上面clone的ssh协议地址>
    //表示用<remote的名称>来代替<从github上面clone的ssh协议地址>
    ```

- 下面是一个例子
    ```
    xiong@xiong-TM1703:~/GitLearn$ git remote add github_learning git@github.com:xiong-Fgx/GitLearn.git
    xiong@xiong-TM1703:~/GitLearn$ git remote -v
    github_learning	git@github.com:xiong-Fgx/GitLearn.git (fetch)
    github_learning	git@github.com:xiong-Fgx/GitLearn.git (push)
```

## 3. fetch命令
- 从远端将文件拉取到本地，使用git fetch + git merge将远端的内容拉取下来并与本地文件内容合并
- 也可以使用git pull直接拉取，不过这是一种不太安全的方式，尽量还是使用git fetch + git merge

## 4. push命令
- 将本地修改提交到远端 

  ```
  git push github_learning master
  //其中的第一个参数就是当初使用git remote时为该项目设置的名称，master表示分支的名称，可以改成其他的。
  ```