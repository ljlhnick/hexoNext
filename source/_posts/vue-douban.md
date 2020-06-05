---
title: 基于vue框架以及豆瓣API实现的小项目
categories: 
  - Vue
  - Jest
tags:
  - nodejs
  - Vue
  - React
---
<!--more-->
# vue+vue-router+axios+vuex+iview UI
router 分为/  /movie  /book
iview UI 使用了Menu， Tabs，Page， Spin， Button(ButtonGroup)，Divider,BackTop
<BackTop></BackTop> in  movie and book.vue on Spin
vuex: movie.js  book.js 

## 技术方案
github地址： https://github.com/ljlhnick/vueDouban
采用的技术支持：
 1. **Vue框架**，基于vue-cli3版本，可以采用vue ui查看图形化界面 ；
 2. **vue-router** router 将路由分为分为/  /movie  /book三大部分；
 3. 采用 **iview UI** 框架，使用了Menu， Tabs，Page， Spin， Button(ButtonGroup)，Divider,BackTop组件(<BackTop></BackTop> in  movie and book.vue on Spin)；
 4.  **vuex** 进行数据的状态管理；
 5. 使用axios与后台进行接口数据的交互；
 6. 采用 **test**和 **E2E** 

## 模块划分

电影：懒加载，tab切换，分页，点击查看详情的路由跳转，回到顶部
电影详情：轮播图，列表渲染
图书：懒加载，tab切换，分页，点击查看详情的路由跳转，回到顶部
图书详情：列表渲染，modal对话框


## 功能图
movie
![电影列表](https://img-blog.csdnimg.cn/20200430173809269.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
![电影详情](https://img-blog.csdnimg.cn/20200430173910296.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)

Book
![书籍列表](https://img-blog.csdnimg.cn/20200430173606571.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
![书籍tab切换](https://img-blog.csdnimg.cn/20200430173655431.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
![书籍详情](https://img-blog.csdnimg.cn/20200430173741476.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)

## vue ui 上 unit 截图
![unit单元测试](https://img-blog.csdnimg.cn/20200430173337642.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
E2E
![黑盒测试](https://img-blog.csdnimg.cn/2020043017345324.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)

## 单元测试覆盖率之jest测试
安装依赖包： jest、vue-jest、bebel-jest， chai（断言库）
配置图：(jest.conf.js)
![jest.conf.js](https://img-blog.csdnimg.cn/20200523172121918.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
在package.json中新增jest命令
![jest命令](https://img-blog.csdnimg.cn/20200523172358292.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)

## 测试用例编写
编写.spec.js/.js测试文件
```
import { expect } from 'chai';
import fetch from "node-fetch";
import Vue from "vue";
import { shallowMount, createLocalVue } from '@vue/test-utils'
import Index from '@/components/Index.vue';
import Vuex from 'vuex';
import { moduleA } from "../../src/store/index.js";

describe('Index.vue', () => {
  //检查原始组件选项
  it('has a created hook',() => {
    expect(typeof Index.created).to.eql("function");
  });

  // 评估Index组件中Carousel组件从下标为0的图片开始
  it('sets the correct default data',() => { 
    expect(typeof Index.data).to.eql('function');
    const defaultData = Index.data();
    expect(defaultData.carouseIndex).to.exist;
    expect(defaultData.carouseIndex).to.eql(0);
  });


  //创建一个实例并检查渲染输出--图片轮播有5张
  it('render the correct message', ()=>{
    const Constructor = Vue.extend(Index);
    const vm = new Constructor().$mount();
    expect(vm.$el.carsouleList).length == 5;
  });

  //通过但是没有被覆盖
  it('check computed value', () => {
    const vm = new Vue(Index).$mount();
    expect(Index.computed.products).length === 4;
    expect(vm.$el.products).length > 0;
  })

  //测试getters
  it('check getters saleProducts', () => {
    const localVue = createLocalVue();
    localVue.use(Vuex);

    const vuexStore = new Vuex.Store(moduleA);
    vuexStore.getters.saleProducts;
    expect(vuexStore.state.products[1].price).to.eql(120);
  })

  //测试mutataions
  it('check mutataions minuPrice', () => {
    const localVue = createLocalVue();
    localVue.use(Vuex);

    const vuexStore = new Vuex.Store(moduleA);
    vuexStore.commit('minuPrice', 2);
    expect(vuexStore.state.products).length === 4;
  })

  it('check mutataions setActiveMenu', () => {
    const localVue = createLocalVue();
    localVue.use(Vuex);

    const vuexStore = new Vuex.Store(moduleA);
    vuexStore.commit('setActiveMenu', '电影');
    expect(vuexStore.state.activeMenu).to.eql("电影");
  })

  //测试action
  it('check minuPriceAsync', () => {
    const localVue = createLocalVue();
    localVue.use(Vuex);

    const vuexStore = new Vuex.Store(moduleA);
    vuexStore.dispatch('minuPriceAsync', 5);
    setTimeout(() => {
      vuexStore.commit('minuPrice', 5);
      expect(vuexStore.state.products[0].price).to.eql(246);
      done(); 
    }, 2000);
  })
});
```
单元测试覆盖率截图（点开coverage下的一个html文件）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200528135358486.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
## 控制台调试
__VUE_DEVTOOLS_GLOBAL_HOOK__.store查看store，有以下属性
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430175551604.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430175645276.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
## 路由拦截
 beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不能获取组件实例 `this`
    // 因为当钩子执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }

