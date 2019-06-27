后端API测试报告

## 测试简介

测试对象是服务器后端，采用白盒测试，以达到高代码覆盖率。

## 测试环境与工具

nodejs+postman

## 测试方式

- 单元测试：针对各个用例的源代码，编写测试用例，分别在测试路径和生产环境路径下进行较为简略和较为详细的测试。
- 压力测试：针对每个测试单元进行短期内超量请求的测试，以探测系统的漏洞和极限。

## 测试流程

- 在Body中确定发送请求的字段，并将其值修改为变量的形式

  [![1560787866432](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560787866432.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560787866432.png)

- 在Pre-request Script中，对变量进行赋值

[![1560787926763](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560787926763.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560787926763.png)

- 在Tests中，编写测试条件

[![1560787986006](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560787986006.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560787986006.png)

- 在csv文件中，准备测试样例

[![1560788144827](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560788144827.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560788144827.png)

- 点击run，选择外部数据文件和循环次数，开始测试。

## 测试用例

#### 测试路径

- 单元测试

  > 由于测试路径下只有简单的不带参数的get查询命令，因此只需测试返回的response code值是否为200以及响应时间是否在200ms以内即可。

  - 测试代码：

  ```
  tests["返回code为200"] = responseCode.code === 200;
  pm.test("响应时间小于 200ms", function () {
      pm.expect(pm.response.responseTime).to.be.below(200);
  });
  ```

  - 测试结果

    [![1560430575167](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560430575167.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560430575167.png)

- 压力测试

  > 测试时设置delay为0，iteration为300。

  [![1560479306936](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560479306936.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560479306936.png)

#### 生产环境路径

##### 用户相关

1.用户注册

- id格式检测

  - 测试样例

    ```
    uid,uname,uemail,upassword,uphone,ustatus
    "1111111","hhh","222@22.com","123456",11111111111,"student"
    "1111a11","hhh","222@22.com","123456",11111111111,"student"
    "aa1","hhh","222@22.com","123456",11111111111,"student"
    "aaaaaaaaaaa1","hhh","222@22.com","123456",11111111111,"student"
    ```

  - 检测条件

  ```
  pm.test("id格式错误", function () {
       var jsonData = JSON.parse(responseBody);
       console.log(responseBody)
       pm.expect(jsonData.msg).to.eql("The id format is incorrect(6-8 letter or digit, begin with letter)");
   });
  ```

  - 测试结果

  [![1560488172888](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560488172888.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560488172888.png)

  - 测试总结

    | 测试内容         | 通过(Y/N) |
    | ---------------- | --------- |
    | id必须包含字母   | Y         |
    | id必须以字母开头 | Y         |
    | id不得小于6位    | Y         |

  | id不得大于8位 | Y |

  - 测试说明

    一切正常。

- 电话格式检测

  - 测试样例

    ```
    uid,uname,uemail,upassword,uphone,ustatus
    "aaaaaa1","hhh","222@22.com","123456",1111111a111,"student"
    "aaaaaa1","hhh","222@22.com","123456",01111111111,"student"
    "aaaaaa1","hhh","222@22.com","123456",1111111111,"student"
    "aaaaaa1","hhh","222@22.com","123456",111111111111,"student"
    ```

  - 检测条件

    ```
    pm.test("电话格式错误", function () {
        var jsonData = JSON.parse(responseBody);
        console.log(responseBody)
        pm.expect(jsonData.msg).to.eql("The phone format is incorrect(11 digit number, begin with ~0");
    });
    ```

  - 测试结果

    [![1560495102802](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560495102802.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560495102802.png)

  - 测试总结

    | 测试内容         | 通过(Y/N) |
    | ---------------- | --------- |
    | 电话不能包含字母 | Y         |
    | 电话必须以0开头  | Y         |
    | 电话不得小于11位 | Y         |
    | 电话不得大于11位 | Y         |

  - 测试说明

    一切正常。

- 邮箱格式检测

  - 测试样例

    ```
    uid,uname,uemail,upassword,uphone,ustatus
    "aaaaaa1","hhh","22222.com","123456",11111111111,"student"
    "aaaaaa1","hhh","222@22com","123456",11111111111,"student"
    "aaaaaa1","hhh","@22.com","123456",11111111111,"student"
    "aaaaaa1","hhh","222@22.","123456",11111111111,"student"
    "aaaaaa1","hhh","222@.com","123456",11111111111,"student"
    ```

  - 检测条件

    ```
    pm.test("邮箱格式错误", function () {
        var jsonData = JSON.parse(responseBody);
        console.log(responseBody)
        pm.expect(jsonData.msg).to.eql('The email format is incorrect(XXX@XXX.XXX)');
    });
    ```

  - 测试结果

    [![1560497902693](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560497902693.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560497902693.png)

  - 测试总结

    | 测试内容             | 通过(Y/N) |
    | -------------------- | --------- |
    | 邮箱必须包含@        | Y         |
    | 邮箱必须包含.        | Y         |
    | 邮箱@之前要有东西    | Y         |
    | 邮箱.之后要有东西    | Y         |
    | 邮箱.和@之间要有东西 | Y         |

  - 测试说明

    一切正常。

- 密码格式检测

  - 测试样例

    ```
    uid,uname,uemail,upassword,uphone,ustatus
    "aaaaaa1","hhh","222@22.com","123",11111111111,"student"
    "aaaaaa1","hhh","222@22.com","123456123456123",11111111111,"student"
    ```

  - 检测条件

    > 发现写了密码的判断函数，但是没有在路由中进行判断，之后对其进行补充并测试。

    ```
    if(!validator.checkPhone(req.body.password)) {
            utils.sendError(res, 400, "The password format is incorrect(6~12 digit number)");
            return;
        }
    ```

    ```
    pm.test("密码格式错误", function () {
        var jsonData = JSON.parse(responseBody);
        console.log(responseBody)
        pm.expect(jsonData.msg).to.eql("The password format is incorrect(6~12 digit number)");
    });
    ```

  - 测试结果

    [![1560500947631](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560500947631.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560500947631.png)

  - 测试总结

    | 测试内容         | 通过(Y/N) |
    | ---------------- | --------- |
    | 密码不得小于6位  | Y         |
    | 密码不得大于12位 | Y         |

  - 测试说明

    修改之后一切正常。

- 重复输入检测

  - 测试样例

    数据库中已存在的数据：

    [![1560501331544](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560501331544.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560501331544.png)

    ```
    uid,uname,uemail,upassword,uphone,ustatus
    "test1","hhh","22222.com","123456","11111111111","student"
    "aaaaaa1","hhh","12345678@qq.com","123456","11111111111","student"
    "aaaaaa1","hhh","222@22.com","123456","12345678910","student"
    ```

  - 检测条件

    ```
    pm.test("插入失败", function () {
        pm.expect(pm.response.code).to.be.oneOf([400]);
    });
    ```

  - 测试结果

    [![1560501547811](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560501547811.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560501547811.png)

  - 测试总结

    | 测试内容     | 通过(Y/N) |
    | ------------ | --------- |
    | id不可重复   | Y         |
    | 电话不可重复 | Y         |
    | 邮箱不可重复 | Y         |

  - 测试说明

    一切正常。

- 压力测试

  - 测试条件

    测试三百次

    ```
    pm.test("插入失败", function () {
        pm.expect(pm.response.code).to.be.oneOf([400]);
    });
    pm.test("响应时间小于 200ms", function () {
        pm.expect(pm.response.responseTime).to.be.below(200);
    });
    ```

  - 测试结果

    [![1560501993244](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560501993244.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560501993244.png)

2.用户登陆

- 用户名密码检测

  - 测试样例

    ```
    uid,upassword
    "test0","1234567"
    "test1","test2"
    ```

  - 检测条件

    ```
    pm.test("登陆失败", function () {
        pm.expect(pm.response.code).to.be.oneOf([400]);
    });
    ```

  - 测试结果

    [![1560504264784](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560504264784.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560504264784.png)

  - 测试总结

  | 测试内容           | 通过(Y/N) |
  | ------------------ | --------- |
  | 用户名不存在       | Y         |
  | 用户名与密码不匹配 | Y         |

  - 测试说明

    一切正常。

- 不可重复登陆检测

  - 测试样例

    ```
    uid,upassword
    "test1","test1"
    "test1","test1"
    ```

  - 检测条件

    ```
    
    ```

  pm.test("登陆成功", function () { pm.expect(pm.response.code).to.be.oneOf([200]); }); tests["不可重复登录"] = responseBody.has("already log in");

  ```
  - 测试结果
  
  ![1560504793188](img/1560504793188.png)
  
  - 测试总结
  
  | 测试内容               | 通过(Y/N) |
  | ---------------------- | --------- |
  | 用户密码正确时成功登陆 | Y         |
  | 登陆一次不可重复登陆   | Y         |
  
  - 测试说明
  
  第一次登陆时登陆成功，不可重复登陆检测失败；第二次则登录失败，不可重复登录成功，一切正常。
  ```

- 压力测试

  - 测试条件

    测试三百次

    ```
    pm.test("登陆失败", function () {
        pm.expect(pm.response.code).to.be.oneOf([400]);
    });
    pm.test("响应时间小于 200ms", function () {
        pm.expect(pm.response.responseTime).to.be.below(200);
    });
    ```

  - 测试结果

    [![1560505179269](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560505179269.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560505179269.png)

3.用户登出

- 先登录再登出再登陆&&压力测试

  - 测试样例

    ```
    uid,upassword
    "test1","test1"
    "test1","test1"
    ```

  - 检测条件

    ```
    pm.test("登陆成功", function () {
        pm.expect(pm.response.code).to.be.oneOf([200]);
    });
    pm.test("响应时间小于 200ms", function () {
        pm.expect(pm.response.responseTime).to.be.below(200);
    });
    postman.setNextRequest("用户登出")
    pm.test("登出成功", function () {
        pm.expect(pm.response.code).to.be.oneOf([200]);
    });
    postman.setNextRequest("用户登录")
    ```

  - 测试结果

    [![1560506350444](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560506350444.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560506350444.png)

  - 测试总结

    | 测试内容     | 通过(Y/N) |
    | ------------ | --------- |
    | 登陆登出循环 | Y         |

  - 测试说明

    一切正常。

4.用户查询

- 查询信息&&压力测试

  - 测试样例

    ```
    uid
    "test0"
    ```

  - 检测条件

    ```
    pm.test("查询失败", function () {
       pm.expect(pm.response.code).to.be.oneOf([400]);
    });
    pm.test("响应时间小于 200ms", function () {
       pm.expect(pm.response.responseTime).to.be.below(200);
    });
    ```

  - 测试结果

    [![1560531831602](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560531831602.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560531831602.png)

  - 测试总结

    | 测试内容           | 通过(Y/N) |
    | ------------------ | --------- |
    | id不存在时查询错误 | Y         |

  - 测试说明 一切正常

5.用户更新

- 更新信息&&压力测试

  - 测试样例

    ```
    uname,uemail,uphone,ustatus
    "test1","","",""
    "","222@22.com","",""
    "","","12345612345",""
    "","","","student"
    "test1","222@22.com","12345612345","student"
    ```

  - 检测条件

    ```
    pm.test("更新成功", function () {
        pm.expect(pm.response.code).to.be.oneOf([200]);
    });
    pm.test("响应时间小于 200ms", function () {
        pm.expect(pm.response.responseTime).to.be.below(200);
    });
    ```

  - 测试结果

    [![1560579559076](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560579559076.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560579559076.png)

  - 测试总结

    | 测试内容       | 通过(Y/N) |
    | -------------- | --------- |
    | 单独更新用户名 | Y         |
    | 单独更新邮箱   | Y         |
    | 单独更新电话   | Y         |
    | 单独更新状态   | Y         |
    | 全部更新       | Y         |

  - 测试说明

    一切正常

##### 事务相关

1.创建事务

- 数字有误

  - 测试样例

    ```
    title,accepters,introduction,content,starttime,endtime,money,type
    "kill pig",1.0.0,"kill some pigs for me","kill some pigs for me","2019-05-04 18:18:18","2019-05-04 18:18:193",10,"survey"
    "kill pig",-1,"kill some pigs for me","kill some pigs for me","2019-05-04 18:18:18","2019-05-04 18:18:193",10,"survey"
    "kill pig",10,"kill some pigs for me","kill some pigs for me","2019-05-04 18:18:18","2019-05-04 18:18:193",1.0.0,"survey"
    "kill pig",10,"kill some pigs for me","kill some pigs for me","2019-05-04 18:18:18","2019-05-04 18:18:193",-1,"survey"
    ```

  - 检测条件

    ```
    pm.test("更新错误数字失败", function () {
       pm.expect(pm.response.code).to.be.oneOf([400]);
    });
    ```

  - 测试结果

    [![1560588012540](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560588012540.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560588012540.png)

  - 测试总结

    | 测试内容           | 通过(Y/N) |
    | ------------------ | --------- |
    | 接受者数字格式错误 | Y         |
    | 接受者数字为负     | N         |
    | 金额数字格式错误   | Y         |
    | 金额数字为负       | N         |

  - 测试说明

    经检查源码，发现未对数字是否为负进行校验。

- 时间有误

  - 测试样例

    ```
    title,accepters,introduction,content,starttime,endtime,money,type
    "kill pig",10,"kill some pigs for me","kill some pigs for me","201905-04 18:18:18","2019-05-04 18:18:19",10,"survey"
    "kill pig",10,"kill some pigs for me","kill some pigs for me","2019-05-04 18:18:18","2019-05-04 18:18:193",10,"survey"
    "kill pig",10,"kill some pigs for me","kill some pigs for me","2019-05-04 18:18:20","2019-05-04 18:18:13",10,"survey"
    ```

  - 检测条件

    ```
    pm.test("更新错误时间失败", function () {
       pm.expect(pm.response.code).to.be.oneOf([400]);
    });
    ```

  - 测试结果

    [![1560588614265](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560588614265.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560588614265.png)

  - 测试总结

    | 测试内容               | 通过(Y/N) |
    | ---------------------- | --------- |
    | 时间格式错误           | N         |
    | 时间超出范围           | N         |
    | 起始时间在终止时间之后 | N         |

  - 测试说明

    经检查源码，发现未对时间的逻辑进行校验。

- 金额不足

  - 测试样例

    ```
    title,accepters,introduction,content,starttime,endtime,money,type
    "kill pig",10,"kill some pigs for me","kill some pigs for me","201905-04 18:18:18","2019-05-04 18:18:19",10,"survey"
    "kill pig",10,"kill some pigs for me","kill some pigs for me","2019-05-04 18:18:18","2019-05-04 18:18:193",10,"survey"
    "kill pig",10,"kill some pigs for me","kill some pigs for me","2019-05-04 18:18:20","2019-05-04 18:18:13",10,"survey"
    ```

  - 检测条件

    ```
    pm.test("金额不足错误", function () {
        var jsonData = JSON.parse(responseBody);
        console.log(responseBody)
        pm.expect(jsonData.msg).to.eql("Not enough money");
    });
    ```

  - 测试结果

    [![1560589107637](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560589107637.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560589107637.png)

  - 测试总结

    | 测试内容                 | 通过(Y/N) |
    | ------------------------ | --------- |
    | 事务总金额超过发布者金额 | Y         |

  - 测试说明

    一切正常。

2.领取任务

- 任务不存在

  - 测试样例

    ```
    did
    "test0_1560170938574"
    ```

  - 检测条件

    ```
    tests["任务不存在"] = responseBody.has("does not exist");
    ```

  - 测试结果

    [![1560599385244](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560599385244.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560599385244.png)

  - 测试总结

    | 测试内容     | 通过(Y/N) |
    | ------------ | --------- |
    | 任务id不存在 | Y         |

  - 测试说明

    一切正常。

- 任务人数已满

  - 测试样例

    （以之前测试的accepter个数为负的样例进行单次测试）

    [![1560599782118](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560599782118.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560599782118.png)

    ```
    did
    "test1_1560587976316"
    ```

  - 检测条件

    ```
    pm.test("任务人数已满", function () {
        var jsonData = JSON.parse(responseBody);
        console.log(responseBody)
        pm.expect(jsonData.msg).to.eql("The number of accepters is full");
    });
    ```

  - 测试结果

    [![1560600090495](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560600090495.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560600090495.png)

  - 测试总结

    | 测试内容     | 通过(Y/N) |
    | ------------ | --------- |
    | 任务人数已满 | N         |

  - 测试说明

    通过检查发现，数据库中的accepter数目对应的字段名为daccepter，但是后端代码错写成了accepter，更改之后顺利通过测试。

    [![1560600863302](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560600863302.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560600863302.png)

- 任务重复认领

  - 测试样例

    ```
    
    ```

did "test1_1560587976563" "test1_1560587976563" ```

- 检测条件

```
  pm.test("成功认领", function () {
    var jsonData = JSON.parse(responseBody);
      console.log(responseBody)
      pm.expect(jsonData.msg).to.eql("success.");
  });
  pm.test("不可重复认领", function () {
      var jsonData = JSON.parse(responseBody);
      console.log(responseBody)
      pm.expect(jsonData.msg).to.eql("Duty cannot be re-requested");
});
```

- 测试结果

  [![1560602410955](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560602410955.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560602410955.png)

- 测试总结

  | 测试内容 | 通过(Y/N) | | ---------------- | --------- | | 任务不可重复认领 | Y |

- 测试说明

  可以看到对于同一个ID，第一次认领成功，第二次认领失败，不可重复认领成功。

- 压力测试

  - 测试样例

    ```
    did
    "test1_1560587976316"
    ```

  - 检测条件

    ```
    pm.test("任务人数已满", function () {
        var jsonData = JSON.parse(responseBody);
        console.log(responseBody)
        pm.expect(jsonData.msg).to.eql("The number of accepters is full");
    });
    pm.test("响应时间小于 200ms", function () {
        pm.expect(pm.response.responseTime).to.be.below(200);
    });
    ```

  - 测试结果

    [![1560601393031](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560601393031.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560601393031.png)

3.查询任务

- 任务查询&&压力测试

  - 测试样例

    ```
    did
    "test0_1560170938574"
    ```

  - 检测条件

    ```
    tests["任务不存在"] = responseBody.has("does not exist");
    pm.test("响应时间小于 200ms", function () {
        pm.expect(pm.response.responseTime).to.be.below(200);
    });
    ```

  - 测试结果

    [![1560604309528](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560604309528.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560604309528.png)

  - 测试总结

    | 测试内容     | 通过(Y/N) |
    | ------------ | --------- |
    | 任务id不存在 | Y         |

  - 测试说明

    一切正常

4.更新任务

- 任务不存在

  - 测试样例

    ```
    did
    "test0_1560170938574"
    ```

  - 检测条件

    ```
    tests["任务不存在"] = responseBody.has("does not exist");
    ```

  - 测试结果

    [![1560606402322](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560606402322.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560606402322.png)

  - 测试总结

    | 测试内容     | 通过(Y/N) |
    | ------------ | --------- |
    | 任务id不存在 | Y         |

  - 测试说明

    一切正常。

- 非任务发起者

  - 测试样例

    ```
    did
    "test1_1560587976316"
    ```

  - 检测条件

    ```
    pm.test("非任务发起者", function () {
        var jsonData = JSON.parse(responseBody);
        console.log(responseBody)
        pm.expect(jsonData.msg).to.eql("you have no permission to change it");
    });
    ```

  - 测试结果

  [![1560606984637](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560606984637.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560606984637.png)

  - 测试总结

    | 测试内容             | 通过(Y/N) |
    | -------------------- | --------- |
    | 非发起者不得删除任务 | Y         |

  - 测试说明

    一切正常

- 任务有人认领不可更新

  - 测试样例

    ```
    did
    "test1_1560587976563"
    ```

- 检测条件

  ```
    pm.test("任务有人认领不可更新", function () {
      var jsonData = JSON.parse(responseBody);
        console.log(responseBody)
      pm.expect(jsonData.msg).to.eql("The duty has been claimed and cannot be changed.");
    });
  ```

- 测试结果

  [![1560607391249](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560607391249.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560607391249.png)

  - 测试总结

    | 测试内容             | 通过(Y/N) |
    | -------------------- | --------- |
    | 任务有人认领不可更新 | Y         |

  - 测试说明

    一切正常

- 余额不足不可更新

  - 测试样例

    ```
    did,money
    "test1_1560587976563",10086
    ```

  - 检测条件

    ```
    pm.test("余额不足不可更新", function () {
        var jsonData = JSON.parse(responseBody);
        console.log(responseBody)
        pm.expect(jsonData.msg).to.eql("The money is not enough");
    });
    ```

  - 测试结果

    [![1560607660733](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560607660733.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560607660733.png)

  - 测试总结

    | 测试内容         | 通过(Y/N) |
    | ---------------- | --------- |
    | 余额不足不可更新 | Y         |

  - 测试说明

    一切正常

- 压力测试

  - 测试样例

    ```
    did,money
    "test1_1560587976563",10086
    ```

  - 检测条件

    ```
    pm.test("余额不足不可更新", function () {
        var jsonData = JSON.parse(responseBody);
        console.log(responseBody)
        pm.expect(jsonData.msg).to.eql("The money is not enough");
    });
    pm.test("响应时间小于 200ms", function () {
        pm.expect(pm.response.responseTime).to.be.below(200);
    });
    ```

  - 测试结果

    [![1560607902253](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560607902253.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560607902253.png)

5.删除任务

- 任务不存在

  - 测试样例

    ```
    did
    "test0_1560170938574"
    ```

  - 检测条件

    ```
    tests["任务不存在"] = responseBody.has("does not exist");
    ```

  - 测试结果

    [![1560608167775](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560608167775.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560608167775.png)

  - 测试总结

    | 测试内容     | 通过(Y/N) |
    | ------------ | --------- |
    | 任务id不存在 | Y         |

  - 测试说明

    一切正常。

- 非任务发起者

  - 测试样例

    ```
    did
    "test2_1560238593804"
    ```

  - 检测条件

    ```
    pm.test("非任务发起者", function () {
        var jsonData = JSON.parse(responseBody);
        console.log(responseBody)
        pm.expect(jsonData.msg).to.eql("you have no permission to delete it");
    });
    ```

  - 测试结果

    [![1560610592186](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560610592186.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560610592186.png)

  - 测试总结

    | 测试内容             | 通过(Y/N) |
    | -------------------- | --------- |
    | 非发起者不得删除任务 | Y         |

  - 测试说明

    一切正常

- 任务有人认领不可删除

  - 测试样例

    ```
    did
    "test1_1560587976563"
    ```

  - 检测条件

    ```
    pm.test("任务有人认领不可删除", function () {
        var jsonData = JSON.parse(responseBody);
        console.log(responseBody)
        pm.expect(jsonData.msg).to.eql("The duty has been claimed and cannot be changed.");
    });
    ```

  - 测试结果

    [![1560610684960](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560610684960.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560610684960.png)

  - 测试总结

    | 测试内容             | 通过(Y/N) |
    | -------------------- | --------- |
    | 任务有人认领不可删除 | Y         |

  - 测试说明

    一切正常

- 压力测试

  - 测试样例

    ```
    did
    "test1_1560587976563"
    ```

  - 检测条件

    ```
    pm.test("任务有人认领不可删除", function () {
        var jsonData = JSON.parse(responseBody);
        console.log(responseBody)
        pm.expect(jsonData.msg).to.eql("The duty has been claimed and cannot be changed.");
    });
    pm.test("响应时间小于 200ms", function () {
        pm.expect(pm.response.responseTime).to.be.below(200);
    });
    ```

  - 测试结果

    [![1560610781553](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560610781553.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560610781553.png)

##### 交易相关

1.用户充值

- 用户不存在&&压力测试

  - 测试样例

    ```
    uid
    "test0"
    ```

  - 检测条件

    ```
    pm.test("充值失败", function () {
       pm.expect(pm.response.code).to.be.oneOf([400]);
    });
    pm.test("响应时间小于 200ms", function () {
       pm.expect(pm.response.responseTime).to.be.below(200);
    });
    ```

  - 测试结果

    [![1560782572758](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560782572758.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560782572758.png)

  - 测试总结

    | 测试内容           | 通过(Y/N) |
    | ------------------ | --------- |
    | id不存在时充值失败 | Y         |

  - 测试说明 一切正常

2.用户提现

- 用户不存在

  - 测试样例

    ```
    uid,amount
    "test0",5
    ```

  - 检测条件

    ```
    pm.test("提现失败", function () {
       pm.expect(pm.response.code).to.be.oneOf([400]);
    });
    ```

  - 测试结果

    [![1560782716152](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560782716152.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560782716152.png)

  - 测试总结

    | 测试内容           | 通过(Y/N) |
    | ------------------ | --------- |
    | id不存在时提现错误 | Y         |

  - 测试说明 一切正常

- 余额不足&&压力测试

  - 测试样例

    ```
    uid,amount
    "test1",500000
    ```

  - 检测条件

    ```
    pm.test("余额不足", function () {
        var jsonData = JSON.parse(responseBody);
        console.log(responseBody)
        pm.expect(jsonData.msg).to.eql("Your balance is not enough.");
    });
    ```

  - 测试结果

    [![1560782953934](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560782953934.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560782953934.png)

  - 测试总结

    | 测试内容           | 通过(Y/N) |
    | ------------------ | --------- |
    | 余额不足时提现错误 | Y         |

  - 测试说明 一切正常

3.用户转账

- 用户不存在

  - 测试样例

    ```
    uid,amount
    "test0",5
    ```

  - 检测条件

    ```
    pm.test("转账失败", function () {
       pm.expect(pm.response.code).to.be.oneOf([400]);
    });
    ```

  - 测试结果

    [![1560783056158](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560783056158.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560783056158.png)

  - 测试总结

    | 测试内容           | 通过(Y/N) |
    | ------------------ | --------- |
    | id不存在时转账错误 | Y         |

  - 测试说明 一切正常

- 不能给自己转账

  - 测试样例

    ```
    uid,amount
    "test1",5
    ```

  - 检测条件

    ```
    pm.test("不能给自己转账", function () {
        var jsonData = JSON.parse(responseBody);
        console.log(responseBody)
        pm.expect(jsonData.msg).to.eql("You can't transfer money to yourself.");
    });
    ```

  - 测试结果

    [![1560783159663](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560783159663.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560783159663.png)

  - 测试总结

    | 测试内容           | 通过(Y/N) |
    | ------------------ | --------- |
    | id为自己时转账错误 | Y         |

  - 测试说明 一切正常

- 余额不足&&压力测试

  - 测试样例

    ```
    uid,amount
    "test1",500000
    ```

  - 检测条件

    ```
    pm.test("余额不足", function () {
        var jsonData = JSON.parse(responseBody);
        console.log(responseBody)
        pm.expect(jsonData.msg).to.eql("Your balance is not enough.");
    });
    ```

  - 测试结果

    [![1560783349232](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560783349232.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560783349232.png)

  - 测试总结

    | 测试内容           | 通过(Y/N) |
    | ------------------ | --------- |
    | 余额不足时转账错误 | Y         |

  - 测试说明 一切正常

##### 问卷相关

1.提交问卷

- 用户不存在&&压力测试

  - 测试样例

    ```
    uid
    "test0"
    ```

  - 检测条件

    ```
    pm.test("问卷提交失败", function () {
       pm.expect(pm.response.code).to.be.oneOf([400]);
    });
    pm.test("响应时间小于 200ms", function () {
       pm.expect(pm.response.responseTime).to.be.below(200);
    });
    ```

  - 测试结果

    [![1560787317564](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560787317564.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560787317564.png)

  - 测试总结

    | 测试内容               | 通过(Y/N) |
    | ---------------------- | --------- |
    | id不存在时问卷提交失败 | Y         |

  - 测试说明 一切正常

2.获取问卷

- 问卷不存在&&压力测试

  - 测试样例

    ```
    did
    "test0_1560587976563"
    ```

  - 检测条件

    ```
    pm.test("问卷获取失败", function () {
       pm.expect(pm.response.code).to.be.oneOf([400]);
    });
    pm.test("响应时间小于 200ms", function () {
       pm.expect(pm.response.responseTime).to.be.below(200);
    });
    ```

  - 测试结果

    [![1560787531525](https://github.com/earn-big-money/Dashboard/raw/master/test/img/1560787531525.png)](https://github.com/earn-big-money/Dashboard/blob/master/test/img/1560787531525.png)

  - 测试总结

    | 测试内容                   | 通过(Y/N) |
    | -------------------------- | --------- |
    | 问卷id不存在时问卷获取失败 | Y         |

  - 测试说明 一切正常

## 测试总结

在测试中，我通过单元测试发现了许多由于种种拼写或遗漏错误导致的小问题，通常改几个字母就能修正过来，除此之外的一些逻辑较为复杂的问题，也通过小组讨论得以解决。在压力测试中，大多数单元都表现良好，足以应对实际中的需求。
