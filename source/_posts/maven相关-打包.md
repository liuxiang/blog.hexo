title: maven相关-打包
date: 2017-4-6 00:00:00
tags: [ maven ]


---


# 环境配置
`path` F:\Tool\Maven\apache-maven-3.3.9\bin
- 依赖`JAVA_HOME` C:\Program Files\Java\jdk1.8.0_91


# 打包`war(java web项目)`与`jar(java项目)`，`ear`
- 设置打包类型
```
<packaging>war</packaging>

```


## 方式一：`maven-jar-plugin`+`maven-dependency-plugin`
```
<build>
    <outputDirectory>${project.basedir}/src/main/webapp/WEB-INF/classes/</outputDirectory>
    <plugins>
        <!-- Compiler 插件, 设定JDK版本 -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.5.1</version>
            <configuration>
                <source>${jdk.version}</source>
                <target>${jdk.version}</target>
                <showWarnings>true</showWarnings>
            </configuration>
        </plugin>


        <!-- 打包jar文件时，配置manifest文件，加入lib包的jar依赖 -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jar-plugin</artifactId>
            <version>2.4</version>
            <configuration>
                <!--<encoding>${project.build.sourceEncoding}</encoding>-->
            </configuration>
            <executions>
                <execution>
                    <phase>prepare-package</phase>
                    <goals>
                        <goal>jar</goal>
                    </goals>
                    <configuration>
                        <classesDirectory>${project.build.outputDirectory}</classesDirectory>
                        <finalName>bright</finalName>
                        <outputDirectory>${project.build.directory}/${project.artifactId}/WEB-INF/lib
                        </outputDirectory>
                        <includes>
                            <!--<include>com/thinkgem/jeesite/**</include>--> <!-- 参考项目 -->
                            <include>com/wosai/bright/**</include>
                        </includes>
                    </configuration>
                </execution>
            </executions>
        </plugin>


        <!-- dependency插件 -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-dependency-plugin</artifactId>
            <version>2.10</version>
        </plugin>
    </plugins>
</build>
```
maven-dependency-plugin插件用于添加依赖包


## 方式二：`maven-assembly-plugin`插件打包
```
<build>  
    <plugins>  
  
        <plugin>  
            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-assembly-plugin</artifactId>  
            <version>2.5.5</version>  
            <configuration>  
                <archive>  
                    <manifest>  
                        <mainClass>com.xxg.Main</mainClass>  
                    </manifest>  
                </archive>  
                <descriptorRefs>  
                    <descriptorRef>jar-with-dependencies</descriptorRef>  
                </descriptorRefs>  
            </configuration>  
        </plugin>  
  
    </plugins>  
</build>  
```
运行
`mvn package assembly:single`

可以直接通过mvn package来打包，无需assembly:single，不过需要加上一些配置：

```
<build>  
    <plugins>  
  
        <plugin>  
            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-assembly-plugin</artifactId>  
            <version>2.5.5</version>  
            <configuration>  
                <archive>  
                    <manifest>  
                        <mainClass>com.xxg.Main</mainClass>  
                    </manifest>  
                </archive>  
                <descriptorRefs>  
                    <descriptorRef>jar-with-dependencies</descriptorRef>  
                </descriptorRefs>  
            </configuration>  
            <executions>  
                <execution>  
                    <id>make-assembly</id>  
                    <phase>package</phase>  
                    <goals>  
                        <goal>single</goal>  
                    </goals>  
                </execution>  
            </executions>  
        </plugin>  
  
    </plugins>  
</build>  
```
其中<phase>package</phase>、<goal>single</goal>即表示在执行package打包时，执行assembly:single，所以可以直接使用mvn package打包



## 方式三：`maven-shade-plugin`插件打包
```
<build>  
    <plugins>  
  
        <plugin>  
            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-shade-plugin</artifactId>  
            <version>2.4.1</version>  
            <executions>  
                <execution>  
                    <phase>package</phase>  
                    <goals>  
                        <goal>shade</goal>  
                    </goals>  
                    <configuration>  
                        <transformers>  
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">  
                                <mainClass>com.xxg.Main</mainClass>  
                            </transformer>  
                        </transformers>  
                    </configuration>  
                </execution>  
            </executions>  
        </plugin>  
  
    </plugins>  
</build> 
```
如果项目中用到了spring Framework，将依赖打到一个jar包中，运行时会出现读取XML schema文件出错。原因是Spring Framework的多个jar包中包含相同的文件spring.handlers和spring.schemas，如果生成一个jar包会互相覆盖。为了避免互相影响，可以使用AppendingTransformer来对文件内容追加合并：

`详见： Maven打包的三种方式 - daiyutage的专栏 - 博客频道 - CSDN.NET `


---
`Maven打包的三种方式 - daiyutage的专栏 - 博客频道 - CSDN.NET`

http://blog.csdn.net/daiyutage/article/details/53739452


`利用maven在一个项目中同时打war包和jar包 - apexlj的博客 - 博客频道 - CSDN.NET`
http://blog.csdn.net/apexlj/article/details/47019663
