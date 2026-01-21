## 首先去给域名添加解析

因为我们是部署在服务器上，以IP的形式去访问的，所以
- 添加的类型是A

![](/blogs/re-public/e70ae5b7e28c5498.png)

- 主机记录就是你想要访问的二级域名的头部

比如你买了`bbb.com`，这个是主域名（也叫一级域名），然后你想要以`aaa.bbb.com`的形式去访问你的项目，那么你的主机记录就是`aaa`。如果想要直接以`bbb.com`去访问，主机记录填*@*即可，详细解释如下

![](/blogs/re-public/372e552090a00852.png)

- 记录值就是你的服务器的**公网IP**地址

## oss配置

腾讯云的CORS记得把自己的域名添加上去`http://你的域名`，这里http即可

![](/blogs/re-public/df8d9070f3122217.png)

## 添加域名

宝塔面板，前端中

![](/blogs/re-public/88b9adc09b4b5f4a.png)

- 这里添加域名即可，然后配置文件略微改一下，http不改成https是因为后端就是http服务，

![](/blogs/re-public/e1aaec13d0259bbc.png)

**location /** 的需要放在下面

## 然后配置SSL

1. 点击 Let's Encrypt 去申请免费90天的证书，然后下载


![](/blogs/re-public/c0f3468e106f8d43.png)


2. 之后来到当前证书，填写下载的压缩包里key文件和pem文件里的内容，点击保存


![](/blogs/re-public/2ff9334b0d9b7fb3.png)

- 后续想要安全**可以打开强制HTTPS**

![](/blogs/re-public/0694909fd6fc8f5c.png)

3. 验证

![](/blogs/re-public/be22ee806e149afe.png)
 
首先，如上图，首先认证域名得是我们填的，如果是guoziyang.com，那么你的SSL证书必须得是泛证书，关于申请方法可参考leikoo大佬的：

[https://www.codefather.cn/post/1831983737277050881#heading-1](https://www.codefather.cn/post/1831983737277050881#heading-1)  

然后看剩余日期是否出现，说明SSL部署成功且生效了

## 前端更改

生产环境那里需要改成自己的域名，然后由于前面开了**强制HTTPS**,所以这里也要改成https

![](/blogs/re-public/4e34161677facf75.png)

同样的ws这里也需要改成域名，不需要加端口号

![](/blogs/re-public/4ccaaa3aa62fee55.png)

之后正常打包

```
npm run pure-build
```

之后把`dist`目录里的都上传到宝塔面板，把原来的文件覆盖掉

### 优化

- 当然这里可以改成根据环境变量来访问不同的接口

新建一个`.env`文件。然后填写

```.env
VITE_WS_BASE_URL=ws://localhost:8123
VITE_API_BASE_URL=http://localhost:8123
```

然后新建一个测试的，一个生产的，比如`.env.dev`、`.env.prod`,当然我们项目都上线了，暂且不像企业那样区分测试环境和生产环境。

我以`.env.prod`为例

```.env
VITE_WS_BASE_URL=ws://你的真实域名
VITE_API_BASE_URL=https://你的真实域名
```
*VITE_API_BASE_URL* 是 https 是因为我强制打开了 https

然后代码里改成通过根据不同环境变量去读取
```ts
const API_BASE_URL = import.meta.env.VITE_API_BASE_URL

// 创建 Axios 实例
const myAxios = axios.create({
  baseURL: API_BASE_URL,
  timeout: 60000,
  withCredentials: true, // 是否携带凭证
})
```

> 这里提一点，可能有人说咋不是`process.env.xxx`,因为我是 Vite 项目所以是`import.meta.xxx`，对于vue-cli创建的项目是`process.env.xxx`。更简单一点，你看下你项目里是`vue.config.js`还是vite.config.js,`vue.config.js`就是`process.env.xxx`，`vite.config.js`就是`import.meta.xxx`

之后记得把`.env.prod`加入到`.gitignore`中，**别提交到仓库上**

然后到package.json改下打包命令

![](/blogs/re-public/f5876e2911ceb32c.png)

就改下之前鱼皮写的`pure-build`的后面添加`--mode prod`,意思就是打包的时候加载生产环境，即`.env.prod`

> **注意哦，**如果你是`.env.producement`,那么这里就是`--mode producement`，打包和文件名得对上，不然找不到

之后我们到控制台输入`npm run pure-build`来打生产包即可，如果你想要测试下，或者像企业那样打测试环境的包，那我门再添加一条
```json
"pure-build:dev": "vite build --mode dev",
```
之后我们在控制台输入`pure-build:dev`就意味着打的是测试环境的包了

估计有人有个疑问了，那本地呢？本地打啥包哦，我们日常开发前后端联调就是本地，没必要，真想要本地打包那就本地添加`.env.local`,同理 `package.json` 添加上`"pure-build:local":"vite build --mode local"`这个打包命令就可以用了。但是企业不一样，企业你拉项目可能会看到`.env`、`.env.local`、.`env.dev`、`.env.prod`，甚至还有个uat环境的`.env.uat`,uat环境一般是指客户公司真正使用的线上环境，甲方公司由于有定制的需求，跟我们做的产品有些方向略不一样所以就有了这个区分，这时候正式环境就是指公司内部研发产品运行的环境

## 后端注意

后端并没有需要改的地方，只是要注意下数据库以及分库分表的位置，*localhost*是否改成了服务器的ip地址

![](/blogs/re-public/975aab21a73bc56b.png)

宝塔面板的配置按照之前的即可

可以再浏览器输入`https://你的域名/api/doc.html`，访问成功说明没问题



> 最后就可以通过你的域名去访问前端了，当你去查看浏览器的时候，完整地址也是https

然后点击那个**锁**，会显示连接安全，毕竟配置了SSL证书

![](/blogs/re-public/ac708fd41846c7ff.png)

> 感觉 **SSL** 这玩意一定要在服务器上把 **SSL** 证书上传了才行，否则如果你的前端是部署在`vercel`上，然后添加了自定义域名(哪怕备案了)，虽然能正常访问，但还是显示不安全。