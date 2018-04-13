# JAVA API


通过SqlSessionFactoryBuilder来创建SessionFactory,然后用SessionFactoryOpenSQLSession。SQLSession可以获取映射类——```sqlSession.getMapper(UserDao.class)```。SqlSession要确认被关闭。

- SqlSessions
- SqlSessionFactoryBuilder
- SqlSessionFactory