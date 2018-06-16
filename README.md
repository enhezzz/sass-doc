sass是css3语法的超集
1. 定义变量
$key: value,[...alternative value]
2. 导入外部文件
@import 指令
eg: @import 'example.scss','example2.scss'
以逗号作为分隔符
3. 注释
// 方式的注释在处理成css文件时会消失
/**/ 方式的注册则会保留到输出的css文件中
4. 嵌套语法
不仅支持类形式的嵌套，也支持属性的嵌套，但是要在简洁属性后加上冒号。比如：
```
    .main-sec{
        font-family: $font
        .headline {
            font: {
                family: $family;
                size: 18px;
            }
        }
    }
```
& ： 与

5. mixin
* 声明
@mixin name{
    //css规则
}
* 调用
@include mixinName()
eg:
```
@maxin col-6{
    width: 50%;
    float: left
}
.webdemo-sec{
    @include col-6()
}
```
当然了，也可以和其他样式规则混用
```
.webdemo-sec{
    @include col-6();
    &:hover{
        //背景暗灰常用色
        background-color: #f5f5f5
    }
}
```

maxin 支持传递参数
```
//这里给以$width默认值，在$width不存在的时候启用
@maxin col ($width[: 50%]){
    width: $width;
    float: left
}
```
支持一次include多个maxin，通过，分隔
6. extend
* 指令
@extend
* 用法
输入：
```
.error{
    color: #f00
}
.serious-error{
    @extend .error;
    border: 1px solid #f00
}
```

输出：
```
.error,serious-error{
    color: #f00
}
.serious-error{
    border: 1px solid #f00
}
```
##### 知识点：
    1. extend不可以继承选择器序列
    ```
    //错误的
    .a .b{

    }
    .c {
        @extend .a .b
    }
    ```

    2. 使用%，用来构建只用来继承的选择器
    输入：
    ```
    %error{
        color: #f00
    }
    .serious-error{
        @extend %error;
        border: 1px solid #f00
    }
    ```

    输出：
    ```
    .serious-error{
        color: #foo
    }
    .serious-error{
        border: 1px solid #f00
    }
    ```
7. @at-root
明确嵌套的内容输出到样式的顶层
输入
```
    .container{
        .piece{

        }
        @at-root{
            .container-wrap{

            }
        }
    }
```
输出：
```
    .container .piece{

    }
    container-wrap{

    }
```

这样做的好处是即通过嵌套规则表达出语义，又可以避免按需分配权重的问题


8. @media
sass中的@media跟css区别：
    sass中的media query可以内嵌在css规则中，在生成css的时候，media query才会被提到样式表的最高层级
好处：避免了重复书写选择器或者打乱样式表的流程