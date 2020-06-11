---
title: react+mobx+typescript项目练手
categories:
  - react
tags:
  - react
---

<!--more-->

## TM 项目用 react+mobx

公司 TM 项目使用自己搭建 hive 框架，react 技术，mobx 做数据状态管理，ts 用来定义一些数据结构以及接口和枚举值，更好的校验数据的类型结构

项目上线后，自己用 react 框架+mobx 以及 github 上有名的网易云音乐接口项目想实践下，目前代理成功。可以获取部分接口，有些接口需要登录成功才可以使用。目前我登录成功能拿到 cookie 存储成功，但是还是不能获取到接口，一直 404，很迷。

## 表单登录检验，成功登录

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200611120126102.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)

## 获取指定 uid 的歌单列表

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200611120528568.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)

## 获取指定 uid 的听歌记录

分为最近一周和所有记录（tab 切换），每个 tab 最高条数 100，支持分页（10 条/页）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200611120445337.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)

## mobx 配合有状态函数使用（class）

```
import React from "react";
import { observer, inject } from "mobx-react";
import { toJS } from "mobx";
import { Button, Form, Input, notification } from "antd";
import "../App.css";
import "antd/dist/antd.css";

@inject(({ Login }) => ({
  setField: Login.setField,
  submitAndLogin: Login.submitAndLogin,
  getCaptcha: Login.getCaptcha,
  profile: Login.profile,
  isLogin: Login.isLogin,
  getPlayList: Login.getPlayList,
}))
@observer
class Login extends React.Component {
  openNotification() {
    const { profile } = this.props;
    let profileToJs = toJS(profile);
    notification.open({
      message: "Login success",
      description: `welcome ${profileToJs.nickName}, your uid is ${profileToJs.userId}`,
    });
  }

  render() {
    const {
      setField,
      submitAndLogin,
      regist,
      isLogin,
      getCaptcha,
    } = this.props;
    return (
      <div>
        <Form className="from">
          <Form.Item
            label="phoneNumber"
            name="phoneNumber"
            rules={[
              { required: true, message: "Please input your phoneNumber" },
            ]}
          >
            <Input
              placeholder="phoneNumber"
              onChange={(e) => setField("phoneNumber", e.target.value)}
            />
          </Form.Item>
          <Form.Item
            label="PassWord"
            name="PassWord"
            rules={[{ required: true, message: "Please input your PassWord" }]}
          >
            <Input
              placeholder="passWord"
              onChange={(e) => setField("passWord", e.target.value)}
            />
          </Form.Item>
          <Form.Item label="验证码" name="captcha">
            <Input
              placeholder="验证码"
              onChange={(e) => setField("captcha", e.target.value)}
            />
            <Button onClick={() => getCaptcha()}>获取验证码</Button>
          </Form.Item>
          <Form.Item label="昵称" name="nickName">
            <Input
              placeholder="昵称"
              onChange={(e) => setField("nickName", e.target.value)}
            />
          </Form.Item>
          <Form.Item>
            <Button
              type="primary"
              htmlType="submit"
              onClick={() => submitAndLogin()}
            >
              Login in
            </Button>
            <Button type="primary" htmlType="regist" onClick={() => regist()}>
              Register
            </Button>
            {isLogin && this.openNotification()}
          </Form.Item>
        </Form>
      </div>
    );
  }
}

export default Login;
```

antd 4.0 移除了 From.create 和 onSubmit 改用 onFinish 会自动验证

## create=react-app 引入 typescript

```
首次：create-react-app demo02 --typescript
已构建： npm install --save typescript @types/node @types/react @types/react-dom @types/jest
```
