[<back](https://www.notion.so/Spring-basic-41cfc2c136874d9896aa115923bd4109?pvs=21)

---

<aside>
📃

Contents

</aside>

---

## 빈 스코프란?

지금까지 우리는 스프링 빈이 스프링 컨테이너의 시작과 함께 생성되어서 스프링 컨테이너가 종료될 때 까지 유지된다고 학습했다. 이것이 스프링 빈이 기본적으로 싱글톤 스코프로 생서되기 때문이다. 스코프는 번역 그대로 빈이 존재할 수 있는 범위를 뜻한다.

**스프링은 다음과 같은 다양한 스코프를 지원한다.**

- **싱글톤**: 기본 스코프, 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 스코프이다.
- **프로토타입**: 스프링 컨테이너는 프로토타입 빈의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 매우 짧은 범위의 스코프이다.
- **웹 관련 스코프**
    - **request**: 웹 요청이 들어오고 나갈때 까지 유지되는 스코프이다.
    - **session**: 웹 세션이 생성되고 종료될 때 까지 유지되는 스코프이다.
    - **application**: 웹 서블릿 컨텍스와 같은 범위로 유지되는 스코프이다.

## 프로토타입 스코프

싱글톤 스코프의 빈을 조회하면 스프링 컨테이너는 항상 같은 인스턴스의 스프링 빈을 반환한다. 반면에 프로토타입 스코프를 스프링 컨테이너에 조회하면 스프링 컨테이너는 항상 새로운 인스턴스를 생성해서 반환한다.

**싱글톤 빈 요청**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/e0ab47d7-92b6-4e8e-8221-d9a1d716a2b0/c692586c-1117-4922-8724-baec6b9c75b4.png)

1. 싱글톤 스코프의 빈을 스프링 컨테이너에 요청한다.
2. 스프링 컨테이너는 본인이 관리하는 스프링 빈을 반환한다.
3. 이후에 스프링 컨테이너에 같은 요청이 와도 같은 객체 인스턴스 스프링 빈을 반환한다.

**프로토타입 빈 요청1**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/12e56500-b28f-4caf-ba39-f5b9390e0b2f/0fe2cbe4-7b07-4835-b526-b85c57f97bb5.png)

1. 프로토타입 스코프의 빈을 스프링 컨테이너에 요청.
2. 스프링 컨테이너는 이 시점에 프로토타입 빈을 생성하고, 필요한 의존관계를 주입한다.

**프로토타입 빈 요청2**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/104d0eb5-1290-4186-a78e-25e3af1f5aa5/e1292c26-c5c3-4954-877f-d115cd644f09.png)

1. 스프링 컨테이너는 생성한 프로토타입 빈을 클라이언트에게 반환한다.
2. 이후에 스프링 컨테이너에 같은 요청이 오면 항상 새로운 프로토타입 빈을 생성해서 반환한다.

정리

여기서 **핵심은 스프링 컨테이너는 프로토타입 빈을 생성하고, 의존관계 주입, 초기화까지만 처리한다는 것.** 클라이언트에 빈을 반환하고, 이후 스프링 컨테이너는 생성된 프로토타입 빈을 관리하지 않는다. 프로토타입 빈을 관리할 책임은 프로토타입 빈을 받은 클라이언트에 있다. 그래서 `@PostDetroy` 같은 종료메서드가 호출되지 않는다.

`core/src/test/java/hello/core/scope`

**SingletonTest**

```java
public class SingletonTest {

    @Test
    void singletonBeanFind() {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SingletonBean.class);
        System.out.println("singletonBean1");
        SingletonBean singletonBean1 = ac.getBean(SingletonBean.class);
        System.out.println("singletonBean2");
        SingletonBean singletonBean2 = ac.getBean(SingletonBean.class);
        System.out.println("singletonBean1 = " + singletonBean1);
        System.out.println("singletonBean2 = " + singletonBean2);
        assertThat(singletonBean1).isSameAs(singletonBean2);

        ac.close();
    }

    @Scope("singleton")
    static class SingletonBean {

        @PostConstruct
        public void init() {
            System.out.println("SingletonBean.init");
        }

        @PreDestroy
        public void destroy() {
            System.out.println("SingletonBean.destroy");
        }
    }
}
```

- `singletonBean` 에 `@ComponentScan`이 없는데 제대로 동작하는 이유
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/da3cb9ea-bcb8-4fa0-aecb-3e69c2f073a3/image.png)
    
    - `AnnotationConfigApplicationContext()` 의 인자로 들어간 클래스는 ComponentScan 대상 자체처럼 동작하므로 따로 등록해줄 필요 없다.
    

**실행결과**

```java
SingletonBean.init  //->  스프링 컨테이너에서 스프링 빈을 조회할 때 초기화 메서드 실행
singletonBean1
singletonBean2
singletonBean1 = hello.core.scope.SingletonTest$SingletonBean@5b07730f
singletonBean2 = hello.core.scope.SingletonTest$SingletonBean@5b07730f
SingletonBean.destroy  //->  종료전 종료 메서드 실행
```

- 싱글톤 빈은 **스프링 컨테이너 생성 시점에 초기화 메서드가 실행**된다.
- 싱글톤 빈은 조회하는 객체가 같다.

**PrototypeBeanTest**

```java
public class PrototypeTest {

    @Test
    void prototypeBeanFind() {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(prototypeBean.class);
        System.out.println("find prototypeBean1");
        prototypeBean prototypeBean1 = ac.getBean(prototypeBean.class);
        System.out.println("find prototypeBean2");
        prototypeBean prototypeBean2 = ac.getBean(prototypeBean.class);
        System.out.println("prototypeBean1 = " + prototypeBean1);
        System.out.println("prototypeBean2 = " + prototypeBean2);

        assertThat(prototypeBean1).isNotSameAs(prototypeBean2);

        ac.close();
    }

    @Scope("prototype")
    static class prototypeBean {

        @PostConstruct
        public void init() {
            System.out.println("prototypeBean.init");
        }

        public void destroy() {
            System.out.println("prototypeBean.destroy");
        }
    }
}
```

