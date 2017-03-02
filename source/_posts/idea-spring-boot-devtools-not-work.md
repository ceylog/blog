---
title: idea-spring-boot-devtools-not-work
date: 2017-03-02 13:25:37
tags: [spring-boot,idea,devtools]
category: devtools
---

java开发中总是会频繁重启应用，非常麻烦，`Spring Boot`可以利用`devtools`配置当修改代码时自动重启应用，也可以使用热部署工具`jrebel`，但是在idea下默认不生效，google后得到答案，idea不会自动编译程序，开启自动编译后发现，在应用运行中也不会自动编译，找到问题原因了，下面来解决它

## 设置idea 自动编译
Settings..
![开启自动编译](/images/intellij_idea/idea_auto_compile.png)

## 设置idea 在程序运行中自动编译
快捷键 ctrl + alt + shift + / 选择Registry
![打开设置](/images/intellij_idea/ctrl_shift_xie_registry.png)
勾选compiler.automake.allow.when.app.running
![勾选compile_when_app_running](/images/intellij_idea/enable_compile_when_app_running.png)

## Spring Boot 配置
pom.xml
```
<dependencies>
    ...
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
<build>
    <plugins>
        ...
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <fork>true</fork>
            </configuration>
        </plugin>
    </plugins>
</build>
```

就这样...
