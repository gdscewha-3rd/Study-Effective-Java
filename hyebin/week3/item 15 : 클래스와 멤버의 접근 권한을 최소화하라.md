## [아이템 15] 클래스와 멤버의 접근 권한을 최소화하라

**핵심 : public 클래스는 상수용 public static final 필드 이외에는 어떠한 public 필드도 가져서는 안됨** 

### 정보 은닉(캡슐화)

잘 설계된 컴포넌트는 모든 내부 구현을 완벽히 숨겨 구현과 API를 깔끔히 분리한다.

오직 API를 통해서만 다른 컴포넌트와 소통하며 서로의 내부 동작 방식에 영향을 주고받지 않는다. 

이러한 설계를 정보 은닉, 혹은 캡슐화라고 한다.

### 정보 은닉(캡슐화)의 장점

1. 시스템 개발 속도를 높인다. 컴포넌트 병렬 개발이 가능해진다.
2. 시스템 관리 비용을 낮춘다. 컴포넌트를 파악하여 디버깅에 용이하고 다른 컴포넌트로 교체 부담도 적다.
3. 성능 최적화할 컴포넌트만 다른 컴포넌트에 영향 없이 최적화할 수 있기 때문에 결과적으로 성능 최적화에 도움이 된다. 
4. 소프트웨어 재사용성을 높인다. 컴포넌트의 독자적인 사용이 가능해진다.
5. 큰 시스템 제작을 수월하게 한다. 완성 전에도 개별 컴포넌트의 동작을 검증할 수 있기 때문이다.

### 제어 매커니즘 : 접근 제한자

**모든 클래스와 멤버의 접근성을 가능한 한 좁혀야 한다**

- 톱레벨 클래스와 인터페이스
    
    가장 노출을 많이 하게 되는 클래스(인터페이스)이다. 
    
     1) `public` :  공개 API가 되며 하위 호환을 위해 영원히 관리해줘야 한다. 
    
    2) `package - private` : 패키지 외부에서 쓸 이유가 없을 때 선언한다. 클라이언트에 아무런 피해 없이 수정, 교체, 제거할 수 있다. 
    
    3) `private static` : 한 내부 클래스에서만 사용하는 클래스나 인터페이스는 이를 사용하는 클래스 안에서 `private static`으로 중첩시켜 오직 해당 클래스만 접근하도록 한다.
    
    ```java
    class Using { // 여기서만 Used 클래스가 사용된다면 
      ...
      private static class Used{
      }
      ...
    }
    ```
    

- 클래스 멤버
    
    멤버(필드, 메서드, 중첩 클래스, 중첩 인터페이스)에 부여할 수 있는 접근 수준은 네 가지이다.
    
    1) `private` : 멤버를 선언한 톱레벨 클래스에서만 접근 가능
    
    2) `package-private` : 멤버가 소속된 패키지 안의 모든 클래스에서 접근 가능
    
    3) `protected` : package -private 범위 + 이 멤버를 선언한 클래스의 하위 클래스에서도 접근 가능, 여기부터 해당 멤버는 공개 API가 되기 때문에 영원한 지원 필요. 또한 내부 동작 방식을 API 문서에 적어서 공개해야 할 수도 있음 
    
    4) `public` : 모든 곳에서 접근 가능
    
    ### 정보 은닉(캡슐화) 과정
    
    1. 클래스 공개 API를 설계한다.
    2. 그 외의 모든 멤버는 `private`으로 선언한다.
    3. 오직 같은 패키지의 다른 클래스가 접근해야 하는 멤버에 한하여 `private` 제한자를 제거해서 `package -private` (default값, 단 인터페이스 멤버 예외)로 풀어준다.
    4. 만약 제한자를 풀어줘야 하는 일이 자주 생긴다면 컴포넌트를 더 분해해야 하는 것이 아닌지 고민해야 한다.
    
    ### 주의 사항
    
    1. 상위 클래스 메서드 Override
    
    상위 클래스 메서드를 재정의할 때는 그 **접근 수준을 상위 클래스에서보다 좁게 설정할 수 없다.**
    
    1. 테스트를 위한 제한자 수정 
    
    테스트에 필요한 멤버들은 `package - private`하게 설정할 수 있다. 테스트 코드를 테스트 대상과 같은 패키지에 두어야 한다.
    
    1. public 클래스의 인스턴스 필드는 되도록 public이 아니어야 함
    
    필드가 가변 객체를 참조하거나 final이 아닌 인스턴스 필드를 public으로 선언하면 필드에 담을 수 있는 값을 제한할 힘이 없어진다. 또한 필드가 수정될 때 다른 작업을 할 수 없게 되므로 public 가변 필드를 갖는 클래스는 일반적으로 스레드 안전하지 않다. final이고 불변객체를 참조하더라도 리팩터링할 수 없게 된다는 문제가 여전히 있다.
    
     **예외**
    
    해당 클래스가 표현하는 **추상 개념을 완성하는 데 꼭 필요한 구성요소로써의 상수일 경우** `public static final` 선언이 괜찮음
    
    관례상 상수의 이름은 **대문자 알파벳, 각 단어 사이에 밑줄(_)을 넣음**
    
    이 경우 반드시 기본타입이거나 불변객체를 참조해야 한다. 
    
    1. 길이가 0이 아닌 배열은 `static final` 선언해도 변경 가능
    
     배열은 무조건 `private static final` 처리해야 한다. 만약 값을 외부에서 `public` 접근으로 받아야 하는 경우 **public 불변 리스트로 따로 만들어 주거나 방어적 복사로 복사본을 public으로 return 해준다.** 리스트는 `public static final` 선언해도 변하지 않는 듯...?
    
    ### 자바9 부터 제공되는 Export 제한자
    
    모듈은 패키지들의 묶음이다. `export` 제한자는 패키지를 외부 모듈로부터의 공개 여부를 제한한다. 만약 public 제한자를 가진 클래스더라도 export 선언이 되면 해당 패키지가 들어있는 모듈 내에서만 접근이 가능해진다. 
    
    만약 모듈의 JAR 파일을 자신의 모듈 경로가 아닌 애플리케이션의 classpath에 두면 모듈 export 제한자는 모두 무효화되고 모든 패키지는 모듈 외부에서 접근 가능해진다. 
    