**실행결과**

```java
find prototypeBean1
prototypeBean.init  // -> 스프링 컨테이너에서 빈을 조회할 떄 생성되며, 이 때 초기화 메서드 실행.
find prototypeBean2
prototypeBean.init  // -> 스프링 컨테이너에서 빈을 조회할 떄 생성되며, 이 때 초기화 메서드 실행.
prototypeBean1 = hello.core.scope.PrototypeTest$prototypeBean@58ebfd03
prototypeBean2 = hello.core.scope.PrototypeTest$prototypeBean@5b07730f
//종료 메서드 실행 안됨.
```

- 스프링 컨테이너에서 빈을 조회할 때 생성되며, 이 때 초기화 메서드 실행
- 생성하는 객체가 서로 다름.
- 종료메서드 실행 안됨.

**싱글톤 스코프와 프로토타입 스코프의 차이점**

- 초기화 시점
    - **싱글톤 빈**: 스프링 컨테이너 생성시점 초기화 진행
    - **프로토타입 빈**: 스프링 컨테이너에서 빈 조회시 초기화 진행
- 객체
    - **싱글톤 빈**: 호출할 때마다 같은 객체 호출
    - **프로토타입 빈**: 호출할 때마다 다른 객체 호출
- 종료 메서드
    - **싱글톤 빈**: 종료전에 `@PreDestroy`메서드 호출
    - **프로토타입 빈**: 종료전에 `@PreDestroy` 메서드 호출 안함
- 싱글톤 빈은 스프링 컨테이너가 관리하기 때문에 스프링 컨테이너가 종료될 때 빈의 종료메서드가 실행되지만, 프로토타입 빈은 스프링 컨테이너가 생성과 의존관계 주입 그리고 초기화 까지만 관여하고 더는 관리하지 않는다. 따라서 프로토타입 빈은 스프링 컨테이너가 종료될 때 `@PreDestroy`같은 종료 메서드가 전혀 실행되지 않는다.

**프로토타입 빈의 특징 정리**

- 스프링 컨테이너에 요청할 때마다 새로 생성된다.
- 스프링 컨테이너는 프로토타입 빈의 생성과 의존관계 주입 그리고 초기화 까지만 관여한다.
- 종료 메서드가 호출되지 않는다.
- 그래서 프로토타입 빈은 프로토타입 빈을 조회한 클라이언트가 관리해야 한다. 종료 메서드에 대한 호출도 클라이언트가 직접 해야한다.

## 프로토타입 스코프 - 싱글톤 빈과 함께 사용시 문제점

스프링 컨테이너에 프로토타입 스코프의 빈을 요청하면 항상 새로운 객체 인스턴스를 생성해서 반환한다. 하지만 싱글톤 빈과 함께 사용할 때는 의도한 대로 잘 동작하지 않으므로 주의해야한다.

### 프로토타입 빈 직접 요청

**스프링 컨테이너에 프로토타입 빈 직접 요청1**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/e337ed97-5390-4a71-bb32-24792b29b6da/47ea61fc-729a-433a-aee7-796c173f9a2b.png)

1. 클라이언트 A가 스프링 컨테이너에 프로토타입 빈을 요청한다.
2. 스프링 컨테이너는 프로토타입 빈을 새로 생성해서 반환(**x01**)한다. 해당 빈의 count 필드 값은 0이다.
3. 클라이언트는 조회한 프로토타입 빈에 `addCount()`를 호출하면서 count 필드를 +1 한다.
4. 결과적으로 프로토타입 빈(**x01**)의 count는 1이 된다.

**스프링 컨테이너에 프로토타입 빈 직접 요청2**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/9bd0508e-72df-4c57-a0df-f751422a9702/3dae7745-5e63-4f3c-91a7-13bcdceb08ef.png)

1. 클라이언트 B가 스프링 컨테이너에 프로토타입 빈을 요청한다.
2. 스프링 컨테이너는 프로토타입 빈을 새로 생성해서 반환(x02)한다. 해당 빈의 count 필드 값은 0이다.
3. 클라이언트는 조회한 프로토타입 빈에 `addCount()` 를 호출하면서 count 필드를 +1 한다.
4. 결과적으로 프로토타입 빈(**x02**)의 count는 1이 된다.

**SingletonWithPrototypeTest1**

```java
public class SingletonWithPrototypeTest1 {

    @Test
    void prototypeFind() {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(prototypeBean.class);

        prototypeBean prototypeBean1 = ac.getBean(prototypeBean.class);
        prototypeBean1.addCount();
        assertThat(prototypeBean1.getCount()).isEqualTo(1);

        prototypeBean prototypeBean2 = ac.getBean(prototypeBean.class);
        prototypeBean2.addCount();
        assertThat(prototypeBean2.getCount()).isEqualTo(1);
    }

    @Scope("prototype")
    static class prototypeBean {
        private int count;

        public int addCount() {
            return count++;
        }

        public int getCount() {
            return count;
        }

        @PostConstruct
        public void init() {
            System.out.println("prototypeBean.init" + this);
        }

        @PreDestroy
        public void destroy() {
            System.out.println("prototypeBean.destroy");
        }
    }
}
```

### 싱글톤 빈에서 프로토타입 빈 사용

clientBean이라는 싱글톤 빈이 의존관계 주입을 통해서 프로토 타입 빈을 주입받아서 사용하는 예

