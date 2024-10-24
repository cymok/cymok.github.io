---
layout: post
tags: Docker Gitlab
---

# 使用 docker 部署 gitlab

## 部署 gitlab 服务

拉取镜像，可选 (这里不拉取，创建容器时也会自动拉取)

```
sudo docker pull gitlab/gitlab-ce:latest
```

创建容器并运行

```
sudo docker run --detach \
  --hostname <your_hostname> \
  --publish 443:443 --publish 80:80 --publish 22:22 \
  --name gitlab \
  --restart always \
  --volume </path/to/config>:/etc/gitlab \
  --volume </path/to/logs>:/var/log/gitlab \
  --volume </path/to/data>:/var/opt/gitlab \
  gitlab/gitlab-ce:latest
```

`--hostname` 指定主机名，域名 或 IP，(局域网 可填写 IP 或 DNS 可解析的 域名，自己本机使用可填写 localhost)

挂载目录为: 配置、 日志、 数据

## 配置

### 初始账户

登录 http://<your_hostname>/

初始账户 `root`

初始密码在配置目录的文件 `/etc/gitlab/initial_root_password`, 也可以按后面的步骤重置一个新密码

```
sudo docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
```

### 配置管理员邮箱

配置 SMTP 相关，打开 gitlab.rb

```
sudo vim </path/to/config>/gitlab.rb
```

内容

```
gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "smtp.server"
gitlab_rails['smtp_port'] = 465
# 邮箱
gitlab_rails['smtp_user_name'] = "smtp user"
# 授权码
gitlab_rails['smtp_password'] = "smtp password"
# smtp_domain 是你自己的域名或者与你的邮箱帐户关联的域名，hostname
gitlab_rails['smtp_domain'] = "example.com"
gitlab_rails['smtp_authentication'] = "login"
# smtp_enable_starttls_auto 和 smtp_tls 只能二选一，建议直接开启 smtp_tls 为 true
gitlab_rails['smtp_enable_starttls_auto'] = false
gitlab_rails['smtp_tls'] = true
gitlab_rails['smtp_pool'] = false

# 必须跟 smtp_user_name 保持一致
gitlab_rails['gitlab_email_from'] = 'example@example.com'
```

需要注意这里的 tls 协议和对应端口号。

邮箱 | TSL 端口 | 非 TSL 端口 | SMTP 地址
--- | --- | --- | ---
163 | 465 或 994 | 25 或 80 | smtp.163.com
QQ | 465 | 25 或 587 | smtp.qq.com
Gmail | 465 | 25 或 587 | smtp.gmail.com
Mail.ru | 465 | 25 或 587 | smtp.mail.ru
Outlook | 465 | 25 或 587 | smtp.live.com
Yahoo | 465 | 25 或 587 | smtp.mail.yahoo.com
iCloud | 465 | 25 或 587 | smtp.mail.me.com

进入容器

```
sudo docker exec -it gitlab bash
```

更新配置

```
gitlab-ctl reconfigure
```

进入 Rails 控制台

```
gitlab-rails console
```

测试发送邮件

```
Notify.test_email('recipient@example.com', 'This is a test email', 'Test').deliver_now
```

到此，发送成功即可

注意 --hostname 没有填写或者填写不对时，需要手动把邮件确认链接的 hostname 改为部署 gitlab 的设备的 IP

### 其它命令

#### 重启服务，在容器执行

```
gitlab-ctl restart
```

#### Gitlab Rails 控制台

进入 Rails 控制台

```
gitlab-rails console
```

控制台相关命令

查看smpt配置

```
// 检查邮件的协议，进入 `gitlab-rails console` 执行
ActionMailer::Base.delivery_method

// 查看smpt配置，进入 `gitlab-rails console` 执行
ActionMailer::Base.smtp_settings
```

修改用户密码

```
// 获取 id=1 的用户
user = User.where(id: 1).first

// 设置新密码
user.password = '新密码'

// 保存
user.save
```

在这里控制台不能修改邮箱。可以在配置好上面的管理员邮箱后在页面上添加一个新邮箱后再删除 root 用户的 admin@example.com

