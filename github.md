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
    ```
    xiong@xiong-TM1703:~/GitLearn$ git remote add github_learning git@github.com:xiong-Fgx/GitLearn.git
    xiong@xiong-TM1703:~/GitLearn$ git remote -v
    github_learning	git@github.com:xiong-Fgx/GitLearn.git (fetch)
    github_learning	git@github.com:xiong-Fgx/GitLearn.git (push)

    ```
    
## 5. qita
