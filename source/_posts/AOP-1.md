---
title: SpringAOP中通过JoinPoint获取参数名和值
date: 2019-05-07 16:10:27
tag:
---

在Java8之前，代码编译为class文件后，方法参数的类型固定，但是方法名称会丢失，方法名称会变成arg0、arg1….。在Java8开始可以在class文件中保留参数名。

{%codeblock%}
public void tet(JoinPoint joinPoint) {
        // 下面两个数组中，参数值和参数名的个数和位置是一一对应的。
        Object[] args = joinPoint.getArgs(); // 参数值
        String[] argNames = ((MethodSignature)joinPoint.getSignature()).getParameterNames(); // 参数名
}
{%endcodeblock%}

**注意** 
IDEA中需要设置Java编译参数才能获取参数信息。且JDK版本需要在1.8及以上版本。
![info](1.png)

Maven中开启编译参数方法
添加compilerArgs参数
{%codeblock%}
<plugins>
     <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-compiler-plugin</artifactId>
         <version>${maven_compiler_plugin_version}</version>
         <configuration>
             <source>${java_source_version}</source>
             <target>${java_target_version}</target>
             <encoding>${file_encoding}</encoding>
             <compilerArgs>
                 <arg>-parameters</arg>
             </compilerArgs>
         </configuration>
     </plugin>
</plugins>
{%endcodeblock%}

Eclipse中开启方法
Preferences->java->Compiler下勾选Store information about method parameters选项。 
这样在使用eclipse编译java文件的时候就会将参数名称编译到class文件中。
