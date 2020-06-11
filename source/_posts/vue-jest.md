---
title: vue测试
categories:
  - jest
tags:
  - 单元测试
---

<!--more-->

[原文链接](https://juejin.im/post/5e18863df265da3e1932cddc#heading-4)
[store](https://blog.csdn.net/tonylua/article/details/103750356)

## 测试类型

三类测试：单元测试、集成测试、端到端测试
单元测试：单独使用在单个代码单元（类、函数）
集成测试：检查多个单元是否协同（组件层次结构、组件+存储）
端到端：外部观察浏览器上的交互

## 单元测试

descible：围绕测试单元组件测试用例: 可以是类、函数、组件等
it： 测试用例
mocha 没有断言库，选择 chai

## shallowMount 与 mount 区别

前者不会渲染子组件

## 组件交互

### 组件实例交互

```
import {shallowMount} from '@vue/test-utils';
import {expect} from 'chai';
import App from '../../src/App';

const wrapper = shallowMount(Movie);

describe('Movie component', () => {
    it('check default data', () =>{
        const defaultData = Movie.data();
        expect(defaultData.currentTabName).to.eql('Top250');
    })

    it('check change tab function', () => {
        wrapper.vm.changeTab("Top250");
        expect(wrapper.vm.baseUrl).to.eql("/ban/v2/movie/top250");
        wrapper.vm.changeTab("正在上映");
        expect(wrapper.vm.baseUrl).to.eql("/ban/v2/movie/in_theaters");
        wrapper.vm.changeTab("即将上映")
        expect(wrapper.vm.baseUrl).to.eql("/ban/v2/movie/coming_soon");
        wrapper.vm.changeTab("新片榜");
        expect(wrapper.vm.baseUrl).to.eql("/ban/v2/movie/new_movies");
        wrapper.vm.changeTab("口碑榜");
        expect(wrapper.vm.baseUrl).to.eql("/ban/v2/movie/weekly");
        wrapper.vm.changeTab("北美票房榜");
    })

    it('check changePageList', () =>{
        const wrapper = shallowMount(Movie);
        wrapper.vm.changePageList(2);
        expect(wrapper.vm.start).to.eql(2);
    })
})
```

### DOM 交互

```
it('should modify the text after clicking the button', () => {
  const wrapper = shallowMount(Footer);

  wrapper.find('button').trigger('click');
  const text = wrapper.find('.info').text();

  expect(text).to.eql('Modified by click');
});
```

### 父子组件交互

用 propsData 设置输入的 props，触发事件通过调用 emitted 方法，得到一个对象，key 是事件名，value 是事件参数数组

```
it('should handle interactions', () => {
  const wrapper = shallowMount(Footer, {
    propsData: { info: 'Click to modify' }
  });

  wrapper.vm.modify();

  expect(wrapper.vm.info).to.eql('Click to modify');
  expect(wrapper.emitted().modify).to.eql([
    ['Modified by click']
  ]);
});
```

## store 集成

```
import { expect } from 'chai';
import fetch from "node-fetch";
import Vue from "vue";
import { shallowMount, createLocalVue } from '@vue/test-utils'
import Index from '@/components/Index.vue';
import Vuex from 'vuex';
import { moduleA } from "../../src/store/index.js";

describe('Index.vue', () => {
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
```

## 路由

```
import {shallowMount, createLocalVue, mount} from '@vue/test-utils';
import {expect} from 'chai';
import App from '../../src/App';
import Movie from '../../src/components/Movie';
import MovieDetail from '../../src/components/MovieDetail';
import VueRouter from "vue-router";
const wrapper = shallowMount(Movie);

describe('Movie component', () => {

    it('check the detail, render child com via routing', () => {
        const localVue = createLocalVue();
        localVue.use(VueRouter);

        const router = new VueRouter({
            routes: [
                { path: '/movie', name: "Movie", component: Movie},
                { path: "/movie/:id", name: "MovieDetail", component:MovieDetail}
            ]
        });
        const wrapperRouter = mount(App, {localVue, router});
        router.push("/movie/1292052");
        expect(wrapperRouter.find(MovieDetail).exists()).to.be.not.ok;
    })
})
```
