---
title: "Lombok"
date: 2021-02-06T10:59:21+09:00
draft: true
---

# Lombok 이란?

Column: Jan 30, 2021
Concept: Lombok, Spring-boot

# 🌶️ Lombok 이란 ?

: 웹 애플리케이션에서 VO 객체, getter setter 반복적으로 사용되는 코드를 수정이 편리하도록 자동 처리해 주는 라이버리

- 코딩 과정에서는 annotation 만 보이고 getter/setter 메소드는 보이지 않지만 실제 컴파일된 결과물은 (.class) 에 생성. → 컴파일 과정에서 생성

### 🔹 작동 원리

: annotation이 부여된 java source 를 컴파일 할 때 annotation processor로 등록된 lombokprocessor 가 어노테이션을 확인하고 그에 맞는 메소드를 자동 생성하여 bytecode 로 변환.

**\***️⃣ **[예시]**

**✔️ Before**

: 데이터 베이스와 데이터를 주고받기 위해서 DTO, VO 클래스들을 작성했는데 `반복되는 코드들 존재`

- 각 클래스 entity 마다 getter, setter 생성 + toString 생성

```java
public class CategoryModel {
      private String id;
      private String parentId;
      private String name;
      private Long depthLevel;
      private Long seq;
      private String userYn;

      public CategoryModel() {}

      public CategoryModel(String id, String parentId, String name, Long  depthLevel, Long seq, String userYn) {
            super();
            this.id = id;
            this.parentId = parentId;
            this.name = name;
            this.depthLevel = depthLevel;
            this.seq = seq;
            this.userYn = userYn;
      }

      public String getId() {
            return id;
      }

      public void setId(String id) {
            this.id = id;
      }

      @Override
      public String toString() {
            return "CategoryModel [id=" + id + ", parentId=" + parentId  + ", name=" + name + ", depthLevel=" + depthLevel
                        + ", seq=" + seq + ", userYn=" + userYn + "]";
      }

}
```

**✔️ After**

```java
@Getter
@Setter
@ToString
@NoArgsConstructor
@AllArgsConstructor
public class CategoryModel {
      private String id;
      private String parentId;
      private String name;
      private Long depthLevel;
      private Long seq;
      private String userYn;

}
```

```java
@Data
public class CategoryModel {
      private String id;
      private String parentId;
      private String name;
      private Long depthLevel;
      private Long seq;
      private String userYn;
}
```

⭐ DTO 가 더 명시적이다

⭐ 변수명을 바꾸어줄 경우, 작성했던 다량의 코드들과 달리 수정해줄 필요가 없다.

# 🔹 Lombok 어노테이션 종류

**▶️ @Getter and @Setter**

: getter setter 메소드 자동 생성해주어 반복을 제거

- 클래스 레벨, 필드 레벨 모두 사용 가능

**📍 공통 속성 :**

- value : 접근 제한 가능

- onMethod : method annotation 작성

```java
public class GetSetObject {
		@Getter(value = AccessLevel.PACKAGE,
											onMethod = @__({@NonNull, @Id}))
		private Long id;
}
```

✔️ 위 코드 풀이 :

```java
class GetSetObjectOnMethod {
		private Long id;
		@Id @NonNull
		Long getId() {
				return id;
		}
}
```

**📍 @Getter 'lazy'**

: `true` 일 경우, 무조건 final 필드 ; getName()을 호출해야만 expensive() 호출

: `false` 경우, 객체를 생성할 때 expensive() 호출

```java
@Getter(value = AccessLevel.PUBLIC, lazy = true)
private final String name = expensive();

private String expensive() {
		return "hochoon";
}
```

**📍 @Setter 'onParam'**

```java
@Setter(onParam = @__(@NotNull))
private Long id;

class GetSetObjectOnParam {
		private Long id;

		public void setId(@NotNull Long id) {
				this.id = id;
		}
}
```

**▶️ @ToString**

: toString() 메소드를 생성한다. @ToString(exclude={“제외값”})으로 제외시키고 싶은 값을 설정할 수 있다.

```java
@ToString(exclude = "password")
public class User {
  private Long id;
  private String username;
  private String password;
  private int[] scores;
}
```

```java
User user = new User();
user.setId(1L);
user.setUsername("dale");
user.setUsername("1234");
user.setScores(new int[]{80, 70, 100});
System.out.println(user);
```

**✔️ Result :**

```java
User(id=1, username=1234, scores=[80, 70, 100])
```

**▶️ @EqualsAndHashCode**

: equals() → 동등성 , hashCode() → 동일성 메소드를 생성한다.

- Set으로 중복제거할 때, 유용하게 사용할 수 있습니다.
- of는 해당 field들 중 식별의 대상이 될 필드명을 나열해줍니다.
- exclude는 of와 반대로 식별의 대상에서 제외될 필드명을 나열해줍니다

