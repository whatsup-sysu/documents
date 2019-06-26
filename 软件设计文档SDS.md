# 软件设计文档

## UI设计文档

    廖磊负责

## 数据库模式设计
这里包含了系统基础框架所需的完整数据库设计。 


![](./assets/database_er.png)

我们并没有对于权限进行设计，而是直接把用户和发布人的功能进行不同的划分，能够调用的api不同，而不是相同的api所能执行的功能因为权限不同而产生不同的效果。

## API设计

采用了前后端分离的api设计

## 用户注册

`POST /user`

由于需要上传文件，要求使用`multipart/form-data`提交请求。

### 参数说明

| 参数名   | 数据类型               | 描述     | 必需 |
| -------- | ---------------------- | -------- | ---- |
| email    | string                 | 注册邮箱 | 是   |
| password | string                 | 密码     | 是   |
| name     | string                 | 用户名   | 是   |
| license(optional)  | file（docx、doc、pdf） | 组织证明 | 是   |

### 返回值

| HTTP状态码 | 返回格式            | 描述       |
| ---------- | ------------------- | ---------- |
| 200        | NULL                | 成功       |
| 400        | {message: 'reason'} | 错误的请求 |

### 示例

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

## 用户登录

`POST /customer/session`

调用此API后，如果用户没有注册，则会自动注册并登录；如果用户已经注册，则完成登录。

### 参数说明

| 参数名 | 数据类型 | 描述         | 必须 |
| ------ | -------- | ------------ | ---- |
| code   | string   | 登录凭证 | 是   |

### 返回值

| HTTP状态码 | 返回格式            | 描述       |
| ---------- | ------------------- | ---------- |
| 200        | NULL                | 成功       |
| 400        | {message: 'reason'} | 错误的请求 |


### 1. 前端api文档
    [前端api文档](https://documenter.getpostman.com/view/7355822/S1TVXyAV?version=latest#f651b449-dbaa-4758-bab2-cdf4b1acf71b)

### 2. 后端api文档
    [后端api文档](https://documenter.getpostman.com/view/7355822/S1TZzbyN?version=latest)

## 软件架构设计

![](assets/Architecture.png)

### 系统基本架构

- 小程序与商家端作为前端与用户交互
- 前端通过 HTTPS 与后端沟通
- Nginx 捕获 HTTPS 请求并进行均衡负载，反向代理到任一服务器实例
- Node.js 服务器由多个实例，处理请求
- 服务器通过 Redis 维护 Session
- 使用 MySQL 作为数据库



### 部署细节

- Redis 与 MySQL 通过 Docker 部署
- Nginx 与 Server 均通过 Docker Compose 编排

## 用例设计

#### 用例文本与活动图

总用例图
![用例](./assets/usecase.PNG)


### 1.