**싱글톤에서 프로토타입 빈 사용1**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/49c8507d-97ab-4f2a-ad7c-fa41d8bd7bd6/80757f04-389b-4515-b611-8b508eca74a0.png)

- `clientBean`은 싱글톤이므로, 보통 스프링 컨테이너 생성 시점에서 함께 생성됨. 의존관계 주입도 발생
1. `clientBean`은 의존관계 자동 주입을 사용한다. 주입 시점에 스프링 컨테이너에 프로토타입 빈 요청
2. 스프링 컨테이너는 프로토타입 빈을 생성해서 `clientBean`에 반환한다. 프로토타입 빈의 count 값은 0이다.
- 이제 `clientBean`은 프로토타입 빈을 내부 필드에 보관한다.(정확히는 참조값을 보관한다.)

**싱글톤에서 프로토타입 빈 사용2**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/92fb8abc-5240-43df-b998-e392ed74404c/271d53ed-fb2b-4850-abf8-a93967523c8b.png)

- 클라이언트 A는 `clientBean`을 스프링 컨테이너에 요청해서 받는다. 싱글톤 이므로 항상 같은 `clientBean`이 반환된다.
1. 클라이언트 A는 `clientBean.logic()`을 호출한다.
2. `clientBean`은 `prototypeBean`의 `addCount()`를 호출해서 프로토 타입 빈의 count를 증가한다. count값이 1이 된다.

**싱글톤에서 프로토타입 빈 사용3**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/23e4474f-877c-4410-ba3b-791b4b786a97/2b878ec0-8f7f-4189-b206-514dc47a5290.png)

- 클라이언트 B는 `clientBean`을 스프링 컨테이너에 요청해서 받는다. 싱글톤이므로 항상 같은 `clientBean`이 반환된다.
- 여기서 중요한 점은 `clientBean`**이 내부에 가지고 있는 프로토타입 빈은 이미 과거에 주입이 끝난 빈이다. 주입 시점에 스프링 컨테이너에 요청해서 프로토타입 빈이 새로 생성되 것이지, 사용할 때마다 새로 생성되는 것이 아니다.**
1. 클라이언트B는 `clientBean.logic()`을 호출한다.
2. `clientBean`은 `prototypeBean`의 `addCount()`를 호출해서 프로토타입 빈의 count를 증가한다. 이때 count는 1에서 2가 된다.

**SingletonWithPrototypeTest1**

```java
public class SingletonWithPrototypeTest1 {

    @Test
    void prototypeFind() {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(prototypeBean.class);

        prototypeBean prototypeBean1 = ac.getBean(prototypeBean.class);
        prototypeBean1.addCount();
        assertThat(prototypeBean1.getCount()).isEqualTo(1);

        prototypeBean prototypeBean2 = ac.getBean(prototypeBean.class);
        prototypeBean2.addCount();
        assertThat(prototypeBean2.getCount()).isEqualTo(1);
    }

    @Test
    void singletonClientUsePrototyp() {
        AnnotationConfigApplicationContext ac =
                new AnnotationConfigApplicationContext(clientBean.class, prototypeBean.class);

        //clientBean이 생성되면서 최초 prototypeBean이 주입됨.
        clientBean clientBean1 = ac.getBean(clientBean.class);
        int count1 = clientBean1.logic();
        assertThat(count1).isEqualTo(1);

        //이때 새로운 prototypeBean이 생성되는 것이 아닌 생성시점에 주입된 prototypeBean 사용
        clientBean clientBean2 = ac.getBean(clientBean.class);
        int count2 = clientBean2.logic();
        assertThat(count2).isEqualTo(2);
    }

    @Scope("singleton")
    static class clientBean {
        private final prototypeBean prototypeBean;  //생성시점에 주입됨.

        @Autowired
        public clientBean(SingletonWithPrototypeTest1.prototypeBean prototypeBean) {
            this.prototypeBean = prototypeBean;
        }

        public int logic () {
            prototypeBean.addCount();
            return prototypeBean.getCount();
        }
    }

    @Scope("prototype")
    static class prototypeBean {
        private int count;

        public int addCount() {
            return count++;
        }

        public int getCount() {
            return count;
        }

        @PostConstruct
        public void init() {
            System.out.println("prototypeBean.init" + this);
        }

        @PreDestroy
        public void destroy() {
            System.out.println("prototypeBean.destroy");
        }
    }
}
```

![Singleton clientBean using the same prototypeBean instance.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/97c8f055-218b-494e-9840-48a1d792c87c/Singleton_clientBean_using_the_same_prototypeBean_instance.png)

스프링은 일반적으로 싱글톤 빈을 사용하므로, 싱글톤 빈이 프로토타입 빈을 사용하게 된다. 그런데 싱글톤 빈은 생성 시점에만 의존관계 주입을 받기 때문에, 프로토타입 빈이 새로 생성되기는 하지만, 싱글톤 빈과 함께 계속 유지되는 것이 문제.

프로토타입 빈을 주입 시점에만 새로 생성하는게 아니라, 사용할 떄마다 새로 생성해서 사용하는것 을 원한다면?

> 참고: 여러 빈에서 같은 프로토타입 빈을 주입 받으면, 주입 받는 시점에 각각 새로운 프로토타입 빈이 생성된다. 예를 들어서 clientA, clientB가 각각 의존관계 주입을 받으면 각각 다른 인스턴스의 프로토타입 빈을 주입받는다.
clientA → prototypeBean@x01
clientB → prototypeBean@x02
물론 사용할 때마다 새로 생성되는 것은 아니다.
> 

## 프로토타입 스코프 - 싱글톤 빈과 함께 사용시 Provider로 문제 해결

### 싱글톤 컨테이너에 요청

가장 간단한 방법은 싱글톤 빈이 프로토타입을 사용할 때마다 스프링 컨테이너에 새로 요청하는 것.

