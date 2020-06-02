---
title: 前端常用工具库
categories: 
  - 前端
tags:
  - 工具
  - API测试
---
<!--more-->
# postman后台API测试工具
设置全局环境以及全局变量，避免重复手动输入，引入全局环境和变量时
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200602092428912.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
新建
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200602092803609.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
管理集合
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200602092907964.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
进行API测试
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200602093312979.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
# moment时间格式化工具
[文档链接](https://segmentfault.com/a/1190000015240911)
 
## 获取时间、设置时间
Get Time
```
moment().year()/moment().get('year')
moment().month()/moment().get('month')    //0-11
moment().date()/moment().get('date')
moment().hours()/moment().get('hours')
moment().minutes()/moment().get('minutes')
moment().minutes()/seconds().get('seconds')

//获取一个星期中的某一天
moment().day()/moment().get('day')   //0-6 Sunday-Saturday
moment().weekday()/moment().get('weekday')   //0-6 Sunday-Saturday
moment().isoWeekday()/moment().get('isoWeekday')   //1-7 Monday-Sunday

//获取当前月总天数
moment().daysInMonth()

//获取时间戳  Timestamp
moment().format('X')   /   moment().unix()          //以s为单位
moment().format('x')   /   moment().valueOf()    //以ms为单位
```
Set Time
```
moment().year(Number)
moment().set('y', xxx)

//设置一个星期中的某一天
moment().weekday(number)/moment().set('weekday',xxxx)   //0-6 Sunday-Saturday
moment().isoWeekday(number)/moment().set('isoWeekday,xxx')   //1-7 Monday-Sunday
```
加减时间
```
moment().add(Number, String)   /    moment().add(Object)
moment().subtract(Number, String)   /    moment().subtract(Object)
```

## 常用格式化方法
```
moment().format()
moment().format(String)

string可为  'YYYY年MM月DD日'   'YYYY-MM-DD    HH时mm分ss秒   'YYYY年MM月DD日'（12小时制）
```
## 比较时间大小
```
export function startTimeThenCurrentTime(scheduleStartTime, currentTime){
    if(scheduleStartTime && !scheduleStartTime.isAfter(currentTime ,'minute') ){
        return true;
    }else{
        return false;
    }
}

export function endTimeThenCurrentTime(scheduleEndTime, currentTime){
    if(scheduleEndTime && !scheduleEndTime.isAfter(currentTime ,'minute')){
        return true;
    }else{
        return false;
    }
}

export function endTimeThenstartTime(scheduleStartTime, scheduleEndTime){
    if(scheduleEndTime && scheduleEndTime <= scheduleStartTime){
        return true;
    }else{
        return false;
    }
}
```
示例
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200602100627999.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200602100704486.png)
# lodash 模块化、高性能的js实用工具库
[文档链接](https://www.lodashjs.com/docs/latest)

## 常用的方法
```
_.forEach(collection, [iteratee=_.identity])
_.filter(collection, [predicate=_.identity])
_.find(collection, [predicate=_.identity], [fromIndex=0])
_.includes(collection, value, [fromIndex=0])
.map(collection, [iteratee=_.identity])
_.size(collection)
_.orderBy(collection, [iteratees=[_.identity]], [orders])
```

