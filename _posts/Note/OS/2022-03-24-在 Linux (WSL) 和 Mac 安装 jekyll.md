---
layout: post
tags: jekyll
---

1.先更新apt
```
apt update && apt upgrade -y
```

2.安装ruby
Linux
```
apt install ruby-full
```
Mac
```
brew install ruby
```
建议通过 rvm 来安装 ruby 并管理，可以随时切换多个场合的不同版本

3.配置环境
Linux
```
echo ‘# Ruby' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```
Mac
```
echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.bash_profile
source ~/.bash_profile
```
注意是要写入自己的变量文件

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

- jekyll s 出错

原因可能ruby版本过高，可尝试切换到 ruby 2.x  
如果安装了rvm，在rvm上安装2.x，然后再切换，如果不加默认参数 `--default` rvm切换后关闭窗口后会失效，下次使用需要再次切换
```
rvm list # 查看ruby列表
rvm list known # 查看可安装的版本
rvm install 2.6.0 # 没有安装则安装2.x
rvm use 2.6.0 --default # 使用并设为默认
# 开启服务
jekyll s
```

- 提示 bundler 未安装  
尝试
```
gem update --system
```

- 依赖版本冲突报错，`bundle update` 更新失败 timeout

解决办法

1.`rm Gemfile.lock`

2.`bundle clean --force`

3.`bundle install`

重新自动生成即可

- 后来不知道是更新了gem还是什么，导致jekyll打不开了

解决办法
```
gem install -n /usr/local/bin jekyll
```
重新安装到本地即可

原因是不能默认安装到系统目录？`-n`指定到用户目录？那是否用 `gem install jekyll --user-install` 也行？