**SingletonWithPrototypeTest1**

```java
public class SingletonWithPrototypeTest1 {

    @Test
    void singletonClientUsePrototype() {
        AnnotationConfigApplicationContext ac =
                new AnnotationConfigApplicationContext(clientBean.class, prototypeBean.class);

        //clientBean이 생성되면서 ac에 접근할 수 있는 applicationContext가 주입됨.
        clientBean clientBean1 = ac.getBean(clientBean.class);

        //prototypeBean 인스턴스가 생성되어 로직 실행
        int count1 = clientBean1.logic();
        assertThat(count1).isEqualTo(1);

        //clientBean1에서 생성된 스프링 컨테이너가 호출됨.
        clientBean clientBean2 = ac.getBean(clientBean.class);
        
        //새로운 prototypeBean 인스턴스가 생성되어 로직 실행
        int count2 = clientBean2.logic();
        assertThat(count2).isEqualTo(1);

        assertThat(clientBean1).isSameAs(clientBean2);
    }

    @Scope("singleton")
    static class clientBean {

        @Autowired
        ApplicationContext applicationContext;

        public int logic () {
            prototypeBean prototypeBean = applicationContext.getBean(prototypeBean.class);
            prototypeBean.addCount();
            return prototypeBean.getCount();
        }
    }

    @Scope("prototype")
    static class prototypeBean {
        private int count;

        public int addCount() {
            return count++;
        }

        public int getCount() {
            return count;
        }

        @PostConstruct
        public void init() {
            System.out.println("prototypeBean.init" + this);
        }

        @PreDestroy
        public void destroy() {
            System.out.println("prototypeBean.destroy");
        }
    }
}

```

![Singleton clientBean using Prototype Beans.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/82ca3d42-eac5-4f6a-a011-842b33e7fbfb/Singleton_clientBean_using_Prototype_Beans.png)

- 실행해보면 ac.getBean()을 통해서 항상 새로운 프로토타입 빈이 생성되는 것을 확인할 수 있다.
- 의존관계를 외부에서 주입(DI)받는게 아니라 이렇게 직접 필요한 의존관계를 찾는 것을 Dependency Lookup(DL) 의존관계 조회(탐색)이라고 한다.
- 그런데 이렇게 스프링 애플리케이션 컨텍스트 전체를 주입받게 되면, 스프링 컨테이너에 종속적인 코드가 되고, 단위 테스트도 어려워진다.
- 지금 필요한 기능은 지정한 프로토타입 빈을 컨테이너에서 대신 찾아주는 딱 **DL** 정도의 기능만 제공하는 무언가 있으면 된다.

### Objectfactory, ObjectProvider

지정한 빈을 컨테이너에서 대신 찾아주는 DL 서비스를 제공하는 것이 바로 `ObjectProvider`이다. 참고로 과거에는 `Objectfactory`가 있었는데, 여기에 편의기능을 추가해서 `ObjectProvider`가 만들어 졌다.

```java
public class SingletonWithPrototypeTest2 {

    @Test
    void singletonClientUsePrototype() {
        AnnotationConfigApplicationContext ac =
                new AnnotationConfigApplicationContext(clientBean.class, prototypeBean.class);

        clientBean clientBean1 = ac.getBean(clientBean.class);
        int count1 = clientBean1.logic();
        assertThat(count1).isEqualTo(1);

        clientBean clientBean2 = ac.getBean(clientBean.class);
        int count2 = clientBean2.logic();
        assertThat(count2).isEqualTo(1);

        assertThat(clientBean1).isSameAs(clientBean2);
    }

    @Scope("singleton")
    static class clientBean {

        @Autowired
        private ObjectProvider<prototypeBean> prototypeBeanProvider;

        public int logic () {
            prototypeBean prototypeBean = prototypeBeanProvider.getObject();
            prototypeBean.addCount();
            return prototypeBean.getCount();
        }
    }

    @Scope("prototype")
    static class prototypeBean {
        private int count;

        public int addCount() {
            return count++;
        }

        public int getCount() {
            return count;
        }

        @PostConstruct
        public void init() {
            System.out.println("prototypeBean.init: " + this);
        }

        @PreDestroy
        public void destroy() {
            System.out.println("prototypeBean.destroy");
        }
    }
}
```

**실행결과**

```java
prototypeBean.init: hello.core.scope.SingletonWithPrototypeTest2$prototypeBean@207ea13
prototypeBean.init: hello.core.scope.SingletonWithPrototypeTest2$prototypeBean@44c79f32
```

- **ObjectProvider의 핵심 컨셉**
    - Context에 생성된 스프링 컨테이너에서 직접 호출하여 조회하기보다 대신 조회해주는 대리자 역할

<aside>
💡

**Provider의 역할**

해당 객체를 지연호출 해주는 역할을한다.

빈의 생성주기 관점에서 보면 스프링 컨테이너 singleton 빈인 clientBean 생성 시점에 prototypeBean이 생성되어 의존관계 주입 및 초기화가 진행되면, singleton Bean에 종속되어, 다시 호출되어도 같은 객체가 호출되어, prototype의 매번 새로운 객체를 생성한다는 특성을 잃어버린다.

그러나 호출 지연을 통해, 스프링 컨테이너 생성 시점에 컨테이너에 등록은 해두되 호출을 하지 않는다면 clientBean입장에서는 prototypeBean이 없는 것이기 때문에 prototypeBean은 clientBean에 종속되지 않는다. 따라서 prototypeBean을 호출할 때 prototype의 매번 새로운 객체를 호출한다는 특성을 잃지 않을 수 있다.

**즉, Provider는 생성시점을 지연해 의존관계 주입, 객체 활성화 여부를 조절해주는 역할을 해준다.**

</aside>

