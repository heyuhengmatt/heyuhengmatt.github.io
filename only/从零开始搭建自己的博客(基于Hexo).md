# 从零开始搭建自己的博客(基于Hexo)

因为云服务器的价格并不亲民，我们从免费的`GitHub Page`入手搭建一个属于自己的免费博客！

## 1.GitHub的相关准备工作

Github是一个代码托管仓库。

从名字我们很容易可以知道，我们可以把我们的代码上传到这个云端的仓库里。

##### 注册GitHub账号

https://github.com/

这个没什么好说的，登录GitHub官网完成注册，拥有你的GitHub账号。

##### 创建仓库

注册完成后我们会看到这样一个界面

<img src="https://kevinmatt-1303917904.file.myqcloud.com/20201108112626.png" alt="屏幕截图 2020-11-08 112518" style="zoom:33%;" />

红框选项就代表着你的代码仓库

<img src="https://kevinmatt-1303917904.file.myqcloud.com/20201108112806.png" alt="image-20201108112806012" style="zoom:33%;" >

关键来了：创建一个名字为`用户名.github.io`的仓库，比如我的ID为`heyuhengmatt`，那么就创建一个`heyuhengmatt.github.io`.

![image](https://kevinmatt-1303917904.file.myqcloud.com/20210117140742.jpeg)

创建好之后就先不管它，也不要着急关闭这个页面，咱们后面会用到。

##### 下载Git桌面客户端

https://git-scm.com/downloads

在Git官网可以下载到最新的Git客户端，但因为网站在境外可能对不能翻墙的小伙伴来说下载速度不太友好，所以我这里给出2.29.2的Git蓝奏云下载链接https://wwa.lanzous.com/i1nkzi6feyh

安装过程没什么好说的，一路下一步就行了。

安装完成后你的电脑上会出现Git Bash和Git GUI两个程序，一般我们使用Git Bash。

打开Git Bash可以看到这个和CMD很像的界面，没错，它也算是一个控制台

<img src="https://kevinmatt-1303917904.file.myqcloud.com/20201108113932.png" alt="image-20201108113931902" style="zoom: 50%;" />

到这里，我们的准备工作就差不多了，具体需要的指令操作后续会有讲到。

## 2.GitHubPage的配置

#### 配置全局登录GitHub及私钥

首先全局登录GitHub，并配置私钥：

```bash
git config --global user.name "用户名"
git config --global user.email "邮箱地址"
```

```bash
ssh-keygen -t rsa -C '上面的邮箱'
```

按照提示完成三次回车，即可生成 ssh key。通过查看 ~/.ssh/id_rsa.pub 文件内容，获取到你的 SSH key

![image](https://kevinmatt-1303917904.file.myqcloud.com/20210117141950.jpeg)

然后输入该命令测试：

```bash
ssh -T git@github.com
```

到这还没完，还要登录 [Github](https://github.com/) 上添加刚刚生成的SSH key，按以下步骤添加：

![image](https://kevinmatt-1303917904.file.myqcloud.com/20210117142039.jpeg)

创建一个新的 SSH key, 标题随便，key 就填刚才生成那个，确认创建，搞定！！这样在你的 SSH keys 列表里就会看到你刚刚添加的密钥。

![image](https://kevinmatt-1303917904.file.myqcloud.com/20210117142048.jpeg)

#### 博客的配置

在你打算放置博客本地文件的目录，打开`Git Bash`

- 方法一：直接在目录按住`shift`点右键“Git Bash here”

![image-20210117141107920](https://kevinmatt-1303917904.file.myqcloud.com/20210117141107.png)

- 方法二：在Git Bash里cd到指定目录

  ```bash
  cd C:\Users\he_yu\Desktop\myblog\hexoblog
  ```





然后在`Git Bash`中输入以下命令克隆你创建好的仓库(这里的username就是你的用户名，两个都要替换):

```bash
git clone https://github.com/username/username.github.io
```

clone完成后，键入一段命令写入Hello world到index内：

```bash
cd username.github.io
echo "Hello World" > index.html
```

然后将修改push上去：

```bash
git add --all
git commit -m "Initial Commit"
git push -u origin main
```

然后访问

**https://username.github.io**（username同样要替换为你的用户名）

应该就能看到一个HelloWorld的页面，此时表明GithubPage配置成功。

## 3.Hexo的本地配置

### 1.准备工作

- Nodejs：

  直接在[Nodejs中文网](http://nodejs.cn/download/)下载安装即可

在命令行(可以是Powershell或者cmd)输入以下命令：

```bash
git version
npm -v
node -v
```

均有对应版本号显示则表明安装正常

然后我们安装hexo到本地，首先在你想要存放博客的目录打开gitbash

```bash
npm install -g hexo -cli
```

这个过程应该不会很快，慢慢等待其完成，然后输入以下命令：

```bash
hexo init myBlog
cd myBlog
npm install
```

如果上面的命令都没有报错的话，在当前目录输入以下命令：

```bash
hexo cl&&hexo s
```

然后打开任意浏览器，输入localhost:4000，可以看到以下页面，表明hexo本地部署成功：

![image-20210117142845425](https://kevinmatt-1303917904.file.myqcloud.com/20210117142845.png)

### 2.配置工作

在你存放博客的目录下，应该可以看到这样的文件结构：

> .
> ├── _config.yml # 网站的配置信息，您可以在此配置大部分的参数。 
> ├── package.json
> ├── scaffolds # 模版文件夹
> ├── source  # 资源文件夹，除 _posts 文件，其他以下划线_开头的文件或者文件夹不会被编译打包到public文件夹
> |   ├── _drafts # 草稿文件
> |   └── _posts # 文章Markdowm文件 
> └── themes  # 主题文件夹

##### 1.配置部署仓库

打开你目录下的`_config.yml`，找到`deploy`字段，填入以下内容：

![image](https://kevinmatt-1303917904.file.myqcloud.com/20210117143152.jpeg)

##### 2.安装部署插件

在GitBash中键入：

```bash
npm install hexo-deployer-git --save
```

执行以下命令部署上传：

```bash
hexo g -d
```

如果前面的私钥配置之类的操作都完成过了，等待几分钟你就能看到你的username.github.io的页面和本地预览的一致了！

## 4.开始写作

部署完成之后，你就可以使用`Markdown`进行文档写作了！

还是gitbash，在博客目录键入：

```bash
hexo new 文档标题
```

hexo将自动在目录下`source/_post`中为你生成一个同名的`Markdown`文件，并自带其生成时间,打开这个md文件就可以开始按照`Markdown`标准进行写作了！每次写作完成之后使用部署命令`hexo g -d`就大功告成！

