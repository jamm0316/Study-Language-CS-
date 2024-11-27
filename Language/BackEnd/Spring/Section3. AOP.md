[<back](https://www.notion.so/TIL-87b0cd0d402d49b5aa87ea9615304bb1?pvs=21)

---

<aside>
📄

목차

</aside>

---

## AOP(관점 지향 프로그래밍)

**주기능과 보조기능을 분리해서 메서드에 적용**
→ 주업무가 보조업무에 의존하지 않는다.

- 주업무는 크게 변화되지 않지만 주업무를 감싸는 보조 업무(로그, 보안, 권한, 인증, Transaction 범위 등)는 변경되는 경우가 많다. 달라지는 경우 프로그램에 많은 변경이 필요하다.
- 규모가 있는 엡 애플리케이션 경우 클래스 메서드마다 이런 작업을 일일히 수행할 수 없으며 소스코드도 복잡해짐.
- 따라서 유지관리에 문제가 생길 수 있다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/9c418ef5-8eec-47cd-a8d9-549591117cc6/image.png)

### AOP 주요 용어들

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/c2ac4652-7900-4b7c-8290-a163a39f5473/image.png)

### 4가지 형태의 Advice

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/8830b71e-9192-49ce-bb8e-63d34e3dcb93/image.png)

- pointCut + Advice = Advisor

### Proxy, Interceptor

옵저버 디자인 패턴: filter와 같은 역할

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/799a5026-95cc-48ca-8607-fce3d73bab8c/image.png)

방법1. 스프링 프레임 워크 기반

방법2. 애노테이션

### Pointcut 표현식

**excution()**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/c8ac0f50-913d-4864-8ff0-f29e110306a2/23efec42-5fe3-40d1-8c13-7dae6a075f17.png)

within()

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/16fba168-7108-47d4-8d63-6552409010c3/7795d0c3-7bc6-4391-83f4-431a4d66fa71.png)

### 예제-1(XML 이용)

**AOP기능 구현 과정**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/2bc63a64-2fdc-4308-8833-00d907d7a47f/c3700f79-d6af-43e6-be72-82e642835b6b.png)

**핵심로직(주업무)**

**Calculator**

```java
//Target...주관심사(core concern), 업무로직
public class Calculator {
    public void add() {
        System.out.println("arg0개 가지고있는 add");
    }
    public void add(int x) {
        System.out.println("arg1개 가지고있는 add");
    }
    public void add(int x, int y) {
        int result=x+y;
        System.out.println("arg2개 add 결과:"+ result);
    }
    public void subtract(int x, int y) {
        int result=x - y;
        System.out.println("결과:"+ result);
    }
    public void multiply(int x, int y) {
        int result=x * y;
        System.out.println("결과:"+ result);
    }
    public void divide(int x, int y) {
        int result=x / y;
        System.out.println("결과:"+ result);
    }
}
```

**라이브러리 준비**

```xml
<properties>
    <org.aspectj-version>1.9.6</org.aspectj-version>
</properties>
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>${org.aspectj-version}</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjtools</artifactId>
    <version>${org.aspectj-version}</version>
</dependency>
<dependency>
    <groupId>aspectj</groupId>
    <artifactId>aspectjrt</artifactId>
    <version>1.5.4</version>
</dependency>
```

**보조업무1**

**LoggingAdvice**

```java
public class LoggingAdvice implements MethodInterceptor {
    public Object invoke(MethodInvocation invocation) throws Throwable {
        System.out.println("[메서드 호출 전 : LogginAdvice");
        System.out.println(invocation.getMethod() + "메서드 호출 전");

        //주업무를 수행한다.
        Object object = invocation.proceed();

        //주업무 수행후 돌아와서 수행한다.
        System.out.println("[메서드 호출 후 : loggingAdvice");
        System.out.println(invocation.getMethod() + "메서드 호출 후");
        return object;
    }
}
```

**보조업무2**

**StopWatchAdvice**

```java
public class StopWatchAdvice implements MethodInterceptor {
    public Object invoke(MethodInvocation invocation) throws Throwable {
        //보조업무
        System.out.println("******" + invocation.getMethod() + "메서드 호출 전");
        StopWatch watch = new StopWatch("계산시간");
        watch.start();

        //주업무를 수행한다.
        Object object = invocation.proceed();

        //보조업무
        System.out.println("*****" + invocation.getMethod() + "메서드 호출 후");
        watch.stop();
        System.out.println("주업무 수행 시간:" + watch.getTotalTimeMillis());
        System.out.println(watch.prettyPrint());
        return object;
    }
}
```

