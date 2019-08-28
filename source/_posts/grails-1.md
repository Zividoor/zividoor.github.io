---
title: Grails中配置Spring AOP
date: 2019-05-05 13:25:01
tag:
---

### Grails中集成Spring AOP
* 首先，我们必须在resources.groovy中添加Spring AOP的配置
* 现在在resources.groovy中编写以下代码行
{%codeblock%}
beans = {
xmlns aop:"http://www.springframework.org/schema/aop"
aspectBean(com.test.aop.LoggerInterceptor)
aop.config("proxy-target-class":true) {}
}
{%endcodeblock%}
* 在本例中，我们使用aspectBean（com.test.aop.LoggerInterceptor）创建AOP类的bean。
* aop.config（“proxy-target-class”：true）它启用AOP来创建目标类的代理。
* 创建包含AOP实现的Groovy类。
* 我将使用AOP显示记录功能的示例。
{%codeblock%}
beans = {
package com.test.aop;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Service;
import java.util.Arrays;
@Aspect
public class LoggerInterceptor {
private static Logger logger = LoggerFactory.getLogger(LoggerInterceptor.class);
@Before("within(com.service..*)")
public void logBefore(JoinPoint joinPoint){
String logMessage = String.format("Beginning of each method: %s.%s(%s)",
joinPoint.getTarget().getClass().getName(),
joinPoint.getSignature().getName(),
Arrays.toString(joinPoint.getArgs()));
logger.info(logMessage);
}
}
{%endcodeblock%}
    @Aspect：Spring会自动检测任何带有注释为方面的类的bean。

    @Before：在匹配方法之前执行的之前的建议。

    JoinPoint：提供对当前连接点的访问（目标对象，建议方法的描述，参数类型，参数值等）。

    within（com.service .. *）：此方法将在调用com.service包下的服务类中的任何方法之前执行。*

    您可以轻松地将建议应用于任何公共方法，任何参数化方法到任何类。