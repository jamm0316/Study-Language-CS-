[<back](https://www.notion.so/basic-_-3ee7f95ac24d43f881b7ee2e8f21c210?pvs=21)

---

<aside>
📃 목차

</aside>

---

## 패키지 - 시작

프로그램이 커지면 기능이 많아져 관리하기 어려움

**아주 작은 프로그램**

```java
Order
User
Product
```

**큰 프로그램**

```java
User
UserManager
UserHistory
Product
ProductCatalog
ProductImage
Order
OrderService
OrderHistory
ShoppingCart
CartItem
Payment
PaymentHistory
Shipment
ShipmentTracker
```

매우 많은 클래스가 등장하면서 관련기능들을 분류해서 관리하고 싶을 것

컴퓨터는 보통 파일을 분류하기 위해 폴더, 디렉토리라는 개념 제공.

자바에서는 이러한 기능을 패키지로 제공

```java
* user
	* User
	* UserManager
	* UserHistory
* product
	* Product
	* ProductCatalog
	* ProductImage
* order
	* Order
	* OrderService
	* OrderHistory
* cart
	* ShoppingCart
	* CartItem
* payment
	* Payment
	* PaymentHistory
* shipping
	* Shipment
	* ShipmentTracker
```

여기서 `user`, `product` 등이 패키지.

해당 패키지 안에 관련된 자바 클래스들을 넣으면 된다.

### 패키지 사용

패키지 위치에 주의!!

**pack.Data**

```java
package pack;

public class Data {
    public Data() {
        System.out.println("패키지 pack Data 생성");
    }
}
```

- 패키지를 사용하는 경우 항상 코드 첫줄에 `package pack`과 같이 패키지 이름을 적어주어야함
- 여기서는 `pack` 패키지에 `Data` 클래스 만듦
- 이후 `Data` 인스턴스가 생성되면 생성자를 통해 정보 출력

**pack.a.User**

```java
package pack.a;

public class User {

    public User() {
        System.out.println("패키지 pack.a 회원 생성");
    }
}
```

- `pack`하위에 `a`라는 패키지 만들기
- `pack.a` 패키지 `User` 클래스 만듦
- 이후 `User` 인스턴스가 생성되면 생성자를 통해 정보 출력

**참고:** 생성자에 `public`을 사용했다. 다른 패키지에서 이 클래스의 생성자를 호출하려면 public을 사용해야 함.

**pack.PackageMain1**

```java
package pack;

public class PackageMain1 {

    public static void main(String[] args) {
        Data data = new Data();
        pack.a.User user = new pack.a.User();
    }
}
```

`pack` 패키지 위치에 `PackageMain1` 클래스를 만듦

**실행 결과**

```java
패키지 pack Data 생성
패키지 pack.a 회원 생성
```

- **사용자와 같은 위치:** `PackageMain1`과 `Data`는 같은 `pack`이라는 피키지에 소속되어 있다.
  이렇게 같은 패키지에 있는 경우 패키지 경로 생략 가능
- **사용자와 다른 위치:** `PackageMain1`과 `User`는 서로 다른 패키지다. 이렇게 패키지가 다르면 `pack.a.User`와 가이 패키지 전체 경로를 포함해서 클래스 적어줘야함.

## 패키지 - import

### import

패키기지가 다르면 전체 경로를 적어줘야했다. (`pack.a.User`) 간단하게 `import`를 통해 사용할 수 있다.

**PackageMain2**

```java
package pack;

import pack.a.User;

public class PackageMain2 {

    public static void main(String[] args) {
        Data data = new Data();
        User user = new User();
    }
}
```

**실행 결과**

```java
패키지 pack Data 생성
패키지 pack.a 회원 생성
```

코드 첫줄에는 `package`를 사용, 다음줄에는 `import` 사용 가능

`import`를 사용하면 다른 패키지에 있는 클래스를 가져와 사용가능

`import`를 사용한 덕분에 코드에서는 패키지 명을 생략하고 클래스 이름만 적을 수 있다.

참고. `import` 시점에 `*(별)`을 사용하면 모든 클래스 포함

**패키지 별(*) 사용**

```java
package pack;

import pack.a.*;  //pack.a의 모든 클래스 사용

public class PackageMain2 {

    public static void main(String[] args) {
        Data data = new Data();
        User user = new User();  // import 사용으로 패키지 명 생략 가능
    }
}
```

### 클래스 이름 중복

패키지 덕분에 클래스 이름이 같아도 패키지 이름으로 구분해서 같은 클래스 이름을 사용할 수 있다.

```java
pack.a.User
pack.b.User
```

이런 경우 클래스 이름이 둘다 `User`이지만 패키지 이름으로 대상 구분 할 수 있다.

그러나 둘다 `Main`에서 `import`시키는 경우 어떻게 클래스를 사용할 수 있을까?

**pack.b.User**

```java
package pack.b;

public class User {
    public User() {
        System.out.println("패키지 pack.b 회원 생성");
    }
}
```

**PackageMain3**

```java
package pack;

import pack.a.User;

public class PackageMain3 {

    public static void main(String[] args) {
        User userA = new User();
        pack.b.User userB = new pack.b.User();
    }
}
```

**실행결과**

```java
패키지 pack.a 회원 생성
패키지 pack.b 회원 생성
```

보통 자주 쓰는 이름의 패키지를 `import`하고 모든 패키지의 주소를 적어서 사용할 수 도 있다.

## 패키지 규칙

### 패키지 규칙

- 패키지의 이름과 위치는 폴더(디렉토리) 위치와 같아야한다. (필수)
- 패키지 이름은 모두 소문자를 사용.(관례)
- 패키지 이름의 앞 부분에는 일반적으로 회사의 도메인 이름을 거꾸로 사용한다.
    - ex) `com.company.myapp` 과 같이 사용한다(관례)
    - 이 부분은 필수가 아니다. 그러나 수 많은 외부 라이브러리가 함께 사용되면 같은 패키지에 같은 클래스 이름이 존재 할 수 도 있다. 이렇게 도메인 이름을 거꾸로 사용하면 이런 문제 방지 가능.
    - 내가 오픈소스나 라이브러리 만들어서 외부에 제공한다면 꼭 지키는 것이 좋다.
    - 내가 만든 애플리케이션을 다른 곳에 공유하지 않고, 직접 베포한다면 문제가 되지 않는다.

