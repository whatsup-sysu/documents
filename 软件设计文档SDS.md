<!-- vscode-markdown-toc -->
* 1. [技术选型](#)
* 2. [软件架构设计](#-1)
	* 2.1. [1. 软件架构的逻辑视图](#-1)
	* 2.2. [2. 软件架构的物理视图](#-1)
	* 2.3. [部署细节](#-1)
		* 2.3.1. [前端部署说明](#-1)
		* 2.3.2. [前端文件目录](#-1)
		* 2.3.3. [后端源码目录以及说明](#-1)
* 3. [模块划分](#-1)
	* 3.1. [client端](#client)
	* 3.2. [server端](#server)
* 4. [详细解释具体设计在源码中的体现](#-1)
	* 4.1. [client端](#client-1)
	* 4.2. [server端](#server-1)
* 5. [UI设计文档](#UI)
	* 5.1. [UI Design](#UIDesign)
		* 5.1.1. [Use case 1](#Usecase1)
		* 5.1.2. [Version 0.1 (Week 3 ~ Week 6)](#Version0.1Week3Week6)
		* 5.1.3. [组件设计思路](#-1)
		* 5.1.4. [自评](#-1)
	* 5.2. [Version 0.2 (Week 7 ~ Now)](#Version0.2Week7Now)
		* 5.2.1. [总览](#-1)
* 6. [数据库模式设计](#-1)
* 7. [API设计](#API)
	* 7.1. [用户注册](#-1)
		* 7.1.1. [参数说明](#-1)
		* 7.1.2. [返回值](#-1)
		* 7.1.3. [示例](#-1)
	* 7.2. [用户登录](#-1)
		* 7.2.1. [参数说明](#-1)
		* 7.2.2. [返回值](#-1)
		* 7.2.3. [1. 前端api文档](#api)
		* 7.2.4. [2. 后端api文档](#api-1)
* 8. [用例设计](#-1)
		* 8.1. [用例文本与活动图](#-1)
	* 8.1. [1.发布和接收任务](#-1)
	* 8.2. [2.登陆注册](#-1)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->
# 软件设计文档（SD）

整个设计文档分成两部分，为了方便检查把课程要求检查的技术选型理由、架构设计、模块划分、详细解释具体设计在源码中的体现 提前放在SD的最前几个小结。


##  1. <a name=''></a>技术选型
（这个我来写，你们写一下剩下的模块划分、详细解释吧，涉及到源码的我写不了  https://teamwego.github.io/SE/%E8%BD%AF%E4%BB%B6%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3.html）

### client端
可选的技术选型及其选择理由分析
- 前端框架
  - Vue
### server端
可选的技术选型及其选择理由分析

- 后端框架
  - nodejs
    选用nodejs达到前后端一致，减少可能会出现的语言支持不一致导致的坑。并且因为这次的项目并不是一个计算密集的项目，所以没必要考虑分布式服务，nodejs在性能上足够达成需求。
  - go
    高性能，对于中间件支持较好，但是因为这是一个团队项目，贸然使用一个大家熟练程度不同的技术栈最严重的后果就是因为熟练程度不同导致部分同学编程bug生成机器，写的bug比写的task还更多，平白无故耗费其他同学的精力，对于项目的效率来说是一个负提升。
  - django
    后端数据库二合一，小白学习后端第一个应该学的后端框架，虽然说简单但是还是因为和go一样的原因导致不被采用。

所以我们后端框架选择的是nodejs作为大家开发的统一框架。

- 后台服务支持
  没什么好说的，Nginx，傻瓜式操作。

- 数据库DBMS
  - sql
    最常用的数据库DBMS类型，优化好效率高，只能对大数据量低并发的不二之选，因为上过数据库的课所以大家都会用。
  - nosql
    现在web架构必不可少的分布式数据库DBMS，一句话问就是分布式，针对对于上锁频率不高的服务来说非常完美，相对于sql的时候可操作性更大。
    
  但是还是因为效率负提升的考虑，最后还是选用了mysql作为数据库的技术栈。
- 额外的技术支持
  - docker
    容器化服务，即插即用的特性

##  2. <a name='-1'></a>软件架构设计

###  2.1. <a name='-1'></a>1. 软件架构的逻辑视图

![](./assets/logic_arch.PNG)

架构是前后端分离，前端发放服务器和后端交互服务器分离，其中用restful-like api进行交互。用mysql作为持久化层。

前端的模式为mvc模式，其中使用到的外部支持库有编辑调查表的survey.js

###  2.2. <a name='-1'></a>2. 软件架构的物理视图

![](./assets/physics_arch.PNG)

架构的物理层架构，分别分成了前端用户的浏览器访问端，从静态服务器中获取前端文件，用restful的方式从api服务器去进行交互，其中涉及到持久层的交互由与mysql dbrs完成。



###  2.3. <a name='-1'></a>部署细节

####  2.3.1. <a name='-1'></a>前端部署说明

##### 使用cnpm安装的部分均可用npm安装替代。使用cnpm是因为cnpm使用国内镜像资源，下载安装依赖包较块。

1. cnpm install -g vue-cli（前端依靠vue来实现，所以在运行项目之前，首先要安装vue）
2. cd Frontend/whatsup（进入项目文件夹，在这里运行项目启动命令）
3. cnpm install（一键安装项目运行所需要的所有依赖包）
4. npm run dev（项目启动，浏览器自动跳出页面）

##### npm以及cnpm安装方式

cnpm解决了国内某些依赖包使用npm无法安装或者安装过慢的问题。在安装cnpm之前，首先要确保安装了npm。

- npm安装

1. 根据自己的操作系统，首先前往nodejs官网[下载nodejs](http://nodejs.cn/download/) 

2. 然后点击安装，选择自己要安装的路径，此处我选择的是：**D:\Program Files\nodejs**，一路next，安装至完成。 

3. **window+R**,输入**cmd**,打开命令提示符窗口，输入：`npm -v` 检测是否安装成功。

4. 配置npm的全局模块的存放路径、cache的路径，此处我选择放在：**D:\Program Files\nodejs** 

   输入如下命令：

   ```
   npm config set prefix "D:\Program Files\nodejs\node_global"
   npm config set cache"D:\Program Files\nodejs\node_cache"
   ```

5. 在系统环境变量添加NODE_PATH,输入路径为： D:\Program Files\nodejs\node_global 
   操作如下：我的电脑右击，打开属性->高级系统设置->环境变量->新建（系统变量下）->输入变量名NODE_PATH->变量值：输入上面路径，确定即可。 

- cnpm安装

1. 安装cnpm，输入以下命令：

   ```
   npm install -g cnpm --registry=https://registry.npm.taobao.org
   ```

2. 添加系统变量path的内容 。因为cnpm会被安装到D:\Program Files\nodejs\node_global下，而系统变量path并未包含该路径。在系统变量path下添加该路径即可正常使用cnpm。 

3. 输入`cnpm-v`检测是否安装成功。

####  2.3.2. <a name='-1'></a>前端文件目录

- front-end
  - build:  webpack文件，配置参数,打包的代码存放在这里，自动生成
  - config:  vue项目的基本配置文件，自动生成
  - node_modules: 存放项目中使用的各种npm包，cnpm/npm install后生成
  - src: 项目源文件存放地址，包含前端页面，路径配置，静态资源等
    - assets：存放前端页面用到的各种静态资源
    - components：存放前端的各个vue页面
    - router：存放前端页面的路径配置文件
    - App.vue：VUE demo演示页面的启动组件
    - main.js：该模块页面的初始化js
  - static：项目静态资源目录
  - test：测试文件，初始化测试都写在这里,可以删除
  - .babelrc ：babel编译参数，vue开发需要babel编译
  - .editorconfig：代码编辑环境配置文件
  - .gitignore：设置git忽略文件
  - .postcssrc.js：css配置文件,自动补齐浏览器css前缀
  - index.html：主页，项目入口文件
  - package.json：项目配置文件，描述项目信息和依赖
  - README.md ：项目的说明文档，markdown 格式
  - run.bat：Windows批处理文件，运行项目时生成

####  2.3.3. <a name='-1'></a>后端源码目录以及说明
##### 项目代码  
https://github.com/whatsup-sysu/Backend

##### 代码说明
- src
  - bin: 执行程序
  - controllor:  控制器目录
    - DBController_public.js:数据库接口
    - userSystem_public.js:用户系统，登录注册等
    - dutySystem_public.js:任务系统，发布接受等
    - tradeSystem_public.js:交易系统，转账充值等
    - surveySystem_public.js:问卷调查相关
    - imageSystem_public.js:图片资源上传下载
    - utils_public:公共模块
    - validator.js:正则校验
  - database: 数据库管理
    - Config：数据表及配置信息
    - DataBaseMySQL：数据库连接管理
  - routes：路由分级处理
    - users.js 
    - dities.js
    - photo.js
    - survey.js
    - trades.js
    - survey.js
  - sessions：保存后端与前端的会话状态
  - upload：保存用户上传的资源信息
  - app.js ：express程序执行入口
  - package.json：项目配置文件，描述项目信息和依赖


##  3. <a name='-1'></a>模块划分
（我真的不知道怎么写了，我猜需要结合文件目录去解释每个模块分别在哪）

###  3.1. <a name='client'></a>client端

###  3.2. <a name='server'></a>server端

##  4. <a name='-1'></a>详细解释具体设计在源码中的体现
（structure programing -- 结构化编程（什么模式都没用））
（OOP --如果用了类）
（Aspect-oriented programing 对于功能的层次划分非常明显 mvc，mvvm可以算）
（service oriented arch 服务为主，应该和装饰器模式一样，api为主，通过装饰器可以同一个api多用）
（design pattern 设计模式，各种模式 工厂模式之类）
（从上面觉得可能涉及到的设计里面选几种出来，贴上源代码进行分析，糊弄的成分更大，写个两三个，因为这部分涉及到可扩展可维护分数，一看就知道只看你用的设计是不是很高大上，不会详细去看源代码的）
###  4.1. <a name='client-1'></a>client端

###  4.2. <a name='server-1'></a>server端


##  5. <a name='UI'></a>UI设计文档

###  5.1. <a name='UIDesign'></a>UI Design

####  5.1.1. <a name='Usecase1'></a>Use case 1

![](https://raw.githubusercontent.com/rookies-sysu/Dashboard/master/imgs/usecase/UC1_ui.png)

---

####  5.1.2. <a name='Version0.1Week3Week6'></a>Version 0.1 (Week 3 ~ Week 6)

##### 总览

第一版设计思路主要如下
- 逻辑上按照现有常见的点餐小程序设计思路，对菜品进行分类，然后用列表的形式对单个菜品展现。已有的点餐程序逻辑较为清晰，这里直接沿用
  1. 找到对应分类
  2. 选择对应分类下的菜品
  3. 切换分类继续选择
  4. 进入购物车结算
- 视觉效果上，由于传统点餐程序（例如麦当劳和美团外卖等）类别是放在左边的，所以界面整体显得很拥挤。为了解决拥挤的问题，v0.1中我牺牲掉了操作的简便性（将菜品细节放到二级菜单），而换来了足够流畅的空间表达，整体界面注入了足够的空气。同时，在卡片（Material & iOS10+）、磁贴（UWP）和扁平（iOS7~9）三种设计思路中选择卡片的陈列方式，因为点餐界面需要展现足够多的信息，磁贴的表达能力不够强，纯色块反而会抢走图片的风头；同时纯扁平的设计元素已经不再适合当前多维度信息的表达，所以最后采用了卡片作为主要表达手段

整个小程序逻辑上分成两个界面：

![](https://raw.githubusercontent.com/rookies-sysu/Dashboard/master/imgs/UI/v0.1/version1.png)

1. 分类浏览界面。
  分类浏览界面由卡片的形式展现，每个卡片对应一个类别。这样用户可以比较清楚的知道每个类别是干什么的，视觉上也不会显得太拥挤
2. 菜品列表浏览
  菜品列表浏览用列表卡片的形式呈现，购物车在右下角有一个小按钮可以随时查看购物车情况。
3. 购物车查看
  没有太多新颖的地方

####  5.1.3. <a name='-1'></a>组件设计思路

1. 类别卡片。类别卡片采用 2:1 的长宽比，每张类别卡片不会占用太多空间，同时能提供足够明显的视觉效果
2. 菜品细节。菜品细节直接用一整行来表示，每个菜品都可以占满整行，同时每张卡片之间有一定间隔，界面不会显得太拥挤
3. 信息陈列上，主要信息会突出于底层。这里用了颜色或者边框阴影进行突出。每个部分有相应的大标题来指引

####  5.1.4. <a name='-1'></a>自评

1. 逻辑较为清晰，界面不太拥挤
2. 操作不够简洁，点菜必须进入二级菜单
3. 设计语言不够统一，购物车也可以改成卡片式

###  5.2. <a name='Version0.2Week7Now'></a>Version 0.2 (Week 7 ~ Now)

####  5.2.1. <a name='-1'></a>总览

根据上一版本的思考之后，为了解决操作不流畅（二级菜单）的问题，我引入了底部导航栏。这是一个非常慎重的决定，因为引入底部导航意味着整个小程序承载的信息要足够丰富，否则底部导航会显得十分多余而且占用空间

由于在第二次迭代的讨论中我们希望加入同时点单、推荐系统等功能，所以这个时候我决定对 UI 界面进行大修改

![](![](https://raw.githubusercontent.com/rookies-sysu/Dashboard/master/imgs/UI/v0.2/version2.png)

1. 加入推荐界面。因为需要推荐的菜品有限，同时要足够吸引眼球，所以我用了非常大的正方形卡片来容纳。第一眼看过去只能完整的看到一张卡片，即大部分用户只会被推荐到一个产品，这样推荐效率会非常高
2. 菜单的分类和细节集中到一个界面。为了便捷性考虑，我最好还是决定把分类和细节集中到一个界面，考虑到界面展示空间的问题，我加入了横向滑动的方式来拓展承载空间。这样用大标题和分割线区分主要类别，而类别菜品细节也有整整一行的空间来承载。同时如果菜品太多，可以点击查看全部浏览类别内所有菜品
3. 为了比较统一的设计语言，购物车直接用连贯的白色背景、黑色小标题和分割线进行板块的区分


##  6. <a name='-1'></a>数据库模式设计
这里包含了系统基础框架所需的完整数据库设计。 


![](./assets/database_er.png)

我们并没有对于权限进行设计，而是直接把用户和发布人的功能进行不同的划分，能够调用的api不同，而不是相同的api所能执行的功能因为权限不同而产生不同的效果。

##  7. <a name='API'></a>API设计

采用了前后端分离的api设计，因为api太多所以只列了作为样例的几个。

###  7.1. <a name='-1'></a>用户注册

`POST /user`

由于需要上传文件，要求使用`multipart/form-data`提交请求。

####  7.1.1. <a name='-1'></a>参数说明

| 参数名   | 数据类型               | 描述     | 必需 |
| -------- | ---------------------- | -------- | ---- |
| email    | string                 | 注册邮箱 | 是   |
| password | string                 | 密码     | 是   |
| name     | string                 | 用户名   | 是   |
| license(optional)  | file（docx、doc、pdf） | 组织证明 | 是   |

####  7.1.2. <a name='-1'></a>返回值

| HTTP状态码 | 返回格式            | 描述       |
| ---------- | ------------------- | ---------- |
| 200        | NULL                | 成功       |
| 400        | {message: 'reason'} | 错误的请求 |

####  7.1.3. <a name='-1'></a>示例

```js
const form = new FormData();
form.append('email', info.email);
form.append('name', info.name);
form.append('password', info.password);
form.append('license', info.license, 'a.docx');
axios.post('/restaurant', form, {
  headers: form.getHeaders()
});
```

###  7.2. <a name='-1'></a>用户登录

`POST /customer/session`

调用此API后，如果用户没有注册，则会自动注册并登录；如果用户已经注册，则完成登录。

####  7.2.1. <a name='-1'></a>参数说明

| 参数名 | 数据类型 | 描述         | 必须 |
| ------ | -------- | ------------ | ---- |
| code   | string   | 登录凭证 | 是   |

####  7.2.2. <a name='-1'></a>返回值

| HTTP状态码 | 返回格式            | 描述       |
| ---------- | ------------------- | ---------- |
| 200        | NULL                | 成功       |
| 400        | {message: 'reason'} | 错误的请求 |


####  7.2.3. <a name='api'></a>1. 前端api文档
    [前端api文档](https://documenter.getpostman.com/view/7355822/S1TVXyAV?version=latest#f651b449-dbaa-4758-bab2-cdf4b1acf71b)

####  7.2.4. <a name='api-1'></a>2. 后端api文档
    [后端api文档](https://documenter.getpostman.com/view/7355822/S1TZzbyN?version=latest)


###### 测试文档  
附文件《后端API测试报告》



##  8. <a name='-1'></a>用例设计

####  8.1. <a name='-1'></a>用例文本与活动图

总用例框图
![用例](./assets/usecase.PNG)

一共分为两个用例，一个是发布和接受任务，一个是登陆和注册。
###  8.1. <a name='-1'></a>1.发布和接收任务
BCE模式下的ssd如下

![](assets/sequence1.PNG)


状态图如下

![](assets/state_sign.PNG)

###  8.2. <a name='-1'></a>2.登陆注册

BCE模式下的ssd如下


![](assets/publishersignup.PNG)

状态图如下

用户的注册登陆

![](assets/usersignup.PNG)

发布人的注册登陆

![](assets/publishersignup.PNG)