---
​---
layout: post
title:  "Study note of Maven"
date:   2023-10-20 23:25:05 +0800
categories: Maven Configuration
​---
---

# 学习笔记

### Maven

maven依赖传递，依赖冲突（解决两原则：1、看引用路径谁短，引用哪个版本；2、先声明的先引用，如  a-c2.0,f-c1.0,最终导入a  f  c2.0.

maven项目结构，

构建javaee工程

1、创建普通java工程，可以改变packaging为war,并在src/main目录下新增web项目结构文件webapp/WEB-INF/web.xml，刷新。

2、使用idea的javatoweb插件，自动转换为web项目。



版本提取可以写在pom.xml中的properties中，dependency中的version可以写在propertieis中声明的版本 ${变量}



maven的声明周期分为

**1、Clean 生命周期：**

- **clean**：删除目标目录中的编译输出文件。这通常是在构建之前执行的，以确保项目从一个干净的状态开始。

**2、Default 生命周期（也称为 Build 生命周期）：**

- **validate**：验证项目的正确性，例如检查项目的版本是否正确。
- **compile**：编译项目的源代码。
- **test**：运行项目的单元测试。
- **package**：将编译后的代码打包成可分发的格式，例如 JAR 或 WAR。
- **verify**：对项目进行额外的检查以确保质量。
- **install**：将项目的构建结果安装到本地 Maven 仓库中，以供其他项目使用。
- **deploy**：将项目的构建结果复制到远程仓库，以供其他开发人员或团队使用。

**3、Site 生命周期：**

- **site**：生成项目文档和站点信息。
- **deploy-site**：将生成的站点信息发布到远程服务器，以便共享项目文档。
- 打包命令：mvn clean package.

真正执行的是插件，周期-> 命令->插件。

------

插件版本不对，可以在pom.xml中定义插件的gav.

`<build>`
  `<plugins>`
    `**插件**gav-->`
`\*    <plugin>`
      `<groupId>org.apache.maven.plugins</groupId>`
      `<artifactId>maven-war-plugin</artifactId>`
      `<version>3.2.2</version>`
    `</plugin>`
  `</plugins>`
`</build>`

------

背景：一个大项目有多个模块，多个模块需要引用同版本的库，可以在父工程中为整个项目维护依赖信息的组合，保证了整个项目的使用规范、准确的jar包。

父工程中声明gavp，其中package为pom，Maven的三种项目打包方式——pom，jar，war。

**jar：工程的默认打包方式**，打包成jar用作jar包使用。存放一些其他工程都会使用的类，工具类。我们可以在其他工程的pom文件中去引用它

 **war**：将会打包成war，**发布在服务器上**，如网站或服务。用户可以通过浏览器直接访问，或者是通过发布服务被别的工程调用

 **pom**：用在**父级**工程或聚合工程中，用来做jar包的版本控制，**必须指明这个聚合工程的打包方式为pom**。 

子工程继承的gv和父工程一样，可以省略，只写a。

父工程中在 dependencyManagement中声明依赖的gav，子工程在dependency中导入依赖，若和父工程中gav一样，可以省略version。若写version则会覆盖父版本。NOTICE:父工程不会去下载依赖，只有子工程才会。

聚合语法：父项目中包含子项目的列表 ，通过触发父工程的构建，统一按顺序触发子工程构建的过程。在pom

作用：统一管理子项目构建；优化构建顺序，对多个项目进行顺序控制。