- 실행해보면 `prototypeBeanProvider.getObject()`을 통해서 항상 **새로운 프로토타입 빈이 생성되는 것을 확인할 수 있다.**
- `ObjectProvider`의 `getObject()`를 호출하면 내부에서는 스프링 컨테이너를 통해 해당 빈을 찾아서 반환.(**DL**)
- 스프링이 제공하는 기능을 사용하지만, 기능이 단순하므로 단위테스트를 만들거나 mock 코드를 만들기는 훨씬 쉬워진다.
- `ObjectProvider`는 지금 딱 필요한 DL정도의 기능만 제공한다.

**특징**

- `ObjectFactory`: 기능이 단순, 별도의 라이브러리 필요 없음. 스프링에 의존.
- `ObjectProvider`: ObjectFactory 상속 옵션, 스트림 처리등 편의 기능이 많고, 별도의 라이브러리 필요 없음. 스프링에 의존.

### JSR-330 Provider

마지막 방법은 `jakarta.inject.Provider` 라는 JSR-330 자바 표준을 사용하는 방법이다.

이 방법을 사용하려면 `jakarta.inject:jakarta.inject-api:2.0.1` 라이브러리를 gradle에 추가해야한다.

**build.gradle**

```groovy
dependencies {
  ...
	implementation 'jakarta.inject:jakarta.inject-api:2.0.1'
	...
}
```

**SingletonWithPrototypeTest2**

```java
@Scope("singleton")
static class clientBean {

    @Autowired
    private Provider<PrototypeBean> prototypeBeanProvider;

    public int logic () {
        PrototypeBean prototypeBean = prototypeBeanProvider.get();
        prototypeBean.addCount();
        return prototypeBean.getCount();
    }
}
```

**실행결과**

```java
prototypeBean.init: hello.core.scope.SingletonWithPrototypeTest2$PrototypeBean@4e28bdd1
prototypeBean.init: hello.core.scope.SingletonWithPrototypeTest2$PrototypeBean@5cc126dc
```

- 실행결과 `Provider.get()`을 통해서 항상 새로운 프로토타입 빈이 생성되는 것을 확인할 수 있다.
- `Provider`의 `get()`을 호출하면 내부에서는 스프링 컨테이너를 통해 빈을 찾아서 반환한다(**DL**)
- 자바 표준이고, 기능이 단순하므로 단위테스트를 만들거나 mock 코드를 만들기는 훨씬 쉬워진다.
- `Provider`는 지금 딱 필요한 DL 정도의 기능만 제공한다.

**특징**

- `get()` 메서드 하나로 기능이 매우 단순하다.
- 별도의 라이브러리가 필요하다.
- 자바 표준이므로 스프링이 아닌 다른 컨테이너에서도 사용할 수 있다.

**정리**

- 그러면 프로토타입 빈을 언제 사용할까? 매번 사용할 때마다 의존관계 주입이 완료된 새로운 객체가 필요하면 사용하면 된다. 그런데 실무에서 웹 애플리케이션을 개발해보면, 싱글톤 빈으로 대부분의 문제를 해결할 수 있기 때문에 프로토타입 빈을 직접적으로 사용하는 일은 매우 드물다.
- `ObjectProvider`, `JSR330 Provider` 등은 프로토타입 뿐만 아니라 DL이 필요한 경우 언제든지 사용할 수 있다.

> **참고**: 스프링이 제공하는 메서드에 @Lookup 애노테이션을 사용하는 방법도 있지만, 이전 방법들로 충분하고, 고려해야할 내용이 많아서 생략함.
> 

> **참고**: 실무에서 자바 표준인 JSR-330 Provider를 사용할 것인지, 아니면 스프링이 제공하는 ObjectProvier르 사용할 것인가 고민될 것이다. ObjectProvider는 DL을 위한 편의 기능을 많이 제공해주고 스프링 외에 별도의 의존관계 추가가 필요 없기 때문에 편리하다. 만약(정말 그럴일은 없겠지만) 코드를 스프링이 아닌 다른 컨테이너에서도 사용할 수 있어야 한다면 JSR-330 Provider를 사용해야한다.

스프링을 사용하다 보면 이 기능 뿐만 아니라 다른 기능들도 자바 표준과 스프링이 제공하는 기능이 겹칠떄가 많다. 대부분 스프링이 더 다양하고 편리한 기능을 제공해주기 때문에, 특별히 다른 컨테이너에서 사용할 일이 없다면 스프링이 제공하는 기능을 사용하면 된다.
> 

## 웹 스코프

지금까지 싱글톤과 프로토타입 스코프를 학습했다. 싱글톤은 스프링 컨테이너의 시작과 끝까지 함께하는 매우 긴 스코프이고, 프로토타입은 생성과 의존관계 주입, 그리고 초기화까지 진행하는 특별한 스코프이다.

**웹 스코프의 특징**

- 웹 스코프는 웹 환경에서만 동작한다.
- 웹 스코프는 프로토타입과 다르게 스프링이 해당 스코프의 종료시점까지 관리한다. 따라서 종료 메서드가 호출된다.

**웹 스코프의 종류**

- **request**: HTTP 요청 하나가 들어오고 나갈 때까지 유지되는 스코프, 각각의 HTTP 요청마다 별도의 빈 인스턴스가 생성되고, 관리된다.
- **session**: HTTP Session과 동일한 생명주기를 가지는 스코프
- **application**: 서블릿 컨텍스트(`ServletContext`)와 동일한 생명주기를 가지는 스코프
- **websoket**: 웹 소켓과 동일한 생명주기를 가지는 스코프

사실 세션이나, 서블릿 컨텍스트, 웹 소켓 같은 용어를 잘 모를 수 있다. 여기서는 request 스코프를 예제로 설명하고, 나머지도 범위만 다를 뿐 동작 방식은 비슷하다.

