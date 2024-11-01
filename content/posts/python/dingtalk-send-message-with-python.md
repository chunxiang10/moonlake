---
title: '用钉钉给TA发一个特别的加班通知吧'
date: '2023-03-30T14:31:11+0800'
tags: []
categories: 
 - 技术
---

## 桌面环境
- python3.6+

## 钉钉后台设置

#### 登陆钉钉开放平台

​	`https://open-dev.dingtalk.com/`

#### 创建应用

1. 按顺序点击，进入创建应用界面

    ![](/pictures/dingtalk/dingtalk01.png)

    必填项：应用名称和应用描述。

    ![](/pictures/dingtalk/dingtalk02.png)

    显示已经创建成功的应用，其中AgentId、AppKey和AppSecret以后会用到，**不能泄漏给别人**

    ![](/pictures/dingtalk/dingtalk03.png)

2. 点击左侧开发管理，然后点击右上角修改按钮，添加IP白名单。

    ![](/pictures/dingtalk/dingtalk04.png)

    ![](/pictures/dingtalk/dingtalk05.png)

	其中 服务器出口IP按以下方法查询:

	打开[ip138.com](https://ip138.com/),复制红框中的数字，粘贴到服务器出口IP框内。

    ![](/pictures/dingtalk/dingtalk06.png)

	点击右上角保存

3. 点击左侧权限管理，授权应用使用的范围

    ![](/pictures/dingtalk/dingtalk07.png)

	按下面的名称搜索并开通权限

    ![](/pictures/dingtalk/dingtalk08.png)


4. 点击左侧应用功能中的机器人与消息推送，创建机器人

    ![](/pictures/dingtalk/dingtalk09.png)

	只需要填写机器人名称和机器人图标，然后点击发布按钮，然后确认发布。
	
	然后可以看到机器人配置下面有一个**复制RobotCode**的选项，点击可以复制RobotCode。

    ![](/pictures/dingtalk/dingtalk10.png)


到此，应用和机器人创建完成

#### 创建卡片模版

1. 按图顺序打开创建卡片模版页面

    ![](/pictures/dingtalk/dingtalk11.png)


    ![](/pictures/dingtalk/dingtalk12.png)


    ![](/pictures/dingtalk/dingtalk13.png)


2. 选择预设模版，点击使用并确认。

    ![](/pictures/dingtalk/dingtalk14.png)

	然后我们就可以看到预览了

    ![](/pictures/dingtalk/dingtalk15.png)


3. 查看数据结构

    ![](/pictures/dingtalk/dingtalk16.png)

	上面的pv,users,comments,likes都是可以修改值的参数。chart是图表数据，我们用不到，就不用管。

**钉钉后台设置，到此结束。**

## Python代码

<details>
<summary>Python代码（点击展开）</summary>

``` python
import requests
import random
# ↓↓↓修改以下参数，保留引号！！↓↓↓
# -----------------------------------
# 应用的AppKey和AppSecret
appkey = "ding88rinjbedv5c7c4p"
appsecret = "ri9CMiEu2ktS1Vs2afKIGLDXv9Q3KWPogv2g3rEXBbORP-lTeTsUAExZE0jsFYwN"
# 卡片模版的模版ID
template_id = "79e24ee7-0f72-4b7d-a9b6-579363b9bed5"
# 机器人的RobotCode
robotcode = "ding88rinjbedv5c7c4p"
# -----------------------------------
# ↑↑↑修改以上参数，保留引号！！↑↑↑

header = {
    'content-type': 'application/json; charset=UTF-8',
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.0.0 Safari/537.36 Edg/105.0.1343.53'
}

# 获取token
def get_token():
    url = "https://oapi.dingtalk.com/gettoken"
    param = {
        "appkey": appkey,
        "appsecret": appsecret,
    }
    try:
        res = requests.get(url,headers=header,params=param).json()
        return res["access_token"]
    except:
        return "something went wrong"

# 根据手机号获取userID
def get_userid(mobile,token):
    userid_list = []
    url = f"https://oapi.dingtalk.com/topapi/v2/user/getbymobile?access_token={token}"
    for i in mobile:
        data = {
            "mobile": i
        }
        res = requests.post(url,json=data,headers=header).json()
        if res["errcode"] == 0:
            userid_list.append(res["result"]["userid"])
        else:
            return {"err":404,"emsg":"参数错误"}
    return userid_list

# 创建outTrackId
def out_track_id():
    chars = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"
    code = ""
    for i in range(32):
        code += random.choice(chars)
    return code

# 卡片消息 todo
def send_card(userid,token):
    url = "https://api.dingtalk.com/v1.0/im/interactiveCards/send"
    header["x-acs-dingtalk-access-token"] = token
    jsons = {
        "cardTemplateId" : template_id,
        "receiverUserIdList":userid,
        "outTrackId" : out_track_id(),
        "robotCode" : robotcode,
        "conversationType" : 0,
        "cardData" : {
            "cardParamMap" : {
                # 修改下面右侧的参数可以更改数据
                # 保留双引号
                "pv": "2000",
                "users": "1000",
                "comments": "500",
                "likes": "200"
            }
        }
    }
    requests.post(url,json=jsons,headers=header)
    
 def main():
    token = get_token()
    # 修改下面的手机号，保留引号
    mobile = [
        "15012341231",
        "15012341232",
        "15012341233",
    ]
    userid_list = get_userid(mobile,token)
    send_card(userid_list,token)

if __name__ == "__main__":
    main()
```
</details>

---

