# 测试方式

```
public abstract class BaseTest {

    private String configPath = "mybatis.xml";
    private InputStream inputStream;
    private SqlSessionFactory sqlSessionFactory;
    private SqlSession sqlSession;

    //加载资源
    protected SqlSession loadResource() throws IOException {
        inputStream = Resources.getResourceAsStream(configPath);
        sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    //关闭资源
    protected void destory() throws IOException {
        if (inputStream == null) {
            inputStream.close();
        }
        if (sqlSession == null) {
            sqlSession.close();
        }
    }

}

public class UserDaoTest extends com.lzy.mybatis.BaseTest {

    @Test
    public void testListUser() {
        try {
            SqlSession sqlSession = loadResource();
            UserDao userDao = sqlSession.getMapper(UserDao.class);
            List<UserDO> users = userDao.listUser();
            System.out.println(users.toString());
            destory();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

}
```

