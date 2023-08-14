## xml文件配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    
</beans>
```

## bug: mybatis核心配置文件里的environment
* 数据库的url最好不要开启useSSL,即useSSL=false，因为加密会让数据库的连接报错，无法连接