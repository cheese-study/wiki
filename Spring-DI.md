스프링에서는 의존성 주입 하는 방법이 크게 3가지가 있다. 각 방법의 소개와 장단점을 소개 해보겠다.

## IoC 구현 방법

### DL
* 저장소에 저장되어 있는 빈(Bean)에 접근하기 위하여 개발자들이 컨테이너에서 제공하는 API 를 이용하여 사용하고자 하는 빈(Bean) 을 Lookup 하는 것


### DI
* 각 계층 사이, 각 클래스 사이에 필요로하는 의존 관계를 컨테이너가 자동으로 연결해주는것
* 각 클래스의 사이의 의존 관계를 빈 설정 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것

## @Autowired Injection

```java
@Service
@Transactional
public class AccountService {
    @Autowired
    private AccountRepository accountRepository;
}
```

* `@Autowired` 어노테이션을 사용해서 의존성 주입을 한다.
* **순환 참조 의존성을 방지 할 수 있다.**
* POJO 스럽지 않고 테스트 코드 작성시 의존성 주입을 변경해서 진행하기 힘들다

## Setter Injection
```java
@Service
@Transactional
public class AccountService {
    private AccountRepository accountRepository;

    @Autowired
    public void setAccountRepository(AccountRepository accountRepository) {
        this.accountRepository = accountRepository;
    }
}
```

* 인자가 없는 생성자나 인자가 없는 static factory 메서드가 bean을 인스턴스화 하기 위하여 호출된 bean 의 setter 메서드를 호출하여 실체화하는 방법
* 세터를 생성 후 의존성 산입 방식이기에 구현시에 조금더 유연할 수 있다.
* 순환 참조 의존성을 발생할 수 있다.
* **immutable 객체가 아니기 때문에 실수할 수 있는 확률이 높다.**

## Constructor Injection

```java
@Service
@Transactional
public class AccountService {
    private final AccountRepository accountRepository;

    public AccountService(AccountRepository accountRepository) {
        this.accountRepository = accountRepository;
    }
}
```
* 생성자를 이용하여 클래스 사이의 의존 관계를 연결
* 생성자 파라미터를 지정함으로써 생성하고자하는 객체가 불필요 하는 것을 명확하게 알 수 있다.
* **Setter 메서드를 제공하지 않음으로 간단하게 필드를 불변 값으로 지정할 수 있다.**
* **가장 POJO 스럽다**
  * `final` 키워드로 해당 객체가 반드시 외부에서 주입 받는다는 것을 잘표시해준다
* 코드량이 많다
  * Lombok의 `@RequiredArgsConstructor`등을 활용하면 해결 가능