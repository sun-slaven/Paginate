## Paginate
##### 作者: *slaven*
## 用途说明
    1. 分页组件
    2. 兼具页码选择,跳页，左右页选择，开头、结尾页选择等功能。
***
### 文件结构、用途简述
   > Paginate.vue

***
### 入口
| 参数        | 类型           |    是否必须     |     默认值    | 作用 |
| :--------: |:-------------:| :----------:|:----------:|
| maxPage     | Number | false  | 100  | 总页数|
| continousNumber  | Number | true  |  10 |  连续页数值 ，不包括可能出现的左边的1或右边的最后一页数|
| currentPage | Number |  true |  1 | 当前页码|
| showEllipsis  | Boolean | false | true | 开关，是否显示page条左右两边的 ... 标志以及首尾页|
| showPageInput | Boolean  |  false   |  true |开关，是否显示填入page的input框和Go按钮 |
| choosePageCallback   | Function | true | |回调函数，选择页面时触发|
| showFirstPage   | Boolean | false | true |开关，是否显示"..."前的第一页|
| showLastPage   | Boolean | false | true |开关，是否显示"..."后的最后一页|
| canClickEllipsis   | Boolean | false | true |开关，是否可以点击ellipsis|
### 注意点
    canClickEllipsis打开时必须确保showEllipsis打开
***
### example
    参考example目录下的示例
***
### 更新日志
    1. 版本: v1
       更新时间: 2017.1.19
       作者: slaven
       更新内容: 创建组件
       说明:

    2. 版本: v1.1
        更新时间: 2017.1.20
        作者: slaven
        更新内容: 添加showFirstPage、showLastPage、canClickEllipsis等开关

    3. 版本: v1.2
        更新时间: 2017.2.23
        作者: slaven
        更新内容: 修改页码很少的情况下显示的问题

    4. 版本: v2.0
        更新时间: 2017.9.18
        作者: slaven
        更新内容: 升级为vue2.0组件