**HTTP request 요청 당 각각 할당되는 request 스코프**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/77fb7139-b76d-44e4-b1a0-99cec910de2d/8f27cecd-f1b9-4ce5-94bd-2aefd7e27a97.png)

## request 스코프 예제 만들기

### 웹 환경 추가

웹 스코프는 웹 환경에서만 동작하므로 web 환경이 동작하도록 라이브러리 추가.

**build.gradle**

```groovy
dependencies {
  ...
	//web 라이브러리 추가
	implementation 'org.springframework.boot:spring-boot-starter-web'
	...
}
```

이제 hello.core.CoreApplication의 main 메서드를 실행하면 웹 애플리케이셔이 실행되는 것을 확인할 수 있다.

```java
2024-09-23T04:33:25.174+09:00  INFO 1619 --- [core] [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port 8080 (http) with context path '/'
2024-09-23T04:33:25.178+09:00  INFO 1619 --- [core] [           main] hello.core.CoreApplication               : Started CoreApplication in 0.658 seconds (process running for 0.806)
```

> **참고**: `spring-boot-starter-web` 라이브러리를 추가하면 스프링 부트는 내장 톰켓 서버를 활용해서 웹 서버와 스프링을 함께 실행시킨다.
> 

> **참고**: 스프링 부트는 웹 라이브러리가 없으면 우리가 지금까지 학습한 `AnnotationConfigApplicationContext`을 기반으로 애플리케이션을 구동한다. 웹 라이브러리가 추가되면 웹 관련된 추가 설정과 환경들이 필요하므로 `AnnotationConfigServletWebServerApplicationContext` 를 기반으로 애플리케이션을 구동한다.
> 

만약 기본 포트인 8080 포트를 다른 곳에서 사용중이어서 오류가 발생하면 포트를 변경해야 한다. 9090 포트로 변경하려면 다음 설정을 추가하자.

`main/resources/application.properties`

```groovy
server.port=9090
```

### request 스코프 예제 개발

동시에 여러 HTTP 요청이 오면 정확히 어떤 요청이 남긴 로그인지 구분하기 어렵다.

이럴 때 사용하기 좋은 것이 request 스코프이다.

다음과 같이 로그가 남도록 request 스코프를 활용해서 추가 기능을 개발해보자.

```java
[d06b992f...] request scope bean create
[d06b992f...] [http://localhost:8080/log-dome] controller test
[d06b992f...] [http://localhost:8080/log-dome] service id = testId
[d06b992f...] request scope bean close
```

- 기대하는 공통 포멧: [UUID][requestURL]{message}
- UUID를 사용해서 HTTP 요청을 구분하자.
- requestURL 정보도 추가 넣어서 어떤 URL을 요청해서 남은 로그인지 확인하자.

**MyLogger**

```java
package hello.core.common;

import jakarta.annotation.PostConstruct;
import jakarta.annotation.PreDestroy;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

import java.util.UUID;

@Component
@Scope(value = "request")
public class MyLogger {
    private String uuid;
    private String requestURL;

    public void setRequestURL(String requestURL) {
        this.requestURL = requestURL;
    }

    public void log(String message) {
        System.out.println("[" + uuid + "]" + "[" + requestURL + "] " + message);
    }

    @PostConstruct
    public void init() {
        uuid = UUID.randomUUID().toString();
        System.out.println("[" + uuid + "] request scope bean create" + this);
    }
    
    @PreDestroy
    public void close() {
        System.out.println("[" + uuid + "] request scope bean close");
    }
}
```

- 로그를 출력하기 위한 `MyLogger` 클래스
- `@Scope(value = “request”)`를 사용해서 request 스코프로 지정. 이 빈은 HTTP 요청 당 하나 씩 생성되고, HTTP 요청이 끝나는 시점에 소멸된다.
- 이 빈은 생성 시점에서 자동으로 `@PostConstruct` 초기화 메서드를 사용해서 uuid를 생성해서 저장해둔다. 이 빈은 HTTP요청 당 하나씩 생성되므로, uuid를 저장해두면 다른 HTTP 요청과 구분할 수 있다.
- 이 빈이 소멸되는 시점에 `@PreDestroy`를 사용해서 종료 메시지를 남긴다.
- `requestURL`은 이 빈이 생성되는 시점에는 알 수 없으므로, 외부에서 `setter`로 입력받아 넘긴다.

**LogDemoController**

```java
@Controller
@RequiredArgsConstructor
public class LogDemoController {

    private final LogDemoService logDemoService;
    private final MyLogger myLogger;

    @RequestMapping("log-demo")
    @ResponseBody
    public String logDemo(HttpServletRequest request) {
        String requestURL = request.getRequestURL().toString();
        myLogger.setRequestURL(requestURL);

        myLogger.log("controller test");
        logDemoService.logic("testId");
        return "OK";
    }
}
```

- 로거가 잘 작동하는지 확인하는 테스트용 컨트롤러.
- 여기서 HttpServletRequest를 통해 요청 URL을 받았다.
    - requestURL 값: `http://localhost:8080/log-demo`
- 이렇게 받은 requestURL 값을 myLogger에 저장해둔다. myLogger는 HTTP 요청 당 각각 구분되는 다른 HTTP 요청 떄문에 값이 섞이는 걱정은 하지 않아도 된다.

> **참고**: requestURL을 MyLogger에 저장하는 부분은 컨트롤로 보다는 공통 처리가 가능한 스프링 인터셉터나 서블릿 필터 같은 곳을 활용하는 것이 좋다. 여기서는 예제를 단순화하고, 아직 스프링 인터셉터를 학습하지 않았기에 컨트롤러를 사용했다. 스프링 웹에 익숙하다면 인터셉터를 사용해서 구현해보자.
> 
- 스프링 인터셉터: HTTP request 요청이 오면 컨트롤러 호출 직전에 공통화해서 처리할 수 있는 로직

