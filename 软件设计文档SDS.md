<!-- vscode-markdown-toc -->
* 1. [写给TA师兄/师姐看](#TA)
* 2. [技术选型](#)
	* 2.1. [client端](#client)
	* 2.2. [server端](#server)
* 3. [软件架构设计](#-1)
	* 3.1. [1. 软件架构的逻辑视图](#-1)
	* 3.2. [2. 软件架构的物理视图](#-1)
	* 3.3. [部署细节](#-1)
		* 3.3.1. [前端部署说明](#-1)
		* 3.3.2. [前端文件目录](#-1)
		* 3.3.3. [后端源码目录以及说明](#-1)
* 4. [模块划分](#-1)
	* 4.1. [client端](#client-1)
	* 4.2. [server端](#server-1)
* 5. [详细解释具体设计在源码中的体现](#-1)
	* 5.1. [client端](#client-1)
	* 5.2. [server端](#server-1)
* 6. [UI设计文档](#UI)
* 7. [Use case 1](#Usecase1)
* 8. [Version 0.1 (Week 3 ~ Week 8)](#Version0.1Week3Week8)
	* 8.1. [总览](#-1)
	* 8.2. [组件设计思路](#-1)
	* 8.3. [自评](#-1)
* 9. [Version 0.2 (Week 8 ~ Now)](#Version0.2Week8Now)
	* 9.1. [总览](#-1)
* 10. [数据库模式设计](#-1)
* 11. [API设计](#API)
	* 11.1. [用户注册](#-1)
		* 11.1.1. [参数说明](#-1)
		* 11.1.2. [返回值](#-1)
		* 11.1.3. [示例](#-1)
	* 11.2. [用户登录](#-1)
		* 11.2.1. [参数说明](#-1)
		* 11.2.2. [返回值](#-1)
		* 11.2.3. [1. 前端api文档](#api)
		* 11.2.4. [2. 后端api文档](#api-1)
* 12. [用例设计](#-1)
		* 12.1. [用例文本与活动图](#-1)
	* 12.1. [1.发布和接收任务](#-1)
	* 12.2. [2.登陆注册](#-1)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->
# 软件设计文档（SD）


##  1. <a name='TA'></a>写给TA师兄/师姐看

因为我们这次课程的项目要求和 **系统分析与设计** 课程要求的大作业一致，但是文档有很大不同，最关键的一点是因为原先的系统分析与设计课程组队在我们在知道综合实训组队要求的4-8人之前，整个团队已经有9个人了，不符合综合实训的要求。所以原来的团队分成4人和5人的队伍，我们队伍继续把这学期做的**任务种包系统**作为综合实训的作业上交，重新新建了一个org，为的是方便看只有我们四个人的贡献度。另外5人的团队把另一个课程项目作为综合实训的结果上交。如果TA师兄/师姐需要追踪整个项目的commit记录，希望用[系统分析课程项目（由这个repo移植过来）](https://github.com/earn-big-money) 的repo用于参考，**其中整个项目commit的记录从第四周一直保持到第16周**，我们四个人作为项目的主要代码贡献者，确确实实这个学期花了非常多心思在这个项目之上，希望TA师兄/师姐能够理解我们这个项目并不是把现成的写好的项目直接就交上去，从这个项目的题目也可以知道是这个学期才会写的新项目，我们在贡献率方面已经去除了其他同学的贡献。(如果师兄/师姐对于我们整个项目的真实性和持久性还存在疑问，请务必联系我（qq：715817451），因为这门课是我本科的最后三门课之一，每一分都对我未来的影响非常非常大，请师兄/师姐能够给我一个解释的机会)

为了方便检查，我们把能够真实体现我们持久commit的记录从系统分析的org里面贴出来
- 前端持续commit

    ![](./assets/EBM_frontend.PNG)
- 后端持续commit

    ![](./assets/EBM_backend.PNG)
- 需求/设计文档持续commit

    ![](./assets/EBM_doc.PNG)
- 小组个人持续commit记录和commit数量

    - 16350027 @ huangjundashuaige

        ![](./assets/EBM_huangjundashuaige.PNG)
    - 16340218  @ wkfdb

        ![](./assets/EBM_wkfdb.PNG)
    - 16340108 @ huntingwolf320

        ![](./assets/EBM_huntingwolf.PNG)
    - 16340212 @ sherluck314

        ![](./assets/sherluck.PNG)



整个设计文档分成两部分，为了方便检查把课程要求检查的技术选型理由、架构设计、模块划分、详细解释具体设计在源码中的体现 提前放在SD的最前几个小结。




##  2. <a name=''></a>技术选型
###  2.1. <a name='client'></a>client端
可选的技术选型及其选择理由分析
- 前端框架

  - Vue

    Vue是一套用于构建用户界面的渐进式框架，可以自底向上逐层应用。Vue容易上手，便于与第三方库整合（例如element-ui），另外Vue是中国开发的前端框架，官方文档全中文，便于学习使用，另外开发人员之前有学习过Vue，所以采用Vue来搭建前端是最合理的选择。

  - element-ui

    element-ui是一个与Vue兼容的桌面端组件库，该组件库中包含众多网站开发中常用的控件，如下拉菜单，导航栏，进度条等等，官网上每个例子后面都有实现代码，方便易用，而且这些控件UI十分优美，可以极大地提高用户体验。

  - jQuery

    jQuery的重要性不言而喻，控制页面组建的最方便的js包。在前端开发的过程中，将jQuery嵌入到了Vue中，加速了前端页面的开发。

###  2.2. <a name='server'></a>server端
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

##  3. <a name='-1'></a>软件架构设计

###  3.1. <a name='-1'></a>1. 软件架构的逻辑视图

![](./assets/logic_arch.PNG)

架构是前后端分离，前端发放服务器和后端交互服务器分离，其中用restful-like api进行交互。用mysql作为持久化层。

前端的模式为mvc模式，其中使用到的外部支持库有编辑调查表的survey.js

###  3.2. <a name='-1'></a>2. 软件架构的物理视图

![](./assets/physics_arch.PNG)

架构的物理层架构，分别分成了前端用户的浏览器访问端，从静态服务器中获取前端文件，用restful的方式从api服务器去进行交互，其中涉及到持久层的交互由与mysql dbrs完成。



###  3.3. <a name='-1'></a>部署细节

####  3.3.1. <a name='-1'></a>前端部署说明

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

####  3.3.2. <a name='-1'></a>前端文件目录

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

####  3.3.3. <a name='-1'></a>后端源码目录以及说明
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


##  4. <a name='-1'></a>模块划分


###  4.1. <a name='client-1'></a>client端

- 前端的划分主要体现在页面的划分上。前端根目录的src文件夹是需要自己编写的部分，里面记录了页面文件以及路由配置。为了便于管理，路由配置全部放到了index.js文件中。前端页面全部放到components文件下。前端的分工主要体现在页面的分工上，不用的人完成不用部分的页面。前端页面总共分为8个模块，分别为

  - 创建任务（CreateTask）
  - 个人信息（Information）
  - 登录（Login）
  - 注册（Register）
  - 首页（MainPage）
  - 问卷系统（Survey）
  - 任务详细信息（包含接受任务流程）(TaskDetail)
  - 图片管理（Photo）

  这个7个模块用到的vue页面，js，css以及静态资源等全部独立地放到不同的文件夹中。



###  4.2. <a name='server-1'></a>server端

- 后端的划分主要体现在了功能或者说是逻辑的划分上。后端将各个具体的功能模块进行充分的抽象和分层：routes用来控制路由的跳转，database抽象数据库的各项操作，controller则是二者之间的衔接模块，根据上层路由的请求调用底层的数据库方法。app.js位于最顶层，通过调用routes来统领全局。
- routes和controller中主要包含如下一一对应的模块：
  - 个人信息(users)
  - 主页(index)
  - 事务(duties)
  - 问卷(survey)
  - 交易(trades)
  - 图片(photo)


##  5. <a name='-1'></a>详细解释具体设计在源码中的体现
###  5.1. <a name='client-1'></a>client端

- 结构化编程：为了便于前端的分工，将前端需要实现的页面以及业务流程进行了划分，大致分为了8个模块，分别为创建任务（CreateTask），个人信息（Information），登录（Login），注册（Register），首页（MainPage），问卷系统（Survey），任务详细信息（包含接受任务流程），(TaskDetail)，图片管理（Photo）。划分好模块之后编程变得更加容易。

  前端模块源码链接：https://github.com/whatsup-sysu/Frontend/tree/master/whatsup/src/components

- 回调函数，使用回调函数异步地向后端发送请求，支持了并发访问，优化用户体验。

```
//例子：首页在创建的时候需要向后端清楚数据，将目前正在发布中的任务数据保存到前端寄存器，便于任务的显示
//以及对任务的各种操作。请求数据采用异步请求使用回调函数来进行实现。
created: function() {
    this.uid = this.$route.query.uid
    if(this.uid==null){
      this.uid=$("#username").text();
    }
    if(this.id==null){
      this.uid = this.$cookies.get('id')
    }
    //如果当前没有任务数据的话就异步地向后端请求数据
    if(this.nowduty==null){
      var _self = this;
      //采用ajax的方式异步请求
      $.ajax({
        type: 'GET',
        url: '/api/duties/screen?pageNumber=1&countPerPage=7&sortType=time&sortOrder=ascend',
        dataType: 'json',
        timeout: 3000,
        success: function(result, xhr) {
        
          //成功请求数据之后的回调函数，解析并保存得到的数据
          console.log(result)
          _self.nowduty = result['content']
          _self.nowpage = 1
          _self.nowdutynum = result['count']
          if(_self.nowdutynum<7){
            for(var i=_self.nowdutynum;i<7;i++){
              _self.nowduty.push(0);
            }
          }
          //根据得到的数据动态修改页面内容
          for(var i=0;i<7;i++){
            if(_self.nowduty[i]!=0){
              var content="#dcontent"+(i+1).toString();
              $(content).text(_self.nowduty[i].dintroduction);
              var type = "#dtype"+(i+1).toString();
              $(type).text(_self.nowduty[i].dtype);
              var title = "#dtitle"+(i+1).toString();
              $(title).text(_self.nowduty[i].dtitle);
              var sponsor = "#dsponsor"+(i+1).toString();
              $(sponsor).text(_self.nowduty[i].dsponsor);
              var modifyTime = "#dmodifyTime"+(i+1).toString();
              $(modifyTime).text(_self.nowduty[i].dmodifyTime);
              var curaccepters = "#dcuraccepters"+(i+1).toString();
              $(curaccepters).text(_self.nowduty[i].curaccepters);
            } else {
              //alert("hidden");
              var id="#u128-"+(i+1).toString();
              $(id).css("visibility","hidden");
            }
          }
        },
        //请求数据失败之后的回调函数，提示出错，输出错误信息。
        error: function(result, xhr) {
          console.log(result)
          alert('服务器连接错误: ' + xhr)
        }
      })
    }
  }
```



###  5.2. <a name='server-1'></a>server端
后端架构设计技术：
* Service Oriented Architecture  
首先后端是按照面向服务的体系结构开发的，它按照一个基于RESTFUL风格标准定义的API接口文档。服务是最核心的抽象手段，业务被划分为一系列粗粒度的业务服务和业务流程。  
架构开发思想主要体现在[API文档](https://github.com/whatsup-sysu/documents/blob/master/%E6%BA%90%E4%BB%A3%E7%A0%81%E7%9B%AE%E5%BD%95%E5%8F%8A%E8%AF%B4%E6%98%8E/%E5%90%8E%E7%AB%AFAPI%E6%96%87%E6%A1%A3.html)，之后的接口发开都是基于接口文档的标准进行。

* Object-Oriented Programming  
后端在路由分级后按照业务类型抽象了几个处理不同类型业务的对象类型，如userSystem、dutySystem、tradeSystem等等。这些对象封装了相关的业务逻辑方法。抽象的对象列表相关代码主要在[控制器目录](https://github.com/whatsup-sysu/Backend/tree/master/src/controller)中。

##  6. <a name='UI'></a>UI设计文档
# UI Design

##  7. <a name='Usecase1'></a>Use case 1

![](assets/ui.png)
---

##  8. <a name='Version0.1Week3Week8'></a>Version 0.1 (Week 3 ~ Week 8)

###  8.1. <a name='-1'></a>总览

第一版设计思路主要如下
- 逻辑上按照现有常见的社交功能平台设计思路，对现有活动进行分类，并将热门活动在首页展示出来。每个对应有详情页面，可以选择加入活动等。能有用户登录注册功能，对每个用户，有用户详情页可以对其进行管理。这个的主要功能思路
  1. 登陆注册用户
  2. 选择喜欢的活动
  3. 查看活动详情，并选择参加或不参加
  4. 点击个人头像，在个人详情页面可以看到已加入和已发布的任务
- 视觉效果上，采用顶部导航栏的形式，将各个功能的页面结合在一起，同时又能方便用户快速定位功能点。在主页的设计上，采用了和现在大众的社交网络类似的布局，使用web前端中element-ui和fontawesome的元素和icon，利用卡片的布局方式，设计了各个活动排列的方式。在个人主页也是一种常用社交风格、element-ui配色方案设计的。这样的设计适应了大多数人的使用习惯，也能让各个页面之间的逻辑关系比较清晰的排列出来。用element-ui的风格，能在ui上比较简洁方便，主页走马灯的设计，也能比较好的适应热点活动的功能需求。

整个web页面在设计视觉效果上可以有大概一下四个不同的页面：

1. 主页面  
  用走马灯的形式展示最热的活动功能，用卡片的形式展示全部活动，用导航栏的形式链接各个页面和各个功能。  
  ![](assets/mainpage1.png)   
  ![](assets/activities.png)  

2. 活动详情页   
  用左右页面设计的形式，区别活动详情和发起者信息。  
  ![](assets/activityInfo.png)  

3. 个人页面  
   个人页面采用和普通社交网络的设计风格，用上下式的，上部分展示个人基本信息，下部分展示与其有关的全部活动。  
   ![](assets/perInfo1.png)  

4. 创建活动或调查页面  
  这部分和一般的调查问卷的页面类似，不过结合需求，暂时使用固定题型的形式发布和创建。  
  ![](assets/createActivity.png)


###  8.2. <a name='-1'></a>组件设计思路

1. 活动卡片。类别卡片采用占据一行排列，展示活动的
2. 个人页面陈列，将次要的数据信息放到了和头像平齐的地方，而将重要的参与活动信息放到了大块的下方，这样能突出重点。
3. 信息陈列上，主要信息会突出于底层。这里用了颜色或者边框阴影进行突出。每个部分有相应的大标题来指引

###  8.3. <a name='-1'></a>自评

1. 逻辑较为清晰，界面不太拥挤
2. 导航栏的设计能让全部功能更好的结合在一起，而且这也符合element-ui的设计风格
3. 设计语模块不太同一，个人页面和其他页面的风格不太一致，不是选择用卡片式展现

##  9. <a name='Version0.2Week8Now'></a>Version 0.2 (Week 8 ~ Now)

###  9.1. <a name='-1'></a>总览

根据上一版本的思考之后，为了解决填写问卷的时候能增加用户需要的提醒，所以需要对活动发布的页面进行改进。同时为了让个人页面和活动详情页能在整体上风格更加的一致，所以将个人页面的布局从上下分割改成了左右分割。  
1. 将原来发布活动的页面从简陋的固定题型发布改成了可以选择题型的形式，让这个页面中的功能更能适应比较多的活动需求  
   ![](assets/createActivityNow.png)  

2. 为了让个人页面和活动详情页面能有比较统一的风格，在这版里面稍微的改动了一下个人页面的布局，将上下界面划分这种方法改成了左右划分。  
   ![](assets/userinfoNow.png)



##  10. <a name='-1'></a>数据库模式设计
这里包含了系统基础框架所需的完整数据库设计。 


![](./assets/database_er.png)

我们并没有对于权限进行设计，而是直接把用户和发布人的功能进行不同的划分，能够调用的api不同，而不是相同的api所能执行的功能因为权限不同而产生不同的效果。

##  11. <a name='API'></a>API设计

采用了前后端分离的api设计，因为api太多所以只列了作为样例的几个。

###  11.1. <a name='-1'></a>用户注册

`POST /user`

由于需要上传文件，要求使用`multipart/form-data`提交请求。

####  11.1.1. <a name='-1'></a>参数说明

| 参数名   | 数据类型               | 描述     | 必需 |
| -------- | ---------------------- | -------- | ---- |
| email    | string                 | 注册邮箱 | 是   |
| password | string                 | 密码     | 是   |
| name     | string                 | 用户名   | 是   |
| license(optional)  | file（docx、doc、pdf） | 组织证明 | 是   |

####  11.1.2. <a name='-1'></a>返回值

| HTTP状态码 | 返回格式            | 描述       |
| ---------- | ------------------- | ---------- |
| 200        | NULL                | 成功       |
| 400        | {message: 'reason'} | 错误的请求 |

####  11.1.3. <a name='-1'></a>示例

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

###  11.2. <a name='-1'></a>用户登录

`POST /customer/session`

调用此API后，如果用户没有注册，则会自动注册并登录；如果用户已经注册，则完成登录。

####  11.2.1. <a name='-1'></a>参数说明

| 参数名 | 数据类型 | 描述         | 必须 |
| ------ | -------- | ------------ | ---- |
| code   | string   | 登录凭证 | 是   |

####  11.2.2. <a name='-1'></a>返回值

| HTTP状态码 | 返回格式            | 描述       |
| ---------- | ------------------- | ---------- |
| 200        | NULL                | 成功       |
| 400        | {message: 'reason'} | 错误的请求 |


####  11.2.3. <a name='api'></a>1. 前端api文档
    [前端api文档](https://documenter.getpostman.com/view/7355822/S1TVXyAV?version=latest#f651b449-dbaa-4758-bab2-cdf4b1acf71b)

####  11.2.4. <a name='api-1'></a>2. 后端api文档
    [后端api文档](https://documenter.getpostman.com/view/7355822/S1TZzbyN?version=latest)


###### 测试文档  
附文件《后端API测试报告》



##  12. <a name='-1'></a>用例设计

####  12.1. <a name='-1'></a>用例文本与活动图

总用例框图
![用例](./assets/usecase.PNG)

一共分为两个用例，一个是发布和接受任务，一个是登陆和注册。
###  12.1. <a name='-1'></a>1.发布和接收任务
BCE模式下的ssd如下

![](assets/sequence1.PNG)


状态图如下

![](assets/state_sign.PNG)

###  12.2. <a name='-1'></a>2.登陆注册

BCE模式下的ssd如下


![](assets/publishersignup.PNG)

状态图如下

用户的注册登陆

![](assets/usersignup.PNG)

发布人的注册登陆

![](assets/publishersignup.PNG)
