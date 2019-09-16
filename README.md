# Ubuntu安装GO并同步项目到Github

*环境：Ubuntu 18.04 LTS，64bit* 

### 一、下载&配置GO

- 下载

  到`https://golang.google.cn`网站选择适合的版本，下载GO文件并解压。

- 配置环境变量

  在终端中输入：

  ```shell
  $ vim ~/.profile
  ```

  打开`.profile`文件后，在文件末尾添加以下语句：

  ```shell
  export GOROOT=GO文件的位置
  export PATH=$PATH:$GOROOT/bin
  
  export GOPATH=工作目录的位置
  export PATH=$PATH:$GOPATH
  ```

  注意这里的“GO文件的位置”要替换为电脑上GO的实际位置，“工作目录的位置”要替换为电脑上工作目录的位置。然后在终端中输入以下语句更新环境变量并测试是否安装成功：

  ```shell
  $ source ~/.profile
  $ go
  ```

  假如看到go的信息输出，证明已经安装成功。可以通过输入以下语句来查看go的环境变量：

  ```shell
  $ go env
  ```

- 配置go的vscode插件

  vscode是一款十分流行的代码编辑器，然而因为`golang.org`被block了，不能直接在vscode内下载安装拓展，需要手动下载拓展的镜像并安装。

  首先，在终端中输入以下命令，创建拓展存放的文件夹，然后在该文件夹下下载拓展的镜像：

  ```shell
  $ mkdir $GOPATH/src/golang/x
  $ cd $GOPATH/src/golang/x
  $ git clone https://github.com/golang/tools.git tools
  ```

  假如电脑上没有安装git的话可以直接去`https://github.com/golang/tools`下载文件，并解压缩到`$GOPATH/src/golang/x/`文件夹。下载完成后，在终端运行：

  ```shell
  $ go install golang.org/x/tools/go/buildutil
  ```

  然后重新启动vscode，点击右下角提示安装的窗口，选择`install all`即可看到一堆

  >Installing xxx SUCCEEDED

  证明拓展安装成功。



### 二、编写hello.go并同步到远程仓库

- 创建项目

  以最简单的程序`hello.go`为例创建一个项目。首先，创建源代码目录，在终端输入以下语句：

  ```shell
  $ mkdir $GOPATH/src/github.com/github-user/hello -p
  ```

  在该目录下新建`hello.go`文件，使用vscode编辑以下内容：

  ```go
  package main
  
  import "fmt"
  
  func main() {
    fmt.Printf("hello, world\n")
  }
  ```

  在终端运行：

  ```shell
  $ cd $GOPATH/src/github.com/github-user/hello
  $ go run hello.go
  hello, world
  ```

  出现上面的输出，表示成功运行。

- 创建仓库

  首先我们需要配置git的账号信息：

  ```shell
  $ git config --global user.name "Your Name"
  $ git config --global user.email "email@example.com"
  ```

  这里的user.name和user.email要和Github的账户名、email相对应，否则无法同步到远程仓库。在Github上创建仓库后，在终端中输入：

  ```shell
  $ git clone https://github.com/HK40404/hello.git
  ```

  `https://github.com/HK40404/hello.git`是仓库地址，对不同的仓库要进行替换。输入完语句后会发现目录下多了个`hello`文件夹，将我们之前编写的`hello.go`复制进该文件夹，在终端输入以下语句，查看工作区状态：

  ```shell
  $ git status
  ```

  然后终端会输出信息，提醒你`hello.go`尚未添加到git当中。输入以下命令，将`hello.go`添加到仓库中：

  ```shell
  $ git add hello.go
  $ git status
  ```

  再次查看工作区状态，发现`hello.go`已经加入工作区。现在工作区已经发生了更改，所以我们要保存git的状态，假如不保存git的状态，同步到远程仓库时git就会是空的。在终端输入以下语句：

  ```shell
  $ git commit -m "adding hello.go"
  ```

  `-m`后面的字符是本次保存的注释，方便以后根据注释知道版本信息，并进行版本回退等操作。再次输入

  ```shell
  $ git status
  ```

  发现我们的分支是纯净的，因为所有改动已经保存。

- 同步到远程仓库

  在终端中输入：

  ```shell
  $ git push
  ```

  然后根据提示输入GitHub的账号和密码，即可将本地项目推送到远程仓库。

  
