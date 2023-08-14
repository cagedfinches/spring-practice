## 问题1. Tomcat地狱bug，404
究极http 404问题
out文件夹的输出里面没有WEB-INFO文件夹，也没有index.jsp文件，没有jsp文件夹，查了好多资料后，仍未完全解决。

最终解决方案是，project structure，artifacts，在里面手动添加目录结构和文件，甚至是lib里的依赖包，总之要让这个out下面的目录结构和文件与web下面的完全相同。
![Alt text](image.png)

此时只能手动，我也不知道是idea版本的问题还是什么的问题，排除了好多可能性，依赖包的版本，环境变量的设置，Tomcat的设置，web文件夹的小蓝点是否存在，project stucture里面facets和artifacts的配置路径是否正确等等等等。