<!--
 * @Author: your name
 * @Date: 2020-02-08 14:19:38
 * @LastEditTime : 2020-02-08 14:39:10
 * @LastEditors  : Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /VoidBlog/docs/FrontEnd/z-index相关坑.md
 -->
# z-Index的坑

在处理遮罩的时候发现，如果
 - A:元素 (20000)
 - B:遮罩元素 （zIndex:10000）
    - C:遮罩元素内部的 messageBox DIV (30000)  


这种布局的话A 元素的层级是最高的。  
`A > C > B`


>因为 zIndex 本质上是一个相对概念。
