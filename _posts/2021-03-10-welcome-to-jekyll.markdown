---
layout: post
title:  "Welcome to lwtcas' blog!"
date:   2021-03-10 10:30:43 +0800
categories: jekyll update
---
# 解决jekyll无法本地预览中文文件的问题

jekyll 的标题一般通过markdown头部的title来获取，如果不指定title，会自动解析文件名作为标题。

为了让markdown更加简洁方便（好看），让其从文件名中获取显然是一种不错的方式，但是，这样会带来一个问题，就是无法使用中文文件名，否则本地调试会无法访问。

修改安装目录\Ruby22-x64\lib\ruby\2.2.0\webrick\httpservlet下的filehandler.rb文件，建议先备份。

找到下列两处，添加一句（+的一行为添加部分）

```Ruby
path = req.path_info.dup.force_encoding(Encoding.find("filesystem"))
+ path.force_encoding("UTF-8") # 加入编码
if trailing_pathsep?(req.path_info)

break if base == "/"
+ base.force_encoding("UTF-8") #加入编码
break unless File.directory?(File.expand_path(res.filename + base))
```

