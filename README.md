# === serv00 |  vmess一键安装脚本 ===
</br>不要只是下载或Fork。请 **follow** 我的GitHub、给我所有项目一个 **Star** 星星（拜托了）！你的支持是我不断前进的动力！
</br>**更多内容请访问[【个人博客】](https://2010hcy.us.kg)**

# 免费serv00服务器一键脚本部署VMess

这个项目的脚本安装管理并运行一个VMess节点,并可以通过Cloudflare的CDN设置域名回源进行加速,解锁ChatGPT、TikTok、其它流媒体、小网站等，但效果没大厂vps好

# 部署教程：

## 一、需要准备的前提资料
### 1、首先注册一个Serv00账号，建议使用gmail邮箱注册，注册好会有一封邮箱上面写着你注册时的用户名和密码
- 注册帐号地址：https://serv00.com

### 2、下载MobaXterm远程连接工具或使用Windows自带的ssh

## 二、安装前需准备的初始设置
- 1、登入邮件里面发你的 DevilWEB webpanel 后面的网址，进入网站后点击 Change languag 把面板改成英文
- 2、然后在左边栏点击 Additonal services ,接着点击 Run your own applications 看到一个 Enable 点击
- 3、找到 Port reservation 点击后面的 Add Port 新开一个端口，随便写，也可以点击 Port后面的 Random随机选择Port tybe 选择 TCP
- 4、然后点击 Port list 你会看到一个端口

- 5、 启用管理权限

***完成此步骤后，退出 SSH 并再次登录。***

## 三、开始安装部署

- 1、用我们前面下载的工具登入SSH(有些工具 第一次连接还是会弹出输出密码记得点X 然后再添加密码 )
使用 serv00 通过电子邮件发送给您的信息（下面username、panel要修改成你邮箱收到对应的信息）。
```
ssh <username>@<panel>.serv00.com
```

- 2、进入到面板后复制下面代码一键安装
```
bash <(curl -Ls https://raw.githubusercontent.com/2010HCY/serv00-vmess/main/serv00-vmess.sh)
```

- 3、保活命令（有时虚拟主机重启后，会删除所有进程和定时任务，所以要手动重新执行下面保活命令，让定时任务生效，不要问为什么，问就是是免费的后遗证）
```
(crontab -l; echo "*/12 * * * * pgrep -x "web" > /dev/null || nohup /home/${USER}/.vmess/web run -c /home/${USER}/.vmess/config.json >/dev/null 2>&1 &") | crontab -
```
### 隧道保活命令情况如下
- 默认隧道保活命令， <你的面板开通端口> 要修改你的端口
```
(crontab -l; echo "*/12 * * * * pgrep -x "bot" > /dev/null || nohup /home/${USER}/.vmess/bot tunnel --edge-ip-version auto --no-autoupdate --protocol http2 --logfile /home/${USER}/.vmess/boot.log --loglevel info --url http://localhost:<你的面板开通端口> >/dev/null 2>&1 &") | crontab -
```
- token固定隧道保活命令， <ARGO_AUTH> 要修改你的token
```
(crontab -l; echo "*/12 * * * * pgrep -x "bot" > /dev/null || nohup /home/${USER}/.vmess/bot tunnel --edge-ip-version auto --no-autoupdate --protocol http2 run --token <ARGO_AUTH> >/dev/null 2>&1 &") | crontab -
```
- json固定隧道保活命令
```
(crontab -l; echo "*/12 * * * * pgrep -x "bot" > /dev/null || nohup /home/${USER}/.vmess/bot tunnel --edge-ip-version auto --config tunnel.yml run >/dev/null 2>&1 &") | crontab -
```


- 查看保活crontab任务
```
crontab -l
```
- 上面命令完会显示下面信息就是有保活设置成功
```
*/12 * * * * pgrep -x "web" > /dev/null || nohup /home/${USER}/.vmess/web run -c /home/${USER}/.vmess/config.json >/dev/null 2>&1 &
```

## 四、测试节点
- 1、把安装成功返回的节点信息复制到订阅工具里就可以使用

- 2、如果不记得节点配置，可以通过下面信息查看
```
cat /home/${USER}/.vmess/list.txt
```
- 3、节点通过Cloudflare的CDN设置域名回源进行加速
- CF端口类型

HTTP：80，8080，8880，2052，2082，2086，2095

HTTPS：443，2053，2083，2087，2096，8443

- 4、Cloudflare的CDN设置域名回源进行加速
在隧道那配置一个隧道并获得令牌（开隧道的前提是有在Cloudflare托管的域名）
  
## 五、卸载VMess
### 一键卸载命令，根据提示，选择2（2. 卸载sing-box） 直接卸载完成
```
bash <(curl -Ls https://raw.githubusercontent.com/2010HCY/serv00-vmess/main/serv00-vmess.sh)
```