**LogDemoService**

```java
@Service
@RequiredArgsConstructor
public class LogDemoService {

    private final MyLogger myLogger;

    public void logic(String id) {
        myLogger.log("service id =" + id);
    }
}
```

- 비즈니스 로직이 있는 서비스 계층에도 로그 출력.
- 여기서 중요한 점은 request scope를 사용하지 않고 파라미터로 이 모든 정보를 서비스 계층에 넘긴다면, 파라미터가 많아서 지저분해진다. 더 문제는 requestURL같은 웹과 관련된 정보가 웹과 관련없는 서비스 계층까지 넘어간다. 웹과 관련된 부분은 컨트롤러까지만 사용해야한다. 서비스 계층은 웹 기술에 종속되지 않고, 가급적 순수하게 유지하는 것이 유지보수 관점에서 좋다.
- request scope의 MyLogger 덕분에 이런 부분을 파라미터로 넘기지 않고, MyLogger의 멤버변수에 저장해서 코드와 계층을 깔끔하게 유지할 수 있다.

- **CoreApplication**을 실행하면 아래와 같은 오류가 뜬다.

```java
Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'logDemoService' defined in file [/Users/evan/Documents/Developer/03_Study/02_Spring/03_basic/core/build/classes/java/main/hello/core/web/LogDemoService.class]: Unsatisfied dependency expressed through constructor parameter 0: Error creating bean with name **'myLogger': Scope 'request' is not active** for the current thread; consider defining a scoped proxy for this bean if you intend to refer to it from a singleton

```

- myLogger의 Scope ‘reqeust’가 활성화 되지 않아서 오류가 난것임.
    - 이유는 스프링 컨테이너 생성 시점에 의존관계 주입이 일어난다.
    - request 스코프는 HTTP 요청이 들어와야 생성되는 빈이기 때문에 스프링 컨테이너 생성 시점에서는 HTTP 요청이 없으므로 빈을 등록할 수 없다.
    - 따라서, 해당 스코프가 활성화 되지 않았다는 오류를 반환하는 것.
    - 이 문제를 해결하기 위해서는 스프링 컨테이너 생성시점, 의존관계 주입단계가 아니라 실제 HTTP 요청이 들어왔을 때 해당 빈이 등록되도록 호출을 지연시켜야한다.
    - 여기서 앞서 배운 `Provider`를 사용하면 문제를 해결할 수 있다.

## 스코프와 Provider

첫번째 해결방법은 Provider를 써보는 것이다.

**LogdemoController**

```java
@Controller
@RequiredArgsConstructor
public class LogDemoController {

    private final LogDemoService logDemoService;
    private final ObjectProvider<MyLogger> myLoggerObjectProvider;

    @RequestMapping("log-demo")
    @ResponseBody
    public String logDemo(HttpServletRequest request) {
        String requestURL = request.getRequestURL().toString();
        MyLogger myLogger = myLoggerObjectProvider.getObject();
        myLogger.setRequestURL(requestURL);

        myLogger.log("controller test");
        logDemoService.logic("testId");
        return "OK";
    }
}
```

**LogDemoService**

```java
package hello.core.web;

import hello.core.common.MyLogger;
import lombok.RequiredArgsConstructor;
import org.springframework.beans.factory.ObjectProvider;
import org.springframework.stereotype.Service;

@Service
@RequiredArgsConstructor
public class LogDemoService {

    private final ObjectProvider<MyLogger> myLoggerObjectProvider;

    public void logic(String id) {
        MyLogger myLogger = myLoggerObjectProvider.getObject();
        myLogger.log("service id =" + id);

    }
}
```

localhost:8080/log-demo

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/807682ec-1d2b-49fd-967d-9ca8f1aee7d8/image.png)

**실행결과**

```java
[205b8b1d-47bf-475c-9546-03beaefd546e] request scope bean createhello.core.common.MyLogger@112a0cf9
[205b8b1d-47bf-475c-9546-03beaefd546e][http://localhost:8080/log-demo] controller test
[205b8b1d-47bf-475c-9546-03beaefd546e][http://localhost:8080/log-demo] service id =testId
[205b8b1d-47bf-475c-9546-03beaefd546e] request scope bean close
[65d8e493-6cb6-4175-8132-c78425f6793d] request scope bean createhello.core.common.MyLogger@292acc88
[65d8e493-6cb6-4175-8132-c78425f6793d][http://localhost:8080/log-demo] controller test
[65d8e493-6cb6-4175-8132-c78425f6793d][http://localhost:8080/log-demo] service id =testId
[65d8e493-6cb6-4175-8132-c78425f6793d] request scope bean close
```

- `ObjectProvider` 덕분에 `ObjectProvider.getObject()` 를 호출하는 시점까지 request scope **빈을 스프링 컨테이너에서 호출해주는 시점을 지연**할 수 있었다.
- `ObjectProvider.getObject()`를 호출하는 시점에는 HTTP 요청이 진행중이므로 request scope 빈의 생성이 정상처리된다.
- `ObjectProvider.getObject()`를 `LogDemoController`, `LogDemoService`에서 각각 한번씩 따로 호출해도 같은 HTTP요청이면 같은 스프링 빈이된다. → 내가 직접 이걸 구분하려면 얼마나 힘들까

이정도에서 끝내도 될 것 같지만 개발자들의 코드 몇자를 더 줄이려는 욕심을 끝이 없다.

## 스코프와 프록시

**MyLogger**

```java
@Component
@Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
public class MyLogger {
```

- 여기가 핵심. `proxyMode = ScopedProxyMode.TARGET_CLASS` 를 추가.
    - 적용 대상이 인터페이스가 아닌 클래스이면 `TRGET_CLASS`를 선택
    - 적용 대상이 인터페이스면 `INTERFACES` 선택
