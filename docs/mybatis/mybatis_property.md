# Property

有三种方式，优先级如下：
1. 在 properties 元素体内指定的属性首先被读取。
1. 然后根据 properties 元素中的 resource 属性读取类路径下属性文件或根据 url 属性指定的路径读取属性文件，并覆盖已读取的同名属性。
1. 最后读取作为方法参数传递的属性，并覆盖已读取的同名属性。

同时可以占位符指定一个默认值，不过需要加入以下配置：

```
<properties resource="jdbc.properties">
    <!-- Enable this feature，为占位符指定一个默认值 -->
    <property name="org.apache.ibatis.parsing.PropertyParser.enable-default-value" value="true"/>
</properties>
```
