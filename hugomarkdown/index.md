# Hugo中Markdown转网页的一些注意事项


# Hugo中Markdown转网页的一些注意事项

## 图片引用路径

- 方式1：
  - 将Markdown中引用的图片放到Hugo项目的static目录下，注意不是themes/LoveIt/static中的static目录
  - 编写Markdown中的图片引用时使用相对路径，表示上级目录的两个点（`../`）在这里是指static目录。
    - 比如`../images/posts/image.png`是指`static/images/posts/image.png`
- 方式2：
  - 将Markdown文件名命名为index.md或_index.md，就可以使用相对路径引用和Markdown文件同一目录下的图片

## 目录配置（TOC）

在config.toml中配置目录显示设置

- `[params.page.toc]`下的参数配置`keepStatic = false`时，目录在侧边
- `[params.page.toc]`下的参数配置`keepStatic = true`时，目录在顶部

## 主页文章数量

- 主页文章数量设置后超出的文章在分页的头像显示不出来？
