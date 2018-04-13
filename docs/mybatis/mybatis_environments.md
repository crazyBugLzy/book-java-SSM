# 配置环境 environments

1. 每个数据库对应一个 SqlSessionFactory 实例
2. 环境XML配置看上面的配置
3. 数据源（dataSource）
    - UNPOOLED
    - POOLED
    - JNDI

**UNPOOLED**– 这个数据源的实现只是每次被请求时打开和关闭连接。虽然有点慢，但对于在数据库连接可用性方面没有太高要求的简单应用程序来说，是一个很好的选择。 不同的数据库在性能方面的表现也是不一样的，对于某些数据库来说，使用连接池并不重要，这个配置就很适合这种情形。UNPOOLED 类型的数据源仅仅需要配置以下 5 种属性：
 
 - ```driver``` – 这是 JDBC 驱动的 Java 类的完全限定名（并不是 JDBC 驱动中可能包含的数据源类）。
 - ```url``` – 这是数据库的 JDBC URL 地址。
 - ```username``` – 登录数据库的用户名。
 - ```password``` – 登录数据库的密码。  
 - ```defaultTransactionIsolationLevel``` – 默认的连接事务隔离级别。
 作为可选项，你也可以传递属性给数据库驱动。要这样做，属性的前缀为“driver.”，例如：
 
 - driver.encoding=UTF8
 这将通过 DriverManager.getConnection(url,driverProperties) 方法传递值为 UTF8 的 encoding 属性给数据库驱动。
 
**POOLED**– 这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来，避免了创建新的连接实例时所必需的初始化和认证时间。 这是一种使得并发 Web 应用快速响应请求的流行处理方式。
 
 除了上述提到 UNPOOLED 下的属性外，还有更多属性用来配置 POOLED 的数据源：
 
 - ```poolMaximumActiveConnections``` – 在任意时间可以存在的活动（也就是正在使用）连接数量，默认值：10
 - ```poolMaximumIdleConnections``` – 任意时间可能存在的空闲连接数。
 - poolMaximumCheckoutTime – 在被强制返回之前，池中连接被检出（checked out）时间，默认值：20000 毫秒（即 20 秒）
 - ```poolTimeToWait``` – 这是一个底层设置，如果获取连接花费了相当长的时间，连接池会打印状态日志并重新尝试获取一个连接（避免在误配置的情况下一直安静的失败），默认值：20000 毫秒（即 20 秒）。
 - ```poolMaximumLocalBadConnectionTolerance``` – 这是一个关于坏连接容忍度的底层设置，作用于每一个尝试从缓存池获取连接的线程. 如果这个线程获取到的是一个坏的连接，那么这个数据源允许这个线程尝试重新获取一个新的连接，但是这个重新尝试的次数不应该超过 poolMaximumIdleConnections 与 poolMaximumLocalBadConnectionTolerance 之和。 默认值：3 (新增于 3.4.5)
 - ```poolPingQuery``- 发送到数据库的侦测查询，用来检验连接是否正常工作并准备接受请求。默认是“NO PING QUERY SET”，这会导致多数数据库驱动失败时带有一个恰当的错误消息。
 - ```poolPingEnabled``` – 是否启用侦测查询。若开启，需要设置 poolPingQuery 属性为一个可执行的 SQL 语句（最好是一个速度非常快的 SQL 语句），默认值：false。
 - ```poolPingConnectionsNotUsedFor``` – 配置 poolPingQuery 的频率。可以被设置为和数据库连接超时时间一样，来避免不必要的侦测，默认值：0（即所有连接每一时刻都被侦测 — 当然仅当 poolPingEnabled 为 true 时适用）。
 
**JNDI** – 这个数据源的实现是为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的引用。这种数据源配置只需要两个属性：
 
 - initial_context – 这个属性用来在 InitialContext 中寻找上下文（即，initialContext.lookup(initial_context)）。这是个可选属性，如果忽略，那么 data_source 属性将会直接从 InitialContext 中寻找。
 - data_source – 这是引用数据源实例位置的上下文的路径。提供了 initial_context 配置时会在其返回的上下文中进行查找，没有提供时则直接在 InitialContext 中查找。
 和其他数据源配置类似，可以通过添加前缀“env.”直接把属性传递给初始上下文。比如：
 - env.encoding=UTF8