### 패키지와 계층 구조

패키지는 보통 다음과 같이 계층 구조를 이룬다

- `a`
    - `b`
    - `c`

이렇게 하면 다음과 같이 총 3개의 패키지가 존재

`a`, `a.b`, `a.c`

계층 구조상 a패키지 하위에 `a.b`, `a.c`패키지가 있다.

이것은 우리 눈에 보기에 계층 구조를 이룰 뿐, `a` 패키지와 `a.b`, `a.c` 패키지는 서로 완전히 다른 패키지.

따라서 `a` 패키지 클래스에서 `a.b` 패키지 클래스가 필요하면 `import`해서 사용해야한다.

→ 서로 다른 폴더가 있고 한 a폴더가 b폴더에 종속되어 있다 하더라도 서로 다른 폴더로 취급하는 것과 같음.

참고로 카테고리는 보통 큰 분류에서 세세한 분류로 점점 나어진다.

## 패키지 활용

실제 동작하는 코드는 아니지만, 큰 애플리케이션은 대략 이런식으로 패키지를 구성한다.

정답은 아니고 프로젝트 규모와 아키텍처에 따라 달라진다.

**전체 구조도**

- `com.helloshop` (package)
    - `user` (package)
        - `User` (class)
        - `UserService` (class)
    - `product` (package)
        - `Product` (class)
        - `ProductService` (class)
    - `order` (package)
        - `Order` (class)
        - `OrderService` (class)
        - `OderHistory` (class)

<aside>
📃 **Package com.helloshop.user**

**Class User**

```java
package com.helloshop.user;

public class User {
    String userId;
    String name;
}
```

**Class UserService**

```java
package com.helloshop.user;

public class UserService {
}
```

</aside>

---

<aside>
📃 **Package com.helloshop.product**

**Class Product**

```java
package com.helloshop.product;

public class Product {
    String ProductId;
    int price;
}
```

**Class ProductService**

```java
package com.helloshop.product;

public class ProductService {
}
```

</aside>

---

<aside>
📃 **Package com.helloshop.order**

**Class Order**

```java
package com.helloshop.order;

import com.helloshop.product.Product;
import com.helloshop.user.User;

public class Order {
    User user;
    Product product;

    public Order(User user, Product product) {
        this.user = user;
        this.product = product;
    }
}
```

**Class OrderService**

```java
package com.helloshop.order;

import com.helloshop.product.Product;
import com.helloshop.user.User;

public class OrderService {

    public void order() {
        User user = new User();
        Product product = new Product();
        Order order = new Order(user, product);
    }
}
```

**Class OrderHistory**

```java
package com.helloshop.order;

public class OderHistory {
}
```

</aside>

패키지를 구성할 때 서로 관련된 클래스는 하나의 패키지로 모으고, 관련이 적은 클래스는 다른 패키지로 분리