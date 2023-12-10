# 微信模板消息推送 2.6.X
 <img src="https://user-images.githubusercontent.com/56298636/231202157-6cf27e30-26ab-4fbb-a6f9-8e200e27a373.jpg" width = "320" height = "520" alt="图片" align=center />

> 注意：一切内容需要使用英文标点符号！

## 快速开始
### 第一步：复制本项目到你的仓库，设置为私有项目
点击复制项目地址

![点击复制项目地址](https://user-images.githubusercontent.com/56298636/190580174-32b7c197-866f-4e94-b886-36b817e40b03.png)

右上角导入按钮

![image](https://user-images.githubusercontent.com/56298636/190580243-f0b4b8ef-9eb3-4ac2-ab5b-d48aa435e0e7.png)

完成导入

![image](https://user-images.githubusercontent.com/56298636/190580561-fb0cc938-6999-4430-aee2-1616362f6857.png)

### 第二步：开启Actions
如果项目上方没有Actions,就要手动开启Actions，步骤为：

1. 点击项目上方的Settings
2. 点击页面左边Actions
3. 点击Actions下面的General
4. 点击页面中间的Allow all actions and rausabel workflows
5. 点击save保持设置

![image](https://user-images.githubusercontent.com/56298636/190582555-5d576513-eac7-43fd-a248-c62d72b7a03a.png)

### 第三步：配置微信测试平台信息
只需要在`com.pt.vx.config.WechatConfig`中配置好`VxAppId`和`VxAppSecret`以及`用户信息`即可，此时你就拥有了一个基础的微信消息，你将可以使用<a href="#baseMb">基础模板</a>。

关于如何获取`VxAppId`和`VxAppSecret`以及`用户信息`，可以查看<a href="#vxTemplateInfo">微信测试号信息操作</a>

如果要使用更多内容，需要到`WeatherConfig`或者`MainCOnfig`中配置。

```java
public class WechatConfig {
    /**
     * 你的微信的APPID
     * appId
     */
    public static final String VxAppId = "appId";

    /**
     * 你的微信的密钥
     * appSecret
     */
    public static final String VxAppSecret = "appSecret";

    public static final List<User> userList = new ArrayList<>();

    static {
        userList.add(getUser(
                "这个人的微信号", //扫码关注你的测试号以后，测试平台会出现TA的微信号
                "模板ID", //要给这个人发送的模板ID
                "pt", //咋称呼这个人
                "江苏省南京市玄武区", //这个人的详细地址
                "南京", //这个人在的城市
                new BirthDay(1999,2,15,true,false,"pt生日快乐！！"),
                new BirthDay(1999,8,11,false,false,"生日快乐哦~~"),
                new BirthDay(2020,7,8,true,true),
                new BirthDay(2020,7,8,true,false,"周年快乐！！！")
        ));

        userList.add(getUser(
                "这个人扫码后的微信号",
                "微信消息模板ID",
                "这个人的称呼",
                "江苏省南京市玄武区",
                "南京",
                new BirthDay(1999,8,11,false,false,"生日快乐哦~~"),
                new BirthDay(1999,2,15,true,false,"pt生日快乐！！"),
                new BirthDay(2020,7,8,true,true),
                new BirthDay(2020,7,8,true,false,"周年快乐！！！")
        ));


    }
```



## 更多内容

### 获取天气

到`WeatherConfig`中进行配置。

1. 开启天气功能，将`OPEN`设置为`true`
2. 获取对应的天气源的key并且填入`weatherSourceKey`，并且将天气源设置`weatherSourceType`设置成对应的天气源
3. 选择需要的天气类型`getWeatherType`

具体如何获取配置信息到<a href="gaode">高德地图信息获取</a>或者<a href="hefeng">和风天气信息获取</a>查看。



### 自定义模板

<span id="diy"></span>

如果要自定义模板，那么就需要到`KeyConfig`中29个内置的key查看需要的内容。

比如日出时间，new KeyDTO右边第一个参数是关键字key，第二个参数是颜色，第三个参数是是否开启

```java
	/**
     * 日出时间
     */
    public static final KeyDTO KEY_SUN_RISE = new KeyDTO("sunrise","#FFFFFF",true);
```

了解这个以后，就可以自定义模板了，然后将自定义的模板到微信测试平台添加即可。

```java
{{userName.DATA}}， 
{{date.DATA}} 周{{week.DATA}} 
今天日出的时间是{{sunrise.DATA}}
```



> 注意：对于日出（sunrise）和日落（sunset）两个key，只有切换天气源为和风天气才有。


### 修改Action执行时间
到`.github/workflows/maven.yml`,修改`cron: "8 0 * * *"`
分别代表："分 时 天 月 星期"
| 字段 | 允许值            |
| ---- | ----------------- |
| 分   | 0-59              |
| 小时 | 0-23              |
| 日期 | 1-31              |
| 月份 | 1-12 或者 JAN-DEC |
| 星期 | 0-6 或者 SUN-SAT  |
值不但可以使用常规允许值，还可以使用下面的特殊符号。
| **符号** | **描述**       | 示例                                                         |
| -------- | -------------- | ------------------------------------------------------------ |
| *        | 任意数值       | `15 * * * *` 在每天的每个小时的15分运行                      |
| ,        | 数值列表分隔符 | `2,10 4,5 * * *` 在每天的第4和第5个小时的第2和第10分运行     |
| -        | 数值范文连接   | `30 4-6 * * *` 在每天的第4和5和6小时的30分运行               |
| /        | 步进数值       | `20/15 * * * *` 在每天的每个小时中，从第20分钟到59分钟每隔15分钟运行一次（即20分、35分和50分运行） |

> 还有一点，github是使用的标准时间，我们要使用的是北京时间，所有需要在原本的基础上-8小时,所有你要设置北京时间早上8点7分的话，就需要设置成标准时间0点7分，即：`cron: "7 0 * * *"`

### 其他内容

到`MainConfig`中自行查看即可，注释写的很清楚。



## 信息获取

###  微信测试号信息获取

<span id="vxTemplateInfo" ></span>

[微信公众号测试平台](https://mp.weixin.qq.com/debug/cgi-bin/sandboxinfo?action=showinfo&t=sandbox/index)

    1. AppID和appSecret在微信公众号测试平台网站最上方

![image](https://user-images.githubusercontent.com/56298636/190580833-949247b1-2ac0-4399-8ec4-b94abbbed0ce.png)

    2. 模板ID在添加模板后生成

![image](https://user-images.githubusercontent.com/56298636/190581136-d03d102c-b668-47af-96f6-89e0a79e88c1.png)

    3. 用户ID在扫码关注后生产

![image](https://user-images.githubusercontent.com/56298636/190581072-9b14c1b3-6564-498e-8546-3b0b93bdeaed.png)

### 高德地图信息获取

<span id="gaode" ></span>

[高德地图开发者平台](https://lbs.amap.com/api/webservice/guide/create-project/get-key)

1. 创建一个新应用

![image](https://user-images.githubusercontent.com/56298636/190583327-dfae0cd2-9450-4b2b-8f3c-dbd0999959f0.png)

2.给应用起个名字

![image](https://user-images.githubusercontent.com/56298636/190583508-09e3c4d6-0063-4dab-9097-a167a594be38.png)

3. 新增一个key
   ![image](https://user-images.githubusercontent.com/56298636/190583577-0d804449-6cba-41f1-a169-f9c5af4c6bfd.png)

4. 配置一下
   ![image](https://user-images.githubusercontent.com/56298636/190583666-ab4fefa1-e560-46cf-acaa-addabbc748ea.png)

5. 获得key

![image](https://user-images.githubusercontent.com/56298636/190584096-2e34f62c-b4d5-4263-bc3e-24727422586d.png)



### 和风天气信息获取

<span id="hefeng" ></span>

可以到[控制台 | 和风天气](https://console.qweather.com/#/console)进行配置。
配置完后就能拿到key了，基本和高德地图一致。

![image](https://user-images.githubusercontent.com/56298636/197333335-bd7d15ae-71bf-46f9-ab06-409d93844ba9.png)



## 模板

这里我提供一些模板，你也可以<a href="#diy">自定模板</a>。

<span id="baseMb">基础模板</span>

```
Dear {{userName.DATA}} 
今天是{{date.DATA}} 周{{week.DATA}} 
我们在一起的{{birthDay3.DATA}}天 
你的生日还有{{birthDay.DATA}}天 
我的生日还有{{birthDay1.DATA}}天 
距离我们下一次纪念还有{{birthDay2.DATA}}天 
额外提示：{{otherInfo.DATA}}{{otherInfoSplit.DATA}} 

随机推送：{{randomInfo.DATA}}{{randomInfoSplit.DATA}}

最后，开心每一天！
```



有天气的模板

```
Dear {{userName.DATA}}， 
今天是{{date.DATA}} 周{{week.DATA}} 
我们在一起的{{birthDay3.DATA}}天 
你的生日还有{{birthDay.DATA}}天 
我的生日还有{{birthDay1.DATA}}天 
距离我们下一次纪念还有{{birthDay2.DATA}}天 
今天白天{{weatherDay.DATA}}，温度{{temperatureDay.DATA}}℃ 
今天晚上{{weatherNight.DATA}}，温度{{temperatureNight.DATA}}℃ 
明天白天{{weatherDay1.DATA}}，温度{{temperatureDay1.DATA}}℃ 
明天晚上{{weatherNight1.DATA}}，温度{{temperatureNight1.DATA}}℃ 
额外提示：{{otherInfo.DATA}}{{otherInfoSplit.DATA}}

随机推送：{{randomInfo.DATA}}{{randomInfoSplit.DATA}}
```
```
Dear {{userName.DATA}}🦦， 
今天是{{date.DATA}} 周{{week.DATA}} 
我们在一起的{{birthDay3.DATA}}天 💕 

日出时间 {{sunrise.DATA}}🌞 
日落时间 {{sunset.DATA}}🌝 
⛅今天白天{{weatherDay.DATA}}，温度{{temperatureDay.DATA}}℃ 
⛅今天晚上{{weatherNight.DATA}}，温度{{temperatureNight.DATA}}℃ 
⛅明天白天{{weatherDay1.DATA}}，温度{{temperatureDay1.DATA}}℃ 
⛅明天晚上{{weatherNight1.DATA}}，温度{{temperatureNight1.DATA}}℃ 

你的生日还有{{birthDay.DATA}}天 🤏 
我的生日还有{{birthDay1.DATA}}天 🤏 
距离我们下一次纪念还有{{birthDay2.DATA}}天 💕 

{{randomInfo.DATA}}{{randomInfoSplit.DATA}}💓 

最后，开心每一天🥰！
```

最后，开心每一天！
```

