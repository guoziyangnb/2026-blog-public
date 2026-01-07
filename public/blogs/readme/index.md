> blog 前端网站（gzyblog.guoziyang.com）已链接到 public 仓库

该项目使用 Github App 管理项目内容，请保管好后续创建的 **Private key**，不要上传到公开网上。

## 1. 克隆项目

开源项目地址：https://github.com/YYsuni/2025-blog-public

![](blogs/readme/f8fb1af7c34a8cf8.webp)

我们在本地克隆一下

```git
git clone https://github.com/YYsuni/2025-blog-public
```

如果觉得速度太慢可以用以下这个国内香港代理地址

```git
git clone https://hk.gh-proxy.org/https://github.com/YYsuni/2025-blog-public
```

## 2. 然后去github新建一个仓库，

选择**public**,之后按照提示把本地的仓库推送到远程

```git
git add .
git commit -m "Init commit content"
git push origin main
```

## 3. 创建 Github App 链接仓库

在 github 个人设置里面，找到最下面的 Developer Settings ，点击进入

![](/blogs/readme/0abb3b592cbedad6.png)

进入开发者页面，点击 **New Github App**

_GitHub App name_ 和 _Homepage URL_ , 输入什么都不影响。Webhook 也关闭，不需要。

![](/blogs/readme/71dcd9cf8ec967c0.png)

只需要注意设置一个仓库 write 权限，其它不用。

![](/blogs/readme/2be290016e56cd34.png)

点击创建，谁能安装这个仓库这个选择无所谓。直接创建。

![](/blogs/readme/aa002e6805ab2d65.png)

### 创建密钥

创建好 Github App 后会提示必须创建一个 **Private Key**，直接创建，会自动下载（不见了也不要紧，后面自己再创建再下载就行）。页面上有个 **App ID** 需要复制一下

再切换到安装页面

![](/blogs/readme/c122b1585bb7a46a.png)

这里一定要只**授权当前项目**。

![](/blogs/readme/2cf1cee3b04326f1.png)

点击安装，就完成了 Github App 管理该仓库的权限设置了。下一步就是让前端知道推送那个项目，就是最开始提到的**环境变量**。（如果你不会设置环境变量，直接改仓库文件 `src/consts.ts` 也行。因为是公开的，所以环境变量意义也不大）

## 4. 打开腾讯云的EdgeOne

链接：https://console.cloud.tencent.com/edgeone/zones

然后点击**Pages**

![](blogs/readme/c0abbc02a187437a.jpg)


导入仓库 选github，跳登录，github登录 然后选则我们刚才克隆的仓库

![](blogs/readme/6474d287a68e76ca.jpg)

## 5. 设置环境变量

![](blogs/readme/db70df0cfdc5d4ee.png)

分别是_NEXT_PUBLIC_GITHUB_OWNER_和_NEXT_PUBLIC_GITHUB_APP_ID_  

**OWNER** 是你GitHub的用户名，**APP_ID**是你之前创建的APP_ID，之后点击保存

## 6.自定义域名

之后来到域名管理，点击添加自定义域名

![](/blogs/readme/827b5b37b16e8eef.png)
**这个域名必须得先备案！!**，必须得是CNAME类型的才行，然后把记录值添加到对应的域名服务商的解析中，等待即可

![](blogs/readme/a50b43c6af9142ac.jpg)

之后就可以根据自定义的域名访问了