```java
@EqualsAndHashCode(of = {"firstName", "lastName", "dateOfBirth"})
//@EqualsAndHashCode(exclude = {"address", "city"})
public class Person {
    // ...
}
```

📍callSuper 속성

:`callSuper` 속성을 통해 `equals`와 `hashCode` 메소드 자동 생성 시 부모 클래스의 필드까지 감안할지 안 할지에 대해서 설정할 수 있습니다.

`callSuper = true`로 설정하면 부모 클래스 필드 값들도 동일한지 체크,

`callSuper = false`로 설정(기본값)하면 자신 클래스의 필드 값들만 고려합니다.

```java
@EqualsAndHashCode(callSuper = true)
public class User extends Domain {
  private String username;
  private String password;
}
```

▶️ ⭐⭐ **@Data**

: getter, setter, toString, hashCode, equals, constructor 등을 자동으로 생성

```java
@Data(staticConstructor = "of") //static한 생성자를 만들어주는 속성
public class DataObject {
	private final Long id;
	private String name;
}
```

**▶️ @Data(staticConstructor = "of")**

: static 한 생성자 생성

```java
DataObject dataObjcect = DataObject.of(1L);

id 같은 경우에는 final이라 필수 생성자에 포함되어 있다.
따라서 staticConstructor 속성을 사용한다면 new로 객체를 생성할 수 없다.
즉, DataObject dataObject = new DataObject(); 는 컴파일 오류
```

**▶️ ArgsConstructor**

: 생성자를 생성

```
@NoArgsConstructor : 디폴트 생성자만 만들어준다.
   -매개변수가 없는 기본 생성자 생성

@AllArgsConstructor : 모든 필드의 생성자를 만들어준다.
   -해당 클래스에 모든 필드가 매개변수에 들어간 생성자 생성

@RequiredArgsConstructor : 필수 생성자만 만들어준다.
```

- 속한 속성
  - staticName : @Data의 staticConstructor 속성과 같은 역할을 한다. (=static한 생성자를 만들어준다.)
  - access : 'PUBLIC', 'MODULE', 'PROTECTED', 'PACKAGE', 'PRIVATE' 의 값으로 접근 제한자를 설정할 수 있다.
  - onConstructor : 생성자에 어노테이션을 작성할 수 있다.

```java
@Data
**@NoArgsConstructor
@AllArgsConstructor**
@ToString (exclude = {"orderGroup", "item"})
@Builder
@Accessors(chain = true)
public class OrderDetail {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String status;
    private LocalDateTime arrival_date;
}
```

**▶️ @Cleanup**

: try-with-resource 구문과 비슷한 효과를 가진다. 구문이 종료될 때 AutoCloseable 인터페이스의 close()가 호출되는 try-with-resource와 달리 Scope가 종료될 때 close()가 호출된다.

✔️ Try-with-resource

```java
// try 구문이 종료될때 scanner.close()가 호출됨.
try(val scanner = new Scanner(System.in)) {
  val value = scanner.nextLine();
  System.out.println(value);
}
```

✔️ @Cleanup

```java
//이 메소드가 종료될 때 scanner.close()가 호출된다.
public static void getAndPrint() {
  @Cleanup val scanner = new Scanner(System.in);
  val value = scanner.nextLine();
  System.out.println(value);
}
```

**▶️ @Synchronized**

: 가상의 필드 레벨에서 좀 더 안전하게 락을 걸어줍니다.

- 자바의 `synchronized` 키워드를 메소드에 선언하면 객체 레벨에서 락이 걸려서 여러가지 동기화 관련 문제들이 발생할 수 있습니다

```java
@Synchronized
public void hello() {
    System.out.println("world");
}
```

**▶️ @SneakyThrows**

: Checked Exception 때문에 반드시 `throws나 try-catch 구문`을 통해서 예외 처리를 해줘야할 때 가 있습니다. 이럴 때, `@SneakyThrows` 어노테이션을 사용하면 명시적인 예외 처리를 생략할 수 있습니다.

```java
@SneakyThrows(IOException.class)
public void printLines() {
    BufferedReader reader = new BufferedReader(...);
    for (String line; (line = reader.readLine()) != null; ) {
        System.out.println(line);
    }
}
```

**▶️ @Slf4j**

: 로그 변수 생성

```java
@Slf4j
public class UserApiController implements CrudInterface
														<UserApiRequest, UserApiResponse> {
			public Header<UserApiResponse> create(request) {
			        log.info("{}", request);
			        return userApiLogicService.create(request);
			}
}
```

**▶️ @UtilityClass**

: @UtilityClass 은 기본생성자가 private 으로 생성되며 만약 reflection 혹은 내부에서 생성자를 호출할 경우에는 UnsupportedOperationException 이 발생