- 이렇게 하면 `MyLogger`의 가짜 프록시 클래스를 만들어두고 HTTP request와 상관 없이 가짜 프록시 클래스를 다른 빈에 미리 주입해 둘 수 있다.

**LogDemoController**

```java
@Controller
@RequiredArgsConstructor
public class LogDemoController {

    private final LogDemoService logDemoService;
    private final MyLogger myLogger;

    @RequestMapping("log-demo")
    @ResponseBody
    public String logDemo(HttpServletRequest request) {
        String requestURL = request.getRequestURL().toString();
        myLogger.setRequestURL(requestURL);

        //myLogger가 어떤식으로 생성되는지 확인하는 코드
        System.out.println("myLogger = " + myLogger.getClass());

        myLogger.log("controller test");
        logDemoService.logic("testId");
        return "OK";
    }
}

```

**LogDemoService**

```java
@Service
@RequiredArgsConstructor
public class LogDemoService {

    private final MyLogger myLogger;

    public void logic(String id) {
        myLogger.log("service id =" + id);
    }
}
```

**실행결과**

```java
[cfc174b4-e04a-4e10-9b25-1d9f6392c92e] request scope bean createhello.core.common.MyLogger@60cdd5ef
// 스피링 빈이 조작해서 만들어낸 스프링빈
myLogger = class hello.core.common.MyLogger$$Spring**CGLIB**$$0
[cfc174b4-e04a-4e10-9b25-1d9f6392c92e][http://localhost:8080/log-demo] controller test
[cfc174b4-e04a-4e10-9b25-1d9f6392c92e][http://localhost:8080/log-demo] service id =testId
[cfc174b4-e04a-4e10-9b25-1d9f6392c92e] request scope bean close
```

- `@Scope`의 `proxyMode = ScopeProxyMode.TARGET_CLASS`를 설정하면 스프링 컨테이너는 CGLIB라는 바이트 코드를 조작하는 라이브러리를 사용해서, MyLogger를 상속받은 가짜 프록시 객체를 생성한다.
- 결과를 확인해보면 우리가 등록한 순수한 MyLogger 클래스가 아니라 `MyLogger$$SpringCGLIB$$0` 이라는 클래스로 만들어진 객체가 대신 등록된 것을 확인할 수 있다.
- 그리고 스프링 컨테이너에 “myLogger”라는 이름으로 진짜 대신에 이 가짜 프록시 객체를 등록한다.
- `ac.getBean(”myLogger”, MyLogger.class)`로 조회해도 프록시 객체가 조회되는 것을 확인할 수 있다.
- 그래서 의존관계 주입도 이 가짜 프록시 객체가 주입된다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/47e6df77-0fa2-487d-b852-02ba691332c0/6edb100a-7ded-4647-afe8-ffab171ef8fc.png)

**가짜 프록시 객체는 요청이 오면 그때 내부에서 진짜 빈을 요청하는 위임 로직이 들어있다.**

- 가짜 프록시 객체는 내부에 진짜 myLogger를 찾는 방법을 알고 있다.
- 클라이언트가 `myLogger.logic()`을 호출하면 사실 가짜 프록시 객체의 메서드를 호출한 것이다.
- 가짜 프록시 객체는 request 스코프의 진짜 `myLogger.logic()`를 호출한다.
- 가짜 프록시 객체는 원본 클래스를 상속 받아서 만들어졌기 때문에 이 객체를 사용하는 클라이언트 입장에서는 사실 원본인지 아닌지 모르게, 동일하게 사용할 수 있다.(다형성)

**동작 정리**

- CGLIB라는 라이브러리로 내 클래스를 상속 받은 가짜 프록시 객체를 만들어서 주입한다.
- 이 가짜 프록시 객체는 실제 요청이 오면 그 때 내부에서 실제 빈을 요청하는 위임 로직이 들어있다.
- 가짜 프록시 객체는 실제 request scope와는 관계가 없다. 그냥 가짜이고, 내부에는 단순히 위임로직만 있고, 싱글톤 처럼 동작한다.

**특징 정리**

- 프록시 객체 덕분에 클라이언트는 마치 싱글톤 빈을 사용하듯 편리하게 request scope를 사용할 수 있다.
    
    (request scope는 컨테이너 생성 시점에서 생성되지만, HTTP요청을 필요로하기 때문에 스프링 컨테이너에서 해당 빈 호출을 지연시킬 필요가 있었다. 따라서, Provider를 이용해 호출을 지연시켜 객체 비활성화 오류를 예방할 수 있다. 그러나 **프록시 객체를 사용하면** 가짜 객체를 미리 스프링 컨테이너에 등록해두고, 해당 객체가 필요한 시점에 가짜 객체가 진짜 객체를 호출하여 의존관계 주입 및 초기화, 메서드 사용을 가능하게 해주기 때문에 Provider를 통해 **생성지연을 사용할 필요가 없다.**)
    
- 사실 Provider를 사용하든, 프록시를 사용하든 핵심 아이디어는 진짜 객체 조회를 꼭 필요한 시점까지 지연처리 한다는 점이다.
- 단지 애노테이션 설정 병경만으로 원본 객체를 프록시 객체로 대체할 수 있다. 이것이 바로 다형성과 DI 컨테이너가 가진 큰 장점이다.
- 꼭 웹 스코프가 아니어도 프록시는 사용할 수 있다.

**주의점**

- 마치 싱글톤 사용하는 것 같지만 다르게 동작하기 때문에 결국 주의해서 사용해야 한다.
- 이런 특별한 scope는 꼭 필요한 곳에만 최소화해서 사용하자. 무분별하게 사용하면 유지보수하기 어려워진다.