**aop7.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="calcTarget" class="com.shinhan.mavenProject.section7.Calculator" />
    <bean id="logAdvice1"  class="com.shinhan.mavenProject.section7.LoggingAdvice" />
    <bean id="watchAdvice" class="com.shinhan.mavenProject.section7.StopWatchAdvice" />

    <!--advisor = advice(보조업무) + pointcut(보조가 들어갈 지점) -->
    <!--advice는 보조업무-->
    <bean id="advisor1"   class="org.springframework.aop.support.DefaultPointcutAdvisor">
        <property name="advice" ref="logAdvice1" />
        <property name="pointcut" >
            <bean class="org.springframework.aop.aspectj.AspectJExpressionPointcut">
                <property name="expression">
                    <value>execution(* add(..))</value>  <!-- 모든 add()함수에 대해 적용 -->
                </property>
            </bean>
        </property>
    </bean>
    <bean id="advisor2"   class="org.springframework.aop.support.DefaultPointcutAdvisor">
        <property name="advice" ref="watchAdvice" />
        <property name="pointcut" >
            <bean id="bb" class="org.springframework.aop.aspectj.AspectJExpressionPointcut">
                <property name="expression"><value>execution(* divide(..))</value></property>
            </bean></property></bean>
    <!-- Aspect.. advisor+target = -->
    <bean id="proxyCal"
          class="org.springframework.aop.framework.ProxyFactoryBean">
        <property name="target" ref="calcTarget"/>
        <property name="interceptorNames">
            <list>
                <value>advisor1</value><value>advisor2</value>
            </list>
        </property>
    </bean>
