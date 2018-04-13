# 静态SQL

1. SQL 代码段

```
<sql id="sometable">
  ${prefix}Table
</sql>

<sql id="someinclude">
  from
    <include refid="${include_target}"/>
</sql>

<select id="select" resultType="map">
  select
    field1, field2, field3
  <include refid="someinclude">
    <property name="prefix" value="Some"/>
    <property name="include_target" value="sometable"/>
  </include>
</select>
```
2. 绑定参数 
- ```#{age,javaType=int,jdbcType=NUMERIC,typeHandler=MyTypeHandler}```
3. RESULT MAP 映射
-   结果集支持继承,extends="xxx"
-   结果集也考虑自动映射的方式，自动驼峰转化，比较清晰点
-   高级映射和嵌套映射在分域查询中进行的比较少，因为大部分表分库分表后，无法连表查询。
4. 查询语句
- SELECT XXX : 此部分根据需要，选择是直接写在代码中还是通过SQL嵌入。如果多个地方都使用到，建议写在SQL里面
- FROM TABLE：很多地方都用到，建议把这部分放在<sql></sql>
- WHERE ：动态查询则参考动态SQL
- RESULT : 具体类型或者Result set，具体类型如果是实体，全路径引用或者别名
    - resultType：如果列名和属性名没有精准匹配，可以使用SELECT AS 别名 
- 根据结果情况返回不同的结果集：==discriminator==

```
<resultMap id="vehicleResult" type="Vehicle">
  <id property="id" column="id" />
  <result property="vin" column="vin"/>
  <result property="year" column="year"/>
  <result property="make" column="make"/>
  <result property="model" column="model"/>
  <result property="color" column="color"/>
  <discriminator javaType="int" column="vehicle_type">
    <case value="1" resultMap="carResult"/>
    <case value="2" resultMap="truckResult"/>
    <case value="3" resultMap="vanResult"/>
    <case value="4" resultMap="suvResult"/>
  </discriminator>
</resultMap>
```

