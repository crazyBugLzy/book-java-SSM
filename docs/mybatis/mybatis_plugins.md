# 插件 plugins

MyBatis 允许你在已映射语句执行过程中的某一点进行拦截调用。默认情况下，MyBatis 允许使用插件来拦截的方法调用包括：

1. Executor (update, query, flushStatements, commit, rollback, getTransaction, close, isClosed)
1. ParameterHandler (getParameterObject, setParameters)
1. ResultSetHandler (handleResultSets, handleOutputParameters)
1. StatementHandler (prepare, parameterize, batch, update, query)
