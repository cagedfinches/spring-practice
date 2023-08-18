## 问题1. Tomcat地狱bug，404
究极http 404问题
out文件夹的输出里面没有WEB-INFO文件夹，也没有index.jsp文件，没有jsp文件夹，查了好多资料后，仍未完全解决。

最终解决方案是，project structure，artifacts，在里面手动添加目录结构和文件，甚至是lib里的依赖包，总之要让这个out下面的目录结构和文件与web下面的完全相同。
![Alt text](image.png)

此时只能手动，我也不知道是idea版本的问题还是什么的问题，排除了好多可能性，依赖包的版本，环境变量的设置，Tomcat的设置，web文件夹的小蓝点是否存在，project stucture里面facets和artifacts的配置路径是否正确等等等等。

解决了！！！！！最终是换了一个idea的版本，2021.1.4的版本，一切都会自动输出和加载，不需要手动了。真他妈巨坑。

后来写的时候又遇到过404，还是idea的问题，需要手动在out里面加入lib依赖。

## Servlet、jsp、javabean在MVC中所扮演的角色及Tomcat

servlet是一种技术，也是一个java类，主要负责接受客户端服务器发出的HTTP请求，处理请求并生成响应，一般包括执行业务逻辑以及数据库操作，一般和javaBean协同，将javaBean作为数据模型，使用javaBean处理请求，然后再将结果生成响应，如转到html前端页面等，因此是一种动态web技术。一般在java程序中我们会写一个继承自HttpServlet类的类，重写里面的一些方法，如doGet，doPost等，以实现业务。

而Tomcat则是一个web服务器和servlet容器，它是用于托管和执行servlet，当浏览器发送请求到Tomcat时，Tomcat把请求传递到servlet进行处理，Tomcat管理着servlet的生命周期，请求的分发，线程管理，会话管理等等。

而jsp则是用于生成html和页面展示，它是一种所谓的模板引擎技术，可以在html页面中嵌入java代码，用于生成动态web内容，它的执行原理也是servlet类，客户端请求访问一个jsp页面时，jsp引擎会把jsp文件编译为一个servlet类，再在服务器上执行生成的servlet代码。

至于javaBean则是业务逻辑和数据操作所依赖的数据模型，它也是一个java类，用于数据封装和处理。

所以javaBean是Model，而jsp更像是Viewer的角色，视图页面，servlet则是Controller的角色，也就是一个调度，接受请求，把请求交给数据模型进行处理，然后再把结果返回给视图，由视图负责展示。

## springMVC的执行原理及流程

首先，DispatcherServlet是整个springMVC的控制中心，负责分发请求到不同的controller，他接收http请求，通过HandlerMapping，这是一种映射，通过这种映射可以找到具体执行的controller控制器，而这个controller要做的就是处理请求，调用业务层进行业务和数据操作，最后返回一个ModelAndView，模型和视图，返回给DispatchServlet，DispatchServlet调用ViewResolver视图解析器，找到具体的视图名字，最终渲染出视图。
![Alt text](image-1.png)
![Alt text](image-2.png)

## web.xml里需要注意

servlet-mapping里面的url-pattern指的是我们要去处理的http请求，例如http://localhost:8080，后再加http://localhost:8080/hello，这里这个hello就是一个新的请求。而下面的/指的是我们要处理/后的所有请求。如果下面写的是/hello，那么指的是要处理/hello之后的所有请求才会走servlet。
```xml
<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

另外，/表示只匹配所有的请求，不会去匹配jsp页面，而/*则表示匹配所有的请求，包括jsp页面。但由于我们将会用viewResolver视图解析器来拼接请求页面的名字，因此如果客户端要请求http://localhost:8080/a.jsp，那么a.jsp页面会在拼接的时候出问题。

## maven静态资源过滤问题

[mybatis里需要把java下的xml文件给过滤掉](https://blog.csdn.net/SHILIKNG/article/details/116983128?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-2-116983128-blog-117930947.235%5Ev38%5Epc_relevant_anti_vip&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-2-116983128-blog-117930947.235%5Ev38%5Epc_relevant_anti_vip&utm_relevant_index=1)

## spring框架中的database.properties

在 Spring 框架中，database.properties 文件通常是用于存储数据库连接相关的配置信息的属性文件。这些配置信息包括数据库的连接URL、用户名、密码、驱动程序类名等。通过将这些配置信息存储在属性文件中，可以实现配置的分离，便于在不同环境下进行配置切换和管理。

具体来说，database.properties 文件的作用包括：

配置数据库连接信息： 在实际应用中，连接数据库需要提供一些关键信息，如数据库连接URL、用户名、密码、数据库驱动程序类名等。将这些敏感信息存储在属性文件中可以避免将其硬编码在代码中，提高了安全性和可维护性。

分离配置： 使用属性文件可以将数据库连接的配置与代码逻辑分离开来。这意味着你可以在不同的环境中（如开发、测试、生产）使用不同的属性文件，而不需要修改应用程序的代码。

简化配置： 在 Spring 中，你可以使用 PropertyPlaceholderConfigurer 或 PropertySourcesPlaceholderConfigurer 等配置类来加载属性文件中的配置信息，并将其注入到 Spring 的配置文件或 Java Bean 中，从而使得配置变得更加简洁和可读。

提供配置切换： 通过使用不同的属性文件，你可以实现在不同的环境下切换配置。例如，在开发环境下使用一个属性文件，而在生产环境下使用另一个属性文件，从而避免了手动修改配置的麻烦。

示例 database.properties 文件内容：

```
properties
Copy code
# Database Configuration
db.url=jdbc:mysql://localhost:3306/mydb
db.username=root
db.password=secret
db.driver=com.mysql.jdbc.Driver