</beans>
```

**App**

```java
package com.shinhan.mavenProject.section7;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("aop7.xml");
        Calculator cal = ctx.getBean("proxyCal", Calculator.class);
        cal.add();
        System.out.println("**************************************");
        cal.add(10);
        System.out.println("**************************************");
        cal.add(10, 20);
        System.out.println("**************************************");
        cal.subtract(10, 20);
        System.out.println("**************************************");
        cal.divide(10, 20);
    }
}
```

**실행결과**

```java
[메서드 호출 전 : LogginAdvice
public void com.shinhan.mavenProject.section7.Calculator.add()메서드 호출 전
arg0개 가지고있는 add
[메서드 호출 후 : loggingAdvice
public void com.shinhan.mavenProject.section7.Calculator.add()메서드 호출 후

**************************************

[메서드 호출 전 : LogginAdvice
public void com.shinhan.mavenProject.section7.Calculator.add(int)메서드 호출 전
arg1개 가지고있는 add
[메서드 호출 후 : loggingAdvice
public void com.shinhan.mavenProject.section7.Calculator.add(int)메서드 호출 후

**************************************

[메서드 호출 전 : LogginAdvice
public void com.shinhan.mavenProject.section7.Calculator.add(int,int)메서드 호출 전
arg2개 add 결과:30
[메서드 호출 후 : loggingAdvice
public void com.shinhan.mavenProject.section7.Calculator.add(int,int)메서드 호출 후

**************************************

결과:-10

**************************************

******public void com.shinhan.mavenProject.section7.Calculator.divide(int,int)메서드 호출 전
결과:0
*****public void com.shinhan.mavenProject.section7.Calculator.divide(int,int)메서드 호출 후
주업무 수행 시간:0
StopWatch '계산시간': running time = 236041 ns
---------------------------------------------
ns         %     Task name
---------------------------------------------
000236041  100%  
```

### 예제-2(Annotation 이용)

- **execution 이용**
    
    **JoinPoint 인터페이스**
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/e7c98fc4-439b-48e2-a817-22d3685210d6/135259bd-8352-4eaa-a035-ee64eff7b539.png)
    
    **Calculator → 핵심로직**
    
    ```java
    //Target...주관심사(core concern), 업무로직
    @Component("cal")
    public class Calculator {
        public void add() {
            System.out.println("arg0개 가지고있는 add");
        }
        public void add(int x) {
            System.out.println("arg1개 가지고있는 add");
        }
        public void add(int x, int y) {
            int result=x+y;
            System.out.println("arg2개 add 결과:"+ result);
        }
        public void subtract(int x, int y) {
            int result=x - y;
            System.out.println("결과:"+ result);
        }
        public void multiply(int x, int y) {
            int result=x * y;
            System.out.println("결과:"+ result);
        }
        public void divide(int x, int y) {
            int result=x / y;
            System.out.println("결과:"+ result);
        }
    }
    ```
    
    **LoggingAdvice → Advice1(execution)**
    
    ```java
    /**
     * @Component: <bean id="LoggingAdvice" class="com.shinhan.mavenProject.section8.LoggingAdvice"/>
     * @Aspect: Pointcut과 Advise를 합쳐 놓은 코드(보조업무)
     */
    @Component
    @Aspect
    public class LoggingAdvice {
        @Pointcut("execution(* add(int)) || execution(* add(int, int))")
        public void targetMethod2() {
            //로직없음
        }
    
        @Around("targetMethod2()")
        public Object aroundMethod2(ProceedingJoinPoint jp) throws Throwable {
            System.out.println("[메서드 호출 전 : LogginAdvice");
            System.out.println(jp.getSignature().getName() + "메서드 호출 전");
            System.out.println("------------------------------------");
    
            Object obj = jp.proceed();
    
            System.out.println("[메서드 호출 후 : loggingAdvice");
            System.out.println(jp.getSignature().getName() + "메서드 호출 후");
            return obj;
        }
    }
    ```
    
    **StopWatch → Advice2(execution)**
    
    ```java
    @Component
    @Aspect  //Pointcut + Advice
    public class StopWatchAdvice {
    
        @Pointcut("execution(* d*(int, int))")
        public void targetMethod2() {
        }
    
        @Around("targetMethod2()")
        public Object aa(ProceedingJoinPoint jp) throws Throwable {
            //보조업무
            System.out.println("******" + jp.getSignature().getName() + "메서드 호출 전");
            StopWatch watch = new StopWatch("계산시간");
            watch.start();
    
            //주업무를 수행한다.
            Object object = jp.proceed();
    
            //보조업무
            System.out.println("*****" + jp.getSignature().getName() + "메서드 호출 후");
            watch.stop();
            System.out.println("주업무 수행 시간:" + watch.getTotalTimeMillis());
            System.out.println(watch.prettyPrint());
            return object;
        }
    }
    ```
    
    **aop8.xml**
    
    ```java
    <?xml version="1.0" encoding="UTF-8"?>
    
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xmlns:aop="http://www.springframework.org/schema/aop"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
    
        <!-- base-package안에 있는 모든 Component를 찾아서 Bean생성
        @Component, @Repository, @Service, @Controller -->
        <context:component-scan base-package="com.shinhan.mavenProject.section8"/>
    
        <!-- @Aspect 스캔 -->
        <aop:aspectj-autoproxy/>
    
    </beans>
    ```
    
    **App**
    
    ```java
    public class App {
        public static void main(String[] args) {
            ApplicationContext ctx = new ClassPathXmlApplicationContext("aop8.xml");
            Calculator cal = ctx.getBean("cal", Calculator.class);
            Calculator2 cal2 = ctx.getBean("cal2", Calculator2.class);
    
            cal.add();
            cal.add(10);
            cal.add(10, 20);
        }
    }
    ```
    

- **within() 이용**
    
    **LoggingAdvice → within() 이용**
    
    ```java
    // DeptService의 모든 메서드가 로그가 남아야한다.
    //within()
    @Pointcut("within(com.shinhan.mavenProject.section8.DeptService)")
    public void targetMethod3() {
    
    }
    
    @Around("targetMethod3()")
    public Object aroundMethod3(ProceedingJoinPoint jp) throws Throwable {
        //around
        System.out.println("[메서드 호출 전 : LogginAdvice");
        System.out.println(jp.getSignature().getName() + "메서드 호출 전");
        System.out.println("------------------------------------");
    
        //before
        Object obj = jp.proceed();
        //afterReturn
        //after
    
        //around
        System.out.println("[메서드 호출 후 : loggingAdvice");
        System.out.println(jp.getSignature().getName() + "메서드 호출 후");
        return obj;
    }
    ```