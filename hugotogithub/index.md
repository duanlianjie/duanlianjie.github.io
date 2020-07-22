# 将Hugo生成的静态网页部署到Github


## 将Hugo生成的静态网页部署到Github

### 前提条件

1. 成功安装Hugo，在源文件目录下运行`hugo serve`时可以在浏览器输入`localhost:1313`查看到网页。
2. 在源文件目录下运行`hugo`生成public目录。
3. 在config.toml中配置参数`baseURL = "https://<userName>.github.io/"`（userName为Github账号ID，下同）。
   - 这一步很关键，如果没有设置最后在浏览器输入`https://<userName>.github.io/`会查看不到网页。
   - 除此之外，也可以选择在第二步生成public时运行`hugo --baseURL="https://<userName>.github.io/"`

### 准备部署

Hugo官网英文文档给出的部署步骤试了一下没有成功，有几处细节说明不够详细，所以在其基础上给出我的部署步骤。

1. 在Github新建一个代码仓，代码仓名字为`<userName>.github.io`。然后将public目录下得文件push到该代码仓。push很简单，在public目录下依次执行：
   1. `git init`
   2. `git remote add origin git@github.com:<userName>/<userName>.github.io.git`
   3. `git add .`
   4. `git commit -m "first commit"`
   5. `git push -u origin master`

>  其实这个时候已经可以访问`https://<userName>.github.io/`查看网页了，但是后续需要上传新的文章到博客，所以需要进一步部署。

2. 在Github新建一个空的代码仓，代码仓名字随意，比如blog。将其clone到本地，然后打开blog文件夹，将hugo项目的源文件全部复制到该目录下。此时**将public目录删除**，随后重复第一步的push命令，将hugo项目的源文件push到blog代码仓。

3. 在blog目录下运行如下命令，把`<userName>.github.io`代码仓添加为blog代码仓的子模块。

   ```bash
   git submodule add -b master https://github.com/<userName>/<userName>.github.io.git public
   ```

4. 编写快速更新网页的脚本：

   1. `vim deploy.sh`，复制以下代码

   ```bash
   #!/bin/sh
   
   # 如果命令行失败，停止部署
   set -e
   printf "\033[0;32mDeploying updates to GitHub...\033[0m\n"
   hugo
   
   # 更新上述中blog代码仓
   git add .
   msg="rebuilding site $(date)"
   if [ -n "$*" ]; then
   	msg="$*"
   fi
   git commit -m "$msg"
   git push origin master
   
   # 更新上述中<userName>.github.io代码仓
   cd public
   git add .
   msg="rebuilding site $(date)"
   if [ -n "$*" ]; then
   	msg="$*"
   fi
   git commit -m "$msg"
   git push origin master
   ```

   

   2. `chmod +x deploy.sh`
   3. `./deploy.sh "你的commit信息"`
