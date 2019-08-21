[TOC]
* [安装](#安装)
* [Linux](#Linux)

# 安装
## Linux
- 先在终端输入 `git` 命令，测试是否自带或已安装；
- 如果未安装，输入`sudo apt-get install git`
- 老一点的版本输入：`sudo apt-get install git-core`	//因为原来有个叫git的软件（GNU Interactive Tools），后来改名了，git才有机会使用新命令安装。
- 还可以通过源码安装，下载源码解压，然后进入目录，依次输入以下命令：`./config`，`make`，`sudo make install`就安装成功。

## Windows

1. 下载git：https://git-scm.com/download/win

2. 选择默认安装即可

3. 设置git，git是要设置提交者的信息才能进行操作

   ```shell
    $ git config --global user.name "Your.Name"
    $ git config --global user.email "email@example.com"
    例如：
    	$ git config --global chuan-xian.name
    	$ git config --global chuan-xian.123456@qq.com
   ```



# 初次使用

## 1. 创建仓库
就是创建一个空目录

``` shell
	$  mkdir Git
	$  cd Git
```

## 2. 管理仓库

通过`git init`命令把这个目录变成`Git`可以管理的仓库
```shell
$ git init
Initialized empty Git repository in E:/Git/location/Git/git/.git/
```
- <b>注意：</b>
  1. 这样就创建了一个.git的文件夹，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把`Git`仓库给破坏了。
  2. 如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见。
  3. `git`更多地是只针对纯文本文件，对于二进制文件、word等文件，git只知道改动了，如100kb变成了200kb，但具体改动了什么，`git`不知道，此外，请勿使用记事本编辑(有编码问题)，建议使用`Notepad++`。
  4. 路径请不要使用中文，建议使用中文。

## 3. 添加文件到Git仓库

这里分两步：

 	1. 使用命令`git add <file_name>`，注意，可反复多次使用，添加多个文件，也可以一次指定多个文件，这是添加到苍仓库缓存，但是还未提交；
 	2. 使用命令`git commit [file_name] -m <message>`，将修改后的文件提交到仓库。

## 4. 查看命令

1. `git status`命令可以让我们时刻掌握仓库当前的状态，所以如果要随时掌握仓库状态，就使用该命令。注意，执行`add`命令后还可以显示哪些文件被修改了，但是如果`commit`了就无法显示信息了。
2. `git diff`可以查看修改了哪些行，前提是没有执行`add`、`commit`命令，执行任何一个都将没有输出，因为它查看的是工作区的变化，一旦`add`了就工作区的变化消除了。
3. 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。
4. 如果`add`或`commit`了，那就可以使用`git log`查看修改记录。

## 5. 分支命令

1. 查看当前有哪些分支：`git branch`。
2. 创建一个分支：`git checkout -b Branch_Name`。
3. 切换分支：`git checkout Branch_Name`。

# 版本控制

## 1. `git log`命令：
1. `git log`命令查看对文件修改的历次相关信息，命令显示从最近到最远的提交日志。注意这里不是所有版本都列出来，而是根据修改的时间顺序，将当前版本之前的版本都列出来，如果回退了，那就不会显示本版本之后的版本。
2. 输出的信息中，第一行`commit`后面是一个`SHA1`计算出来的一个非常大的数字，用十六进制表示，用来唯一表示每一次提交的版本。其实，`git`当用户在某个分支下每提交文档的一个新版本到仓库以后，都会在这个分支下，依次把各个版本的文档，按照时间的先后顺序，串成一条时间线，相当于一棵树。
3. 当前版本会在`commit`一行的末尾用`HEAD`指明。
4. `git log --pretty=oneline`只输出每份修改信息的第一行，即输出`commit`一行。

## 2. `git reset --hard HEAD^`

1. 一个`HEAD^`表示回退一个版本，两个`^`表示回退两个版本，`HEAD^^`，多个版本回退可以直接`HEAD~n`，`n`为一个整数，表示要回退多少个版本。
2. 撤销回退：其实`HEAD`就像一个指针，执行命令的时候，`--hard`指向哪个`commit id`，它就恢复到哪个版本。所以撤销回退的时候，可以用`git reset --hard id`，这里的`id`就是`git log`命令输出的`commit`行后计算出来的`SHA1`，不需要写完整，只需要达到计算机可辨识的程度，它就会自动寻找，一般写四五位就足够了。
3. 本质，每当修改版本的时候，`HEAD`就是一个指针，将指针指向不同的版本，再把工作区的文件更新。
4. 查找`commit id`，一串`SHA1`编码的`commit id`，记住是不可能的，但是版本回退的时候，可能需要再新旧版本之间切换选择，这时候需要知道`commit id`才能进行。`git reflog`记录了相关信息。

## 3. 工作区和版本库

1. 工作区就是工作目录，`pwd`命令可以查看。
2. 工作区目录中有一个`.git`的文件夹，这就是版本库。版本库里面有一个最重要的文件`stage`（或者叫`index`），这个文件叫暂存区，还有`Git`创建的一个默认分支`master`，还有指向`master` 的`HEAD`指针。
3. 当我们在当前工作区创建一个文件时，只是单单存在与当前的工作目录中。当我们使用`add`命令的时候，才把该文件提交到暂存区。当我们把文件`commit`时，就是把暂存区这个文件提交到`Git`的当前分支下，这时候才能正式对该文件进行版本管理。<b>注意：</b>`add`只把工作区的文件提交到暂存区，`commit`只把暂存区的文件提交到`git`版本库。

## 4. 撤销修改

1. `git checkout -- <file_name>`：这里会产生两种情况，①一种是`<file_name>`修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；②一种是把`<file_name>`已经添加到暂存区后，又作了`<file_name>`这个文件做了修改，现在，撤销修改就回到添加到暂存区后的状态。总之，本命令就是让这个文件回到最近一次`git commit`或`git add`时的状态。

2. 还有一种情况，`<file_name>`文件写错了，但是提交到了暂存区，此时如果修改了文件，就`git checkout -- <file_name>`回退到提交后还未修改文件时的状态，但此时肯定还需要进一步撤回暂存区的文件和修改本地文件。`git reset HEAD <file_name>`，可以把暂存区的修改撤销掉`（unstage）`，重新放回工作区，就是回到文件提交到暂存区前的状态。
3. 如果提交到了版本库，那就回退版本库后再修改，但是如果推送到了远程版本库，那就没法修改了。
4. <b>总之：</b>
   1. 当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`
   2. 当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`命令回到上一步，然后再执行上一步
   3. 已经提交了不合适的修改到版本库时，想要撤销本次提交，执行版本回退

## 5. 删除文件

1. 如果删除一个文件，对应有两种情况：①一是确实要删除该文件，那就把版本库中的对应文件也删除，用`git rm <file_name>`即可，然后再`git commit -m <message>`保存一下修改的解释内容。②如果是误删，那就需要从版本库中恢复该文件，使用`git checkout -- <file_name>`即可恢复文件（该命令使用的是暂存区的文件恢复工作区的文件）。

# 远程仓库
## 1. 创建ssh连接秘钥对

1. 创建`SSH Key`。在用户主目录下，看看有没有`.ssh`目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开`Shell`（`Windows`下打开`Git Bash`），创建`SSH Key`，中间全部选默认即可：

```shell
$ ssh-keygen -t rsa -C "youremail@example.com"
在用户主目录或当前目录下，有一个.ssh 的目录，里面会有id_rsa(私钥)和id_rsa.pub(公钥)这两个文件
```

2. 登陆`GitHub`，打开`Account --> settings`，`SSH and GPG keys`页面，点击右上角的`New SSH key`添加，`Title`任意填写，在`Key`文本框里粘贴`id_rsa.pub`文件的内容。最后点击`Add SSH key`，你就应该看到已经添加的`Key`。

3. `GitHub`需要`key`是因为它需要识别出是你提交的文件，所以`GitHub`以`SSH`的方式，知道你的公钥，然后你可以用私钥去配对，配对成功就可以向其中提交文件，所以你可以配置多个私钥公钥对，这样就能在各处用不同的钥对工作，也可以给不同的人分配不同的密钥，从而协同办公。但是`GitHub`免费版本的仓库只能是公开的，私人版收费。因此可以多人协同维护一个仓库，但是别人都可以看到，故不要泄露个人隐私信息。

## 2. 创建远程仓库

现在本地有了一个仓库，但是现实情况是我们需要一个远程仓库，以来可以作为备份，二来可以作为一个共享仓库，和他人协同作业。

1. 登录`GitHub`，点击右上角`+`号，点击下方的`New repository`，然后输入仓库名字，其余保持默认，创建仓库。

2. 把本地仓库和远程仓库关联起来：

   ```shell
   $ git remote add origin git@github.com:chuan-xian/My_First_Repository.git
   # chuan-xian是GitHub用户名，My_First_Repository是仓库名。
   ```

3.  把本地仓库的内容推送到远程库：

   ```shell
   $ git push -u origin master
   # git push是推送命令，把当前分支master推送到远程库；
   # 第一次推送master分支，加上了 -u 参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
   # origin是远程库的名字，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。
   ```

排错：

1. 当你第一次使用`Git`的`clone`或者`push`命令连接`GitHub`时，会得到一个警告：(输入`yes`即可)

```
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
```

```
# 输出如下信息：告诉你已经把GitHub的Key添加到本机的一个信任列表里了，这个警告仅出现一次。
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
```

2. 连接过程报错

```shell
$ git remote add origin git@github.com:chuan-xian/My_First_Repository.git
$ git push -u origin master
The authenticity of host 'github.com (52.74.223.119)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? y
Please type 'yes', 'no' or the fingerprint: yes
Warning: Permanently added 'github.com,52.74.223.119' (RSA) to the list of known hosts.
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

# 1. 删掉原来的id_rsa和id_isa.pub文件
# 2. git config --global user.name "your_github_name"
$ git config --global chuan-xian.name
# 3. git config --global user.email "your_github_email"
$ git config --global chuan-xian.916165918@qq.com
# 4. cd ~/.ssh/
# 5. ssh-keygen -t rsa -C "your_github_email"	//生成密钥，公钥上传，私钥保留
$ ssh-keygen.exe -t rsa -C "916165918@qq.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/xx/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/xx/.ssh/id_rsa.
Your public key has been saved in /c/Users/xx/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:mAFO0Rdl467B+Civr2wnX23zboEhATatfQiSkvrwD+4 916165918@qq.com
The key's randomart image is:
+---[RSA 3072]----+
|   .+++o.o+      |
|  ooooo.+o .     |
| . ....= o.      |
|o     .*+.o      |
| +    + So.o     |
|  +    o +. .    |
| . o. . + +  .   |
|  ..+o.. . o.    |
| .E.+B+    oo    |
+----[SHA256]-----+
# 6. 测试ssh连接是否成功
$ ssh git@github.com
Enter passphrase for key '/c/Users/xx/.ssh/id_rsa':
PTY allocation request failed on channel 0
Hi chuan-xian! You've successfully authenticated, but GitHub does not provide shell access.
Connection to github.com closed.
# 7. 第6步显示连接失败，解决方法：
$ ssh -v git@github.com
OpenSSH_8.0p1, OpenSSL 1.1.1c  28 May 2019
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: Connecting to github.com [13.229.188.59] port 22.
debug1: Connection established.
debug1: identity file /c/Users/\345\263\260/.ssh/id_rsa type 0
debug1: identity file /c/Users/\345\263\260/.ssh/id_rsa-cert type -1
debug1: identity file /c/Users/\345\263\260/.ssh/id_dsa type -1
debug1: identity file /c/Users/\345\263\260/.ssh/id_dsa-cert type -1
debug1: identity file /c/Users/\345\263\260/.ssh/id_ecdsa type -1
debug1: identity file /c/Users/\345\263\260/.ssh/id_ecdsa-cert type -1
debug1: identity file /c/Users/\345\263\260/.ssh/id_ed25519 type -1
debug1: identity file /c/Users/\345\263\260/.ssh/id_ed25519-cert type -1
debug1: identity file /c/Users/\345\263\260/.ssh/id_xmss type -1
debug1: identity file /c/Users/\345\263\260/.ssh/id_xmss-cert type -1
debug1: Local version string SSH-2.0-OpenSSH_8.0
debug1: Remote protocol version 2.0, remote software version babeld-216c4091
debug1: no match: babeld-216c4091
debug1: Authenticating to github.com:22 as 'git'
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: algorithm: curve25519-sha256
debug1: kex: host key algorithm: rsa-sha2-512
debug1: kex: server->client cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: kex: client->server cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
debug1: Server host key: ssh-rsa SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8
debug1: Host 'github.com' is known and matches the RSA host key.
debug1: Found key in /c/Users/\345\263\260/.ssh/known_hosts:1
debug1: rekey out after 134217728 blocks
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug1: SSH2_MSG_NEWKEYS received
debug1: rekey in after 134217728 blocks
debug1: Will attempt key: /c/Users/\345\263\260/.ssh/id_rsa RSA SHA256:mAFO0Rdl467B+Civr2wnX23zboEhATatfQiSkvrwD+4
debug1: Will attempt key: /c/Users/\345\263\260/.ssh/id_dsa
debug1: Will attempt key: /c/Users/\345\263\260/.ssh/id_ecdsa
debug1: Will attempt key: /c/Users/\345\263\260/.ssh/id_ed25519
debug1: Will attempt key: /c/Users/\345\263\260/.ssh/id_xmss
debug1: SSH2_MSG_EXT_INFO received
debug1: kex_input_ext_info: server-sig-algs=<ssh-ed25519,ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521,ssh-rsa,rsa-sha2-512,rsa-sha2-256,ssh-dss>
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug1: Authentications that can continue: publickey
debug1: Next authentication method: publickey
debug1: Offering public key: /c/Users/\345\263\260/.ssh/id_rsa RSA SHA256:mAFO0Rdl467B+Civr2wnX23zboEhATatfQiSkvrwD+4
debug1: Server accepts key: /c/Users/\345\263\260/.ssh/id_rsa RSA SHA256:mAFO0Rdl467B+Civr2wnX23zboEhATatfQiSkvrwD+4
Enter passphrase for key '/c/Users/峰/.ssh/id_rsa':
debug1: Authentication succeeded (publickey).
Authenticated to github.com ([13.229.188.59]:22).
debug1: channel 0: new [client-session]
debug1: Entering interactive session.
debug1: pledge: network
PTY allocation request failed on channel 0
debug1: client_input_channel_req: channel 0 rtype exit-status reply 0
Hi chuan-xian! You've successfully authenticated, but GitHub does not provide shell access.
debug1: channel 0: free: client-session, nchannels 1
Connection to github.com closed.
Transferred: sent 3412, received 2376 bytes, in 0.7 seconds
Bytes per second: sent 4993.1, received 3477.1
debug1: Exit status 1
# 8. ssh-agent.exe -s
$ ssh-agent.exe -s
SSH_AUTH_SOCK=/tmp/ssh-Miia4ofig6sb/agent.2395; export SSH_AUTH_SOCK;
SSH_AGENT_PID=2396; export SSH_AGENT_PID;
echo Agent pid 2396;
# 9. 添加私钥ssh-add ~/.ssh/id_rsa
$ ssh-add ~/.ssh/id_rsa
Identity added: /c/Users/xx/.ssh/id_rsa (916165918@qq.com)
# 10. 如果报错，继续执行
$ eval ssh-agent -s 
Agent pid 24460 
# 11. 再次执行ssh-add ~/.ssh/id_rsa
$ ssh-add ~/.ssh/id_rsa
Identity added: /c/Users/xx/.ssh/id_rsa (916165918@qq.com)
# 12. 最后验证key
$ ssh -T git@github.com
Hi chuan-xian! You've successfully authenticated, but GitHub does not provide shell access.
# 13. 报错
remote: error: GH007: Your push would publish a private email address.
解决方法：登录GitHub账号，setting->emails->Keep my email address private，把这一项去掉勾选即可。
```

<b>总结：</b>

1. 配置全局文件

```shell
$ git config --global chuan-xian.name
$ git config --global chuan-xian.916165918@qq.com
```

2. 生成密钥，私钥保留，公钥上传

```shell
$ ssh-keygen.exe -t rsa -C "916165918@qq.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/峰/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/峰/.ssh/id_rsa.
Your public key has been saved in /c/Users/峰/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:aJaZa1lkxlG5Sfks6ZY92Ga5+j00n7E7sF4mKpxRRnk 916165918@qq.com
The key's randomart image is:
+---[RSA 3072]----+
|        ...o.    |
|       . .+o E   |
|        =..*.    |
|       O  =oo    |
|      B S.o* .   |
|     o + .= B.o. |
|      + ..oo =o=+|
|     .   +  oo=+.|
|          o+o..oo|
+----[SHA256]-----+
```

3. 本地添加私钥

```shell
$ ssh-add.exe ~/.ssh/id_rsa
Enter passphrase for /c/Users/峰/.ssh/id_rsa:
Identity added: /c/Users/峰/.ssh/id_rsa (916165918@qq.com)
$ ssh-add my_key
Could not open a connection to your authentication agent.
$ ssh-agent bash
```

4. 测试是否能连接成功

```shell
$ ssh -T git@github.com
Hi chuan-xian! You've successfully authenticated, but GitHub does not provide shell access.
# 不通就是'ssh-agent -s'  'ssh-add ~/.ssh/id_rsa' 操作这两步
```

5. 把本地仓库和远程仓库关联起来

```shell
$ git remote add origin git@github.com:chuan-xian/My_First_Repository.git
```

6. 把本地仓库的内容推送到远程库

```shell
$ git push -u origin master	
#  -u 参数， Git 不但会把本地的 master 分支全部内容推送到远程新的 master 分支，还会把本地的 master 分支和远程的 master 分支关联起来。
# origin是指远程仓库的分支名称，master是本地仓库的名称，如果要推送不同的本地仓库到不同的远程仓库，需要修改仓库名。
```
7. 切换到其他仓库
```shell
# 有时候当前仓库的工作完成后，需要对其他仓库进行管理，这时候需要切换仓库，首先就要删除当前git库关联的远程仓地址。
$ git remote remove origin  
# 然后再执行第五步和第六步，第五步的后面仓库名改为目标仓库。
```


## 3. 远程克隆仓库

1. 远程的仓库可以通过`git clone`命令克隆到本地

```shell
$ git clone git@github.com:chuan-xian/test.git
```

2. 克隆到本地的库，存储在主目录下的一个目录中，目录名就是库名。

3. `Git`支持多种协议，包括`https`，默认的`git://`使用`ssh`，但也可以使用`https`等其他协议，但通过`ssh`支持的原生`git`协议速度最快。`GitHub`给出的地址也不止一个，还可以用`https://github.com/michaelliao/gitskills.git`这样的地址。

# 分支管理

## 1. 创建分支

``` shell
$ git branch <branch_name>
```

## 2. 切换分支

```shell
$ git checkout <branch_name>
Switched to branch 'branch_name'
```

## 3. 综合命令

```shell
$ git checkout -b <branch_name>
Switched to a new branch 'branch_name'
# 相当于上面两条命令，创建分支并切换到新的分支
```

## 4. 查看分支

```shell
$ git branch
* master
  <other_branch_name>...
```

## 5. 合并分支

```shell
$ git merge <branch_name>
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
# 把<branch_name>这个分支合并到当前分支上来
```

## 6. 删除分支

```shell
$ git branch -d <branch_name>
Deleted branch <branch_name> (was b17d20e).
# 如果分支被修改后，又没有被合并，那么git会提示删除就将丢失修改，可以用-D强行删除。
```

## 7. 合并冲突

在5中只能快速合并，有的多个分之下都对文件进行了修改，可能造成无法合并，这需要先解决冲突。

1. 手动修改冲突文件，把内容编辑为我们想要的内容，然后保存。
2. 一定要先`git add/commit`该文件，然后再合并，否则无法合并分支。
3. 如果有需要，可以删除不必要的分支。
4. 用`git log --graph`命令可以看到分支合并图。

## 8. 保留历史版本

1. 第5、7都不保留历史版本，如果想要保留历史版本，就要在合并的时候加上`--no-ff`，这样原来的修改版本也保留下来了。
2. `master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活。
3. 干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如`1.0`版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布`1.0`版本；

## 9. 临时修复bug

当需要停下当前开发，修复一个临时`bug`任务的时候，一般是创建一个新的分支，去处理临时的`bug`，处理完后合并到`master`分支，最后删除临时分支。

但是当前分支需要暂时挂起来。

```shell
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge
# 挂起当前工作的分支
```

处理完临时`bug`任务后，切换回原来的分支，用`git status`会发现工作目录是干净的，这时需要

```shell
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
# git stash list查看
```

恢复工作：

1. 用用`git stash apply`恢复，但是恢复后，`stash`内容并不删除，你需要用`git stash drop`来删除；

```shell
$ git stash apply
$ git stash drop
```

2. 用`git stash pop`，恢复的同时把stash内容也删了：

```shell
$ git stash pop
# 此时再用git stash list就查看不到任何内容了。
```

3. 一个`bug`既然在`master`上需要修复，那么肯定在`dev`上也需要修复，因为`dev`就是由`master`分支而来的。把`bug`处理修改好之后提交到`master`上的时候(即把`bug`修改好以后，分别执行`add/commit`命令后，出现的`commit_id`)，会有一个`commit id`，把这个`commit id`直接作用到`dev`上，也能起作用。相当于这个`bug`修复也合并到了`dev`分支上一样。

```shell
$ git cherry-pick <commit_id>
# 这样就把<commit_id>所对应的bug修复方案应用于当前分支上了，故要先切换到需要应用的分支上，比如这里是dev
```

## 10. 查看远程仓库

```shell
$ git remote
# 查看远程仓库
$ git remote -v
# 查看远程仓库详细信息
```

## 11. 创建远程分支

```shell
$ git checkout -b dev origin/dev
# 同时创建一个本地dev分支，也创建一个远程的dev分支，(第二个origin/dev分支)
```

## 12. 多人写作

```
1. 首先，可以试图用git push origin <branch_name>推送自己的修改；

2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

3. 如果合并有冲突，则解决冲突(即手动编辑文件)，并在本地提交；

4. 没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

5. 如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

6. 这就是多人协作的工作模式。
```



































































