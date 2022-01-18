# Mybatis

## 介绍

### 框架

> 把重复的代码工作抽取出来，让程序员把精力专注在核心的业务代码实现上
>
> 框架可以理解为半成品软件，框架做好以后，接下来在它基础上进行开发

### Mybatis

> Mybatis是一款优秀的持久层框架，它底层封装的是JDBC
>
> 使用Mybatis之后，程序员就不再需要想JDBC那样去写复杂代码来设置参数、处理结果集等
>
> 而是采用简单的XML配置+接口方法的形式实现对数据库的增删改查操作，可以让程序员只关注sql本身

### API

```markdown
* Resources
		读取配置文件
		InputStream inputStream = Resources.getResourceAsStream("SqlMapConfig.xml")
	
* SqlSessionFactoryBuilder
		用于创建SqlSessionFactory
		SqlSessionFactory sqlSessonFactory = new SqlSessionFactoryBuilder().build(inputStream);
		
* SqlSessionFactory
		生命周期: 项目创建,他就创建;项目停止,他就销毁
		SqlSession sqlSession = sqlSessionFactory.openSession();
		用于获取SqlSession
			
* SqlSession
		生命周期: 用的时候就创建,用完就销毁
		UserDao userDao = sqlSession.getMapper(UserDao.class);
		获取代理对象
```



# 接口代理开发

## 约定

> 在Mapper接口文件和映射文件之间
>
> ​	1.映射文件的namespace的值等于接口的全限定名
>
> ​	2.映射文件中的statementld的值等于接口中的方法名

![image-20211129160612381](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-20211129160612381.png)

## Mybatis编程三步骤

	1. 需要在接口中声明一个方法

![image-20211129161445567](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-20211129161445567.png)

2. 需要在映射文件中添加一条sql

![image-20211129161736104](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-20211129161736104.png)

2. 在测试类中进行新方法测试

![image-20211129161755176](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-20211129161755176.png)

> 以此类推增删改，只需要修改映射中的sql语句+对应的方法
>
> 查询有另外的语句



# 查询

> parameterType   传输指定参数对象  （可以省略不写）

## resultType的使用

> 当数据库返回的结果集中的字段和实体类中的属性名一一对应时，resyltType可以自动将结果封装到实体中

![image-20211129170426181](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-20211129170426181.png)

## resultMap的使用

> 当数据库返回的结果集中的字段和实体类中的属性名存在不对应情况时，可以使用resultMap自定义映射关系

![image-20211129171917437](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-20211129171917437.png)

## 一个条件的查询

> 输入类型：Integer
>
> 返回类型：对象
>
> 根据uid查询

### 映射

![image-20211129172354366](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-20211129172354366.png)

## 多个条件的查询

> 

### 方式一

#### 接口

> @Param 注解
>
> 可以给参数命名，随后在映射里使用

![image-20211129173213388](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-20211129173213388.png)

#### 映射

![image-2021112917322571](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-2021112917322571.png)

### 方式二

> 传递参数多的时候，可以使用一个对象来封装

#### 接口

![image-20211129173456320](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-20211129173456320.png)

#### 映射

![image-20211129173507670](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-20211129173507670.png)

## 模糊查询

> select * from user where name like "%${value}%"
>
> 根据value模糊查询user表中的name

### 接口

![image-20211201083739831](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-20211201083739831.png)

### 映射

![image-20211201084539175](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-20211201084539175.png)

![image-20211201084935362](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-20211201084935362.png)

### 测试

![image-20211201084549615](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-20211201084549615.png)

> 面试题：# 和 $ 的区别
>
> ```markdown
> 1. # 表示占位符，相当于JDBC中的?，底层工作的是PreparedStetement对象，SQL只编译一次，而且没有SQL注入问题
> 2. # 当传入的参数为一个简单类型时，#{ }可以随便写
> 
> 1. $ 表示字符串拼接，底层工作的是Statement对象，每次都会重新编译，而且存在SQL注入问题
> 2. $ 当传入的参数为一个简单类型时，${ }只能写value
> ```



# 返回主键

> 向数据库保存一个用户后，然后再控制台记录下此用户的主键值(id)
>
> useGeneratedKeys：告诉mybatis我们需要返回生成的主键值，让mybatis返回
>
> keyProperty：将返回来的主键的值赋值到传入参数的指定属性上

### 接口

![image-20211201085817408](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-20211201085817408.png)

### 映射

![image-20211201085747939](https://gitee.com/feiyuroot/blogimage/raw/master/img/image-20211201085747939.png)

