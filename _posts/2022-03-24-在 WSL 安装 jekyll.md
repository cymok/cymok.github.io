---
layout: post
tags: jekyll
---

1.先更新apt
```
apt update && apt upgrade -y
```

2.安装ruby
```
apt install ruby-full
```

3.配置环境
```
echo ‘# Ruby' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

4.安装Jekyll和Bundler
```
gem install jekyll bundler
```

新建项目
```
jekyll new NewProject
```

打开已有项目
```
# 进入到项目
cd NewProject
# 启动服务, 默认 host 是 localhost 可以用非127001的本机ip, 默认 port 是 4000
jekyll serve --port 9527
```

---

目前我遇到的问题：

- 依赖版本冲突报错，`bundle update` 更新失败 timeout

解决办法

1.`rm Gemfile.lock`

2.`bundle clean --force`

3.`bundle install`

重新自动生成即可