```
在 Spring 配置文件中加载属性文件：

```xml
<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
    <property name="locations">
        <list>
            <value>classpath:database.properties</value>
        </list>
    </property>
</bean>
```
然后可以在 Spring 配置文件或 Java Bean 中使用 ${} 表达式来引用属性值，例如：

```xml
Copy code
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
    <property name="url" value="${db.url}" />
    <property name="username" value="${db.username}" />
    <property name="password" value="${db.password}" />
    <property name="driverClassName" value="${db.driver}" />
</bean>
```

## spring里的数据库连接池

在 Spring 框架中，数据库连接池（Database Connection Pool）是用于管理和维护数据库连接的一种技术，它的主要作用是提高数据库访问性能、资源利用率和应用程序的响应时间。数据库连接池通过预先创建一组数据库连接，并在需要时将连接提供给应用程序，从而减少了连接创建和释放的开销。

以下是数据库连接池的一些主要作用：

提高性能： 数据库连接的创建和释放是一个相对昂贵的操作，涉及网络通信和数据库资源的分配。连接池可以在应用程序启动时创建一组连接，并将这些连接保持在池中，供应用程序随时使用。这减少了每次请求都需要创建新连接的开销，从而提高了数据库访问性能。

资源利用率： 数据库连接是有限资源，不适当地创建过多的连接可能会导致资源浪费或数据库服务器过载。连接池可以管理连接的数量，并确保连接的合理利用，避免过多的连接占用资源。

连接复用： 连接池可以对连接进行复用，即当一个连接被释放后，它可以被另一个请求重新使用，从而减少了连接的开销和资源消耗。

连接管理： 连接池可以监控连接的状态，并在连接超时、失效或发生异常时自动重新创建或释放连接，从而确保连接的稳定性和可靠性。

减少连接泄漏： 使用连接池可以减少连接泄漏的风险。如果应用程序不正确地释放连接，可能会导致数据库连接泄漏，最终导致数据库资源不足。连接池可以自动管理连接的生命周期，避免这种问题。

配置和管理： 数据库连接池可以根据应用程序的需求进行配置，包括最大连接数、最小连接数、连接超时等。这使得连接池可以根据应用程序的负载和需求进行动态调整。

Spring 框架本身并不包含数据库连接池的实现，但它提供了对多个第三方连接池的集成支持，例如 Apache Commons DBCP、HikariCP、Tomcat JDBC 等。通过配置 Spring 数据源和连接池，你可以在 Spring 应用程序中轻松地使用数据库连接池来管理数据库连接，提高性能和可伸缩性。

## ssm整合：spring层

这里涉及到mybatis-spring的一些使用，在这里新添加了一种动态地将Dao接口注入到Spring的一种方式，利用MapperScannerConfigurer类，MapperScannerConfigurer 是 Spring 框架中用于扫描 MyBatis 映射器（Mapper）接口的配置类。它的作用是自动扫描指定的包路径，将符合条件的 MyBatis 映射器接口注册为 Spring 的 Bean，从而将这些映射器接口交由 Spring 管理。

在此之前的老方式，是创建一个mapper接口的实现类，要么把sqlSessionTemplate作为这个类的一个私有属性，然后在Spring配置文件中利用sqlSessionTemplate来注入sqlSession。要么让这个实现类继承sqlSessionDaoSupport，那么就可以直接利用sqlSessionFactory来注入。无论如何都需要另外写这样一个实现类。