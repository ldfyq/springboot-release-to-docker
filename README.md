# springboot-release-to-docker
springboot deploy to docker

一. 配置pom文件
```
  <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>1.2.0</version>
      <configuration>
          <imageName>${docker.image.prefix}/${project.name}</imageName>
          <dockerHost>http://127.0.0.1:2375</dockerHost>
          <dockerDirectory>src/main/docker</dockerDirectory>
          <resources>
              <resource>
                  <targetPath>/</targetPath>
                  <directory>${project.build.directory}</directory>
                  <include>${project.build.finalName}.jar</include>
              </resource>
          </resources>
      </configuration>
  </plugin>
  ```
 注意：
 imageName的内容注意一定要小写，docker支持大写
 dockerHost 主要没有地址不支持Https就不要用，否则会报unrecognized SSL message 
 
 二. 配置Dockerfile
 
 ```
FROM docker.io/cemmersb/centos-jdk8:latest
MAINTAINER figo
VOLUME /tmp
ADD springdemo-0.0.1-snapshot.jar app.jar
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

具体Dockerfile格式说明参考https://www.jianshu.com/p/cbce69c7a52f

三. 没有安装Maven就安装

安装后：执行指令 mvn clean package docker:build 就可以把镜像发布到本地的docker容器中

四. 运行镜像

docker run -d -p 8999:8999 --name sayHello myproject/springdemo
