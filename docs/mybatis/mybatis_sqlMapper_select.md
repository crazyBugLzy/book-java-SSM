# SELECT

官方文档参考：http://www.mybatis.org/mybatis-3/zh/sqlmap-xml.html#select

```
<select
  id="selectPerson"
  parameterType="int"
  resultType="hashmap"
  resultMap="personResultMap"
  flushCache="false"
  useCache="true"
  timeout="10000"
  fetchSize="256"
  statementType="PREPARED"
  resultSetType="FORWARD_ONLY"
  resultOrdered="true">
```

- ```id``` 最重要，是唯一标识
- ```parameterType``` 参数类型，可选一般可以自动识别
- ```resultType和resultMap``` 只能选择其一，需要必填，不然无法判断返回身噩梦
- ```flushCache/useCache``` 缓存相关，得考虑是否使用
- ```resultOrdered``` 这个设置仅针对嵌套结果 select 语句适用：如果为 true，就是假设包含了嵌套结果集或是分组了，这样的话当返回一个主结果行的时候，就不会发生有对前面结果集的引用的情况。这就使得在获取嵌套的结果集的时候不至于导致内存不够用。默认值：false。
