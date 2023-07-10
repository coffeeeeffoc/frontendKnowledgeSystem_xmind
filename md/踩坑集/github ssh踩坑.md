[toc]

# 问题
## 问题详情
`ssh -T git@github.com`测试链接时，出现
```
ssh: connect to host github.com port 22: Connection timed out
```
（也发现部分情况出现报错`ssh: connect to host github.com port 22: Connection refused`。我姑且把这两种报错归为一类问题。）
- **22端口链接超时的原因**
- **如何解决？**
## 问题由来
想通过github的ssh连接来克隆和上传代码时，需要设置ssh key.
在github官网文档[Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)中根据步骤设置`ssh keys`。在步骤[Testing your SSH connection](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection)的`ssh -T git@github.com`测试链接时，并没有出现
```
> The authenticity of host 'github.com (IP ADDRESS)' can't be established.
> RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
> Are you sure you want to continue connecting (yes/no)?
```
内容，而是出现
```
ssh: connect to host github.com port 22: Connection timed out
```
跟官网预期步骤不一致，于是网上搜索，发现以下方法可以解决问题
- 1.需要新建或者编辑已有文件`~/.ssh/config`，
- 2.在里面添加内容
  ```
  Host github.com
  Hostname ssh.github.com
  Port 443
  User git
  ```
  以下这段话是我个人对这段配置内容的理解，这些理解是我根据[ssh config](https://www.ssh.com/academy/ssh/config)以及网上搜索相关内容得出的，不一定准确：
  > 其中`Hostname ssh.github.com`是必需的值。其他属性：`Host`必填，但因为是别名，故可根据实际情况修改；`Port`和`User`可不填。
  
  后续踩不少坑之后发现，官网[Using SSH over the HTTPS port](https://docs.github.com/en/authentication/troubleshooting-ssh/using-ssh-over-the-https-port)就提供了这种问题的解决方案。对于问题原因的解释只是一带而过：
  > Sometimes, firewalls refuse to allow SSH connections entirely. If using HTTPS cloning with credential caching is not an option, you can attempt to clone using an SSH connection made over the HTTPS port. Most firewall rules should allow this, but proxy servers may interfere.
  > `有时，防火墙会完全拒绝允许 SSH 连接。 如果无法选择使用具有凭据缓存的 HTTPS 克隆，您可以尝试使用通过 HTTPS 端口建立的 SSH 连接克隆。 大多数防火墙规则应允许此操作，但代理服务器可能会干扰`。

于是留下问题：
- **22端口链接超时的原因**
- **为什么会出现port 22 端口被禁？**
- **如何解决端口被禁？**


# 排查
由于是排查过程，涉及的一些知识点或者技术可能最终于真正原因无关，但多掌握一点技术也是棒棒哒。
## 22端口链接超时的原因
参考链接：https://askubuntu.com/a/620527，这里给出了3个可能原因：
  > 1. github.com is down (not too likely, but you could always check their status on https://status.github.com/)
  > 2. you have an invalid IP address for github.com (manual entry in /etc/hosts or your resolver) which blocks ssh from at least your IP address
  > 3. you have a firewall along the way to github.com which blocks the ssh traffic (eg. local firewall or corporate firewall)

## 为什么会出现port 22 端口被禁
参考链接：https://serverfault.com/questions/25545/why-block-port-22-outbound/25566#25566
禁用端口为了安全考虑，各有各的考虑
问题的一个回答给出了一种边缘案例：
  > I don't see that anyone has spelled out the specific risk with SSH port forwarding in > detail.
  >
  > If you are inside a firewall and have outbound SSH access to a machine on the public > internet, you can SSH to that public system and in the process create a tunnel so that people on the public internet can ssh to a system inside your network, completely bypassing the firewall.
  >
  > If fred is your desktop and barney is an important server at your company and wilma is public, running (on fred):
  >
  > ssh -R*:9000:barney:22 wilma
  >
  > and logging in will let an attacker ssh to port 9000 on wilma and talk to barney's SSH daemon.
  >
  > Your firewall never sees it as an incoming connection because the data is being passed through a connection that was originally established in the outgoing direction.
  >
  > It's annoying, but a completely legitimate network security policy.
  >  我没有看到有人详细说明了 SSH 端口转发的具体风险。
  >
  >  如果您在防火墙内并且对公共 Internet 上的计算机具有出站 SSH 访问权限，则可以 SSH 到该公共系统并在此过程中创建一个隧道，以便公共 Internet 上的人们可以完全通过 SSH 访问您网络内的系统绕过防火墙。
  >
  >  如果 fred 是您的桌面并且 barney 是您公司的重要服务器并且 wilma 是公开的，正在运行（在 fred 上）：
  >
  >  ssh -R*:9000:barney:22 威尔玛
  >
  >  并且登录将让攻击者 ssh 到 wilma 上的端口 9000 并与 barney 的 SSH 守护进程对话。
  >
  >  您的防火墙永远不会将其视为传入连接，因为数据正在通过最初在传出方向建立的连接传递。
  >
  >  这很烦人，但却是完全合法的网络安全策略。
## 22端口被谁禁用的
可能原因：
- `ISP（Internet Service Provider）`互联网服务提供商
  - 电信、移动、联通这些运营商？一般电脑能用22端口，所以运营商肯定不会大面积禁用22端口。被政府发现翻墙，然后禁用22端口？那也没必要只禁用一个端口啊。应该不至于。
- 公司防火墙
  - 由于我是在家里，所以没公司防火墙，故排除
- 电脑防火墙
  - 尝试了把电脑防火墙关闭、设置出站规则、查找没有发现当前占用22端口的进程`netstat -aon|findstr ":22"`
- 目标主机
  - 也就是`github.com`这个域名
  - 域名是由`dns`解析的，那就从`hosts`文件（windows下的目录是`C:\Windows\System32\drivers\etc`）开始吧
## 调查dns解析
### 准备知识点
#### hosts文件
hosts文件目录在`C:\Windows\System32\drivers\etc\hosts`。[不同平台的hosts文件目录](https://zh.wikipedia.org/wiki/Hosts%E6%96%87%E4%BB%B6)。修改时需要管理员权限。可以通过复制到其他目录，修改完毕再复制回本目录来实现修改。也可以通过vscode修改，保存时点击右下角弹出的【以管理员身份重试】按钮，来实现保存。
一个误解：有人说hosts文件需要ANSI格式保存而不能UTF-8格式保存，我试下来UTF-8格式保存也没问题。

- https://ipaddress.com/ 是一个用来搜索指定域名的ip的一个网站。我是通过此网站搜索到github.com的ip，然后保存在hosts文件中的。
- hosts文件中，`#`表示注释当前行的后续部分。
- `140.82.113.3 github.com`把 `从ipaddress.com搜索出来ip`加上`1个以上的空格或tab`再加上`github.com`这一行内容填入hosts文件即可。后续DNS解析github的时候就会采用此处设置的IP：`140.82.113.3`。
- 修改完`hosts`文件后，一定要执行`ipconfig /flushdns`命令来刷新dns的缓存。才能使新配置生效。
#### ping
ping （Packet Internet Groper）是一种因特网包探索器，用于测试网络连接量的程序。有人说ping采用的是`echo`服务，采用的是端口7。而事实是：
> 标准ping命令不使用TCP或UDP。它使用ICMP。ICMP没有端口！
部分主机会禁用ping。通过防火墙的安全策略可以指定禁用ICMP协议类型即可实现。
```
ping github.com
```
#### nslookup
  nslookup是用来查询DNS的记录，查看域名解析是否正常，在网络故障的时候用来诊断网络问题
  格式`nslookup domain [dns-server]`
```
# 采用当前主机配置的dns服务器来查找指定domain(也就是github.com)的dns解析结果
nslookup github.com
# 可以网上搜，有很多大公司都提供公共的DNS服务器，都可以选择使用，这里选择谷歌的8.8.8.8
# 指定某个dns服务器来查找指定domain(也就是github.com)的dns解析结果
nslookup github.com 8.8.8.8
```
我电脑由于设置了ip6的dns服务器，故下面查询结果如下：
```
PS C:\Users\15211> nslookup github.com
服务器:  dns.google
Address:  2001:4860:4860::8888

非权威应答:
名称:    github.com
Address:  20.205.243.166

PS C:\Users\15211>
```
#### telnet
这个功能需要在windows的【启用或关闭Windows功能】中，选中`Telnet客户端`选项，并启用。启用完成还有根据提示决定是否需要重启电脑。
主机的23端口用来telnet连接。可参考`C:\Windows\System32\drivers\etc\services`文件，其中记载了某个服务与某个端口的映射关系。只要当前主机的23端口和目标主机的23端口开放，则能telnet连接。少部分主机会禁用telnet连接。
telnet是一个命令行工具，可以用来测试当前主机与目标主机的某个端口的连通性。
```
telnet github.com 80
```
输入完之后，若命令行终端的内容会整体变黑且只有一个光标出现，则表示能正常连接。暂时没好的办法退出，我遇到的情况一般是直接关闭终端。有时也会出现【`过一段时间，终端自动退回刚才的终端界面`】这种情况。
若出现
```
正在连接github.com...无法打开到主机的连接。 在端口 81: 连接失败
```
则表示无法与目标主机的该端口连通。
### 继续调查
根据上面[准备知识点](#准备知识点)的内容继续调查。

上面了解了`nslookup`和`ping`和`telnet`，此时发现一个不一致的问题：
`nslookup`测试`github.com`
```
PS C:\Users\15211\.ssh> nslookup github.com
服务器:  dns.google
Address:  2001:4860:4860::8888

非权威应答:
名称:    github.com
Address:  20.205.243.166

PS C:\Users\15211\.ssh>
```
`ping`测试`github.com`
```
PS C:\Users\15211\.ssh> ping github.com

正在 Ping github.com [140.82.113.3] 具有 32 字节的数据:
来自 140.82.113.3 的回复: 字节=32 时间=263ms TTL=45
来自 140.82.113.3 的回复: 字节=32 时间=263ms TTL=45
来自 140.82.113.3 的回复: 字节=32 时间=262ms TTL=45
来自 140.82.113.3 的回复: 字节=32 时间=263ms TTL=45

140.82.113.3 的 Ping 统计信息:
    数据包: 已发送 = 4，已接收 = 4，丢失 = 0 (0% 丢失)，
往返行程的估计时间(以毫秒为单位):
    最短 = 262ms，最长 = 263ms，平均 = 262ms
PS C:\Users\15211\.ssh>
```
`nslookup`和`ping`同一个域名，得到的dns解析的IP居然是不一致的。我就一直朝着这个目标调查很久，后来发现，这是正常现象。虽然误导我调查了很久，就结果来说没啥作用。但是能更清晰的认识`nslookup`和`ping`这两个工具。
- `DNS`,（Domain Name Server，域名服务器）
- `hosts`,内容比DNS查到的结果优先级更高
  - 用途
    - 拦截一些恶意网站的请求，从而防止访问欺诈网站、感染病毒或恶意软件。
    - 用来处理因域名服务器缓存污染而对访问域名的地址解析进行修正。
    - 涉及域名与IP地址关系的技术调整。
- `nslookup`,ns表示name service。此工具只查询dns解析结果，不代表真正连接。
- 真正连接时，优先依据hosts文件的配置，其次是各级DNS返回的结果。
### bingo
我发现另一台电脑执行`ssh -T git@github.com`时能正常链接不会报timeout。于是通过对比进行排查。
最终发现，能正常连接的电脑的hosts中 github相关的配置为`140.82.113.4 github.com`，而不能正常连接的电脑的配置为`140.82.113.3 github.com`
采用telnet测试140.82.113.4的22端口连通性：
```
PS C:\Users\15211> telnet 140.82.113.4 22
SSH-2.0-babeld-d2b829ee

```
采用telnet测试140.82.113.3的22端口连通性：
```
PS C:\Users\15211> telnet 140.82.113.3 22
正在连接140.82.113.3...无法打开到主机的连接。 在端口 22: 连接失败
PS C:\Users\15211>
```
于是最终得出初步结论：目标主机`140.82.113.3`的22端口被禁，所以无法与之连接。目标主机`140.82.113.4`的22端口没被禁，故可以与之连接。

#### 连号找没被禁端口的
初步判断，是不是可以通过试IP的方式找到没被禁22端口的IP呢？
我经常修改hosts文件，发现的一个规律就是github的IP一般是连号的。比如
- `140.82.113.3` 22端口不可用
- `140.82.113.4` 22端口可用
- `140.82.114.3` 22端口可用
- `140.82.114.4` 22端口可用
- `140.82.114.5` 22端口不可用
#### 定位IP位置
https://iplocation.com/
发现两个IP所在的美国的位置一致，所以就不是地区因素导致的不同IP的22端口被禁不一致。姑且猜测是疏漏或者忽略，或者代理原因，或者不同人员采取了不同的策略等原因吧。
#### 通过`nslookup`指定其他DNS解析
尝试了一些[公共DNS](https://zhuanlan.zhihu.com/p/339495904)
```
PS C:\Users\15211\.ssh> nslookup github.com 119.29.29.29
服务器:  pdns.dnspod.cn
Address:  119.29.29.29

非权威应答:
名称:    github.com
Address:  20.205.243.166

PS C:\Users\15211\.ssh> nslookup github.com 2400:3200:baba::1
服务器:  UnKnown
Address:  2400:3200:baba::1

非权威应答:
名称:    github.com
Address:  20.205.243.166

PS C:\Users\15211\.ssh> nslookup github.com 223.6.6.6
服务器:  public2.alidns.com
Address:  223.6.6.6

非权威应答:
名称:    github.com
Address:  20.205.243.166

PS C:\Users\15211\.ssh> nslookup github.com 114.114.114.114
服务器:  public1.114dns.com
Address:  114.114.114.114

非权威应答:
名称:    github.com
Address:  20.205.243.166

```
发现每个DNS解析的结果都是`20.205.243.166`。原因我不清楚。不过发现一些现象：
- IP是可以直接访问的，比如通过[https://20.205.243.166](https://20.205.243.166)可以直接访问 https://github.com 
- `ping 20.205.243.166`没反应，估计ping被禁了
- `telnet 20.205.243.166 80`,`telnet 20.205.243.166 443`能连接
- `telnet 20.205.243.166 22`不能连接，22端口也被禁了
## 验证解决方案
先修改hosts中内容，把`140.82.113.3 github.com`改为`140.82.113.4 github.com`。
切换目录到 `~/.ssh`并移除`config`文件（为了临时测试，故修改`config`文件名为`config1`）。
再执行`ssh -T git@github.com`，
若出现
```
The authenticity of host 'github.com (140.82.113.4)' can't be established.
ECDSA key fingerprint is SHA256:p2QAMXNIC1TJYWeIOttrVc98/R1BUFWu3/LiyKgUfQM.
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
```
这几句话，则说明链接已经能成功建立，只需要输入yes即可将`域名`、`ip`、`加密算法名`、`fingerprint指纹`等内容存入`known_hosts`文件中。下次连接即可直接复用此`fingerprint指纹`即可。

详细步骤如下：
```
PS C:\Users\15211> cd .\.ssh\
PS C:\Users\15211\.ssh> ls


    目录: C:\Users\15211\.ssh


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         2022/1/10     21:52            133 agent.env
-a----         2022/1/10     13:22            179 config
-a----          2022/1/4     17:32            411 id_ed25519
-a----          2022/1/4     17:32             99 id_ed25519.pub
-a----         2022/1/10     13:21            192 known_hosts


PS C:\Users\15211\.ssh> mv .\config .\config1
PS C:\Users\15211\.ssh> cat .\known_hosts

PS C:\Users\15211\.ssh> ssh -T git@github.com
The authenticity of host 'github.com (140.82.113.4)' can't be established.
ECDSA key fingerprint is SHA256:p2QAMXNIC1TJYWeIOttrVc98/R1BUFWu3/LiyKgUfQM.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com,140.82.113.4' (ECDSA) to the list of known hosts.
Hi coffeeeeffoc! You've successfully authenticated, but GitHub does not provide shell access.
PS C:\Users\15211\.ssh> cat .\known_hosts
github.com,140.82.113.4 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBEmKSENjQEezOmxkZMy7opKgwFB9nkt5YRrYMjNuG5N87uRgg6CLrbo5wAdT/y6v0mKV0U2w0WZ2YB/++Tpockg=
PS C:\Users\15211\.ssh> ssh -T git@github.com
Hi coffeeeeffoc! You've successfully authenticated, but GitHub does not provide shell access.
PS C:\Users\15211\.ssh>
```

以上，至此，大功告成。

# 总结
- **22端口链接超时的原因**
  - 访问github的时候，IP（hosts文件指定的ip或者DNS解析的IP）对应的主机的22端口被禁用，无法连接
- **为什么会出现port 22 端口被禁？**
  - 某些地区或者公司，出于安全策略禁用
  - 姑且猜测是疏漏或者忽略，或者代理原因，或者不同人员采取了不同的策略等原因吧。
- **如何解决端口被禁？**
  - 不使用22端口，用443端口，[Using SSH over the HTTPS port](https://docs.github.com/en/authentication/troubleshooting-ssh/using-ssh-over-the-https-port)
  - [ipaddress.com](https://www.ipaddress.com/)或者其他类似网站，查找github.com对应的IP，尝试一些，或许有些IP就没被禁。不过根据IP找不太好找，根据我的经验，可以根据[连号规律](#连号找没被禁端口的)尝试。换个没禁用22端口的IP并修改到hosts文件中即可。

虽然换IP并不是一件特别好（不一定好找到合适的IP）的解决方案，但通过这次调查，也了解了一些工具和原理，对自己的知识体系大有帮助。
