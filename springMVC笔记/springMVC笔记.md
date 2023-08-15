## 问题1. Tomcat地狱bug，404
究极http 404问题
out文件夹的输出里面没有WEB-INFO文件夹，也没有index.jsp文件，没有jsp文件夹，查了好多资料后，仍未完全解决。

最终解决方案是，project structure，artifacts，在里面手动添加目录结构和文件，甚至是lib里的依赖包，总之要让这个out下面的目录结构和文件与web下面的完全相同。
![Alt text](image.png)

此时只能手动，我也不知道是idea版本的问题还是什么的问题，排除了好多可能性，依赖包的版本，环境变量的设置，Tomcat的设置，web文件夹的小蓝点是否存在，project stucture里面facets和artifacts的配置路径是否正确等等等等。

解决了！！！！！最终是换了一个idea的版本，2021.1.4的版本，一切都会自动输出和加载，不需要手动了。真他妈巨坑。

## Servlet、jsp、javabean在MVC中所扮演的角色及Tomcat

servlet是一种技术，也是一个java类，主要负责接受客户端服务器发出的HTTP请求，处理请求并生成响应，一般包括执行业务逻辑以及数据库操作，一般和javaBean协同，将javaBean作为数据模型，使用javaBean处理请求，然后再将结果生成响应，如转到html前端页面等，因此是一种动态web技术。一般在java程序中我们会写一个继承自HttpServlet类的类，重写里面的一些方法，如doGet，doPost等，以实现业务。

而Tomcat则是一个web服务器和servlet容器，它是用于托管和执行servlet，当浏览器发送请求到Tomcat时，Tomcat把请求传递到servlet进行处理，Tomcat管理着servlet的生命周期，请求的分发，线程管理，会话管理等等。

而jsp则是用于生成html和页面展示，它是一种所谓的模板引擎技术，可以在html页面中嵌入java代码，用于生成动态web内容，它的执行原理也是servlet类，客户端请求访问一个jsp页面时，jsp引擎会把jsp文件编译为一个servlet类，再在服务器上执行生成的servlet代码。

至于javaBean则是业务逻辑和数据操作所依赖的数据模型，它也是一个java类，用于数据封装和处理。

所以javaBean是Model，而jsp更像是Viewer的角色，视图页面，servlet则是Controller的角色，也就是一个调度，接受请求，把请求交给数据模型进行处理，然后再把结果返回给视图，由视图负责展示。

