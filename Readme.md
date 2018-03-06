## Analytics-前端-组件: Tooltipster
##### 作者: *slaven*
## 用途说明
    显示toolTip
***
### 依赖

Jquery插件 [tooltipster](http://iamceege.github.io/tooltipster/)

***
### 文件结构、用途简述


***
### 入口
| 参数        | 类型           |    是否必须     |     默认值    | 作用 |
| :--------: |:-------------:| :----------:|:----------:|
| mountTipTargetSelector     | String | true  |   | 被hover显示tooltip的元素选择器 |
| tooltipTemplateSelector  | String | true  |   |  tooltip的内容模板选择器 |

***
### 注意点
1. tooltipster插件的很多功能还未搬迁，只是实现基本功能。后续版本应该依据需求陆续添加相应的功能
2. 组件使用了slot方式,即**<font color=red>模板需要作为Tooltipster.vue的内容</font>**。这样做的原因是该组件可与外部组件解耦，不需要在外部的组件里将模板内容的样式设置为不可见，因为该步骤已经在Tooltipster.vue中实现。

***
### example

***
### 更新日志
    1. 版本: v1
       更新时间: 2017.1.13
       作者: slaven
       更新内容: 创建组件
       说明:

    2. 版本: v2
       更新时间: 2017.6.22
       作者: slaven
       更新内容: 创建vue2组件
       说明:   

     3. 版本: v3
         更新时间: 2017.9.18
         作者: slaven
         更新内容: 升级为vue2.0组件
