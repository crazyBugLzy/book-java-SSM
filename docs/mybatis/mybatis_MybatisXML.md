# 核心配置文件

从XML配置的角度来讲，主要就几个方面：
1. 全局配置，大部分的默认配置就OK(可选)
2. 配置Mapper，一共有4种方式。不管哪种方式都有一些限制，如果说能把配置文件和映射类放在同一个目录，那么使用方式4比较好。(必选)
3. properties(可选)

**参考下面这个配置文件就已经满足95%的场景了，所以没必要细节的看下属章节。**


```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">


<configuration>

    <properties resource="jdbc.properties">
        <!--
        可以添加额外的配置
        1.在 properties 元素体内指定的属性首先被读取。
        2.然后根据 properties 元素中的 resource 属性读取类路径下属性文件或根据 url 属性指定的路径读取属性文件，并覆盖已读取的同名属性。
        3.最后读取作为方法参数传递的属性，并覆盖已读取的同名属性
        即通过方法参数传递的属性具有最高优先级，resource/url 属性中指定的配置文件次之，最低优先级的是 properties 属性中指定的属性
        -->
        <!-- 如果配置文件中有username和password，下面的参数会被覆盖 -->
       <!-- <property name="username" value="dev_user"/>
        <property name="password" value="F2Fa3!33TYyg"/>-->
        <!-- Enable this feature，为占位符指定一个默认值 -->
        <property name="org.apache.ibatis.parsing.PropertyParser.enable-default-value" value="true"/>
    </properties>

    <settings>
        <!-- 该配置影响的所有映射器中配置的缓存的全局开关 -->
        <setting name="cacheEnabled" value="true"/>
        <!--延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置fetchType属性来覆盖该项的开关状态 -->
        <setting name="lazyLoadingEnabled" value="false"/>
        <!-- 当开启时，任何方法的调用都会加载该对象的所有属性。否则，每个属性会按需加载（参考lazyLoadTriggerMethods). -->
        <setting name="aggressiveLazyLoading" value="false"/>
        <!--是否允许单一语句返回多结果集（需要兼容驱动）。-->
        <setting name="multipleResultSetsEnabled" value="true"/>
        <!-- 使用列标签代替列名。不同的驱动在这方面会有不同的表现， 具体可参考相关驱动文档或通过测试这两种不同的模式来观察所用驱动的结果。 -->
        <setting name="useColumnLabel" value="true"/>
        <!-- 允许 JDBC 支持自动生成主键，需要驱动兼容。 如果设置为 true 则这个设置强制使用自动生成主键，尽管一些驱动不能兼容但仍可正常工作
        （比如 Derby）-->
        <setting name="useGeneratedKeys" value="false"/>
        <!--指定 MyBatis 应如何自动映射列到字段或属性。 NONE 表示取消自动映射；PARTIAL 只会自动映射没有定义嵌套结果集映射的结果集。
        FULL会自动映射任意复杂的结果集（无论是否嵌套）。-->
        <setting name="autoMappingBehavior" value="PARTIAL"/>
        <!--默认值：NONE
         指定发现自动映射目标未知列（或者未知属性类型）的行为。
         NONE: 不做任何反应
         WARNING: 输出提醒日志 ('org.apache.ibatis.session.AutoMappingUnknownColumnBehavior' 的日志等级必须设置为 WARN)
         FAILING: 映射失败 (抛出 SqlSessionException)
        -->
        <setting name="autoMappingUnknownColumnBehavior" value="NONE"/>
        <!-- 配置默认的执行器。
        SIMPLE 就是普通的执行器；REUSE 执行器会重用预处理语句（prepared statements）； BATCH 执行器将重用语句并执行批量更新 -->
        <setting name="defaultExecutorType" value="SIMPLE"/>
        <!--默认不设置 设置超时时间，它决定驱动等待数据库响应的秒数 -->
        <!--<setting name="defaultStatementTimeout" value="25"/>-->
        <!--默认不设置 为驱动的结果集获取数量（fetchSize）设置一个提示值。此参数只可以在查询设置中被覆盖。 -->
        <!-- <setting name="defaultFetchSize" value="100"/>-->
        <setting name="safeRowBoundsEnabled" value="false"/>
        <!-- 是否开启自动驼峰命名规则（camel case）映射，即从经典数据库列名 A_COLUMN 到经典 Java 属性名 aColumn 的类似映射。 -->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <setting name="localCacheScope" value="SESSION"/>
        <setting name="jdbcTypeForNull" value="OTHER"/>
        <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
    </settings>

    <!--
   每一个在包 com.lzy.mybatis 中的 Java Bean
   在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名
   可以加@Alias("UserDO")的注解
   -->
    <typeAliases>
        <!--<typeAlias alias="UserDO" type="com.lzy.mybatis.UserDO"/>-->
        <package name="com.lzy.mybatis"/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <!-- 上方有开启占位符默认值的特性，如果jdbc.username不存在，则会使用默认值 -->
                <property name="username" value="${jdbc.username:root}"/>
                <property name="password" value="${jdbc.password:123}"/>
                <property name="poolMaximumActiveConnections" value="15"/>
                <property name="poolMaximumIdleConnections" value="2"/>
                <property name="poolMaximumCheckoutTime" value="20000"/>
                <property name="poolMaximumLocalBadConnectionTolerance" value="3"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <!-- 方式1：通过XML文件加载 -->
        <mapper resource="com/lzy/mybatis/dao/UserMapper.xml"/>
        <mapper resource="com/lzy/mybatis/dao/DepartmentMapper.xml"/>
        <!-- 方式2：通过完全限定资源定位符 加载 -->
        <!--<mapper url="file:///D:/develop/1.IDE/workspace/testJdbc/src/main/resources/com/lzy/mybatis/UserMapper.xml"/>-->
        <!-- 方式3：通过扫描映射类的方式加载 -->
        <!--<mapper class="org.mybatis.builder.AuthorMapper"/>-->
        <!-- 方式4：通过扫描包的方式来加载，需要把配置文件和映射类放在同一个目录 -->
        <!--<package name="com.lzy.mybatis.dao"/>-->
    </mappers>


</configuration>
```