```java
@UtilityClass
public class UtilityClassObject {
		public static String name() {
				return "wonwoo";
		}
}
```

✔️ 코드 풀이 :

```java
class UtilityClassObjectNot {
  private UtilityClassObjectNot() {
    throw new
					UnsupportedOperationException();
  }
  public static String name() {
    return "wonwoo;";
  }
}
```

# 🔹 Lombok 의 장점과 단점

### **📌 장점**

- 코드 작성이 쉽고 필요한 코드가 적음

  : 복잡한 코드가 줄어들기 때문에 가독성도 높고 생산성도 높다.

- 코드가 명시적이다
- 수정이 간편하다

### 📌 단점

- 속성의 리네임 리펙토링 시 문제가 될 소지

  : @Data @Setter @Getter

- 상호참조 문제로 인한 무한 루프

  : @ToString 이랑 @EqualsAndHashCode

- Java 를 위한 라이버리인 만큼 언어의 변환하게 되면 Lombok annotation을 쓸수 없다.
- ORM(annotation-based object-related-mapping) 프레임워크 사용시, Lomok 에게 의존하게 되는 data class코드들이 방대해지므로 통제가 어려운 문제가 발생할 수 있다.

### [📌 주의사항](https://kwonnam.pe.kr/wiki/java/lombok/pitfall)

🖇️ [https://kwonnam.pe.kr/wiki/java/lombok/pitfall](https://kwonnam.pe.kr/wiki/java/lombok/pitfall)

- **@Data / @ToString**

  → toString() 메서드는 순환 참조 또는 무한 재귀 호출 문제로 StackOverFlowError 발생 가능

- **@AllArgsConstructor, @RequiredArgsConstructor 사용 주의**

\***\* Example :**

```java
@AllArgsConstructor
public static class Order {
    private long cancelPrice;
    private long orderPrice;
}
```

```java
// 취소금액 5,000원,
// 주문금액 10,000원
Order order = new
Order(5000L, 10000L);
```

💁‍♂️ 만약 개발자가 순서를 바꾸고 되면, 리팩토링은 전혀 작동하지않고 , lombok이 개발자가 인식하기도 전에 생성자 파라미터 순서 필드 선언 순서에 맞춰 순서를 바꾸어 버립니다.

```java
// 주문금액 5,000원, 취소금액 10,000원. 취소금액이 주문금액보다 많아짐!
Order order = new Order(5000L, 10000L); // 인자값의 순서 변경 없음
```

💁‍♂️ 생성자를 직접 만들고 필요한 경우에는 직접 만든 생성자에 `@Builder` 권장. 파라미터 순서가 아닌 이름으로 값을 설정하기 때문에 리팩토링에 유연하게 대응 가능

```java
public static class Order {
    private long cancelPrice;
    private long orderPrice;

    @Builder
    private Order(long cancelPrice, long orderPrice) {
        this.cancelPrice = cancelPrice;
        this.orderPrice = orderPrice;
    }
}

// 필드 순서를 변경해도 문제 없음.
Order order = Order.builder()
.cancelPrice(5000L).orderPrice(10000L).build();
System.out.println(order);
```

👉 필드에 붙은 애노테이션이 생성자 쪽으로 전달 안됨

- **무분별한 @EqualsAndHashCode 사용 자제**

👉 문제 ! : Mutable(변경가능한) 객체에 아무런 파라미터 없이 그냥 사용하는 경우

- Immutable(불변) 클래스를 제외하고는 **아무 파라미터 없는 `@EqualsAndHashCode` 사용은 금지**한다.

- 일반적으로 비교에서 사용하지 않는 Data 성 객체는 `equals` & `hashCode`를 따로 구현하지 않는게 차라리 낫다.

- 항상 `@EqualsAndHashCode(of={“필드명시”})` 형태로 동등성 비교에 필요한 필드를 명시하는 형태로 사용한다.

- 실전에서는 누군가는 이에 대해 실수하기 마련인지라 차라리 사용을 완전히 금지시키고 IDE 자동생성으로 꼭 필요한 필드를 지정하는 것이 나을 수도 있다.

- [Java equals & hashCode](https://kwonnam.pe.kr/wiki/java/equals_hashcode) 좋은 `equals` 만드는 방법

- 막상 개발을 하다보면 온전히 Immutable 필드를 대상으로만 `equals & hashCode`를 만들기는 매우 어렵다. 최소한 꼭 필요하고 일반적으로 변하지 않는 필드에 대해서만 만들도록 노력해야 한다.

- [Equals Verifier](https://kwonnam.pe.kr/wiki/java/equals_verifier)를 통해 `equals, hashCode` 메소드 테스트를 자동으로 할 수 있다.
