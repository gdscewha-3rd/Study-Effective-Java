## 아이템 15. 클래스와 멤버의 접근 권한을 최소화하라

### 정보 은닉의 장점
- 시스템 개발 속도를 높일 수 있다.
   - 여러 컴포넌트를 병렬로 개발할 수 있기 때문
- 시스템 관리 비용을 낮춘다.
   - 각 컴포넌트를 더 빨리 파악하여 디버깅할 수 있고, 다른 컴포넌트로 교체하는 부담이 적어지므로
- 정보 은닉 자체가 성능을 높여주지는 않지만, 성능 최적화에 도움을 준다. 
   - 완성된 시스템을 프로파일링해 최적화할 컴포넌트를 정한 다음 다른 컴포넌트에 영향을 주지 않고 해당 컴포넌트만 최적화할 수 있기 때문
- 소프트웨어 재사용성을 증대시킨다. 
   - 외부에 의존하지 않고 독자적으로 동작할 수 있는 컴포넌트라면 그 컴포넌트와 함께 개발되지 않은 낯선 환경에서도 유용하게 쓰일 가능성이 크기 때문
- 큰 시스템을 제작하는 난이도를 낮춰준다. 
   - 시스템 전체가 아직 완성되지 않은 상태에서도 개별 컴포넌트의 동작을 검증할 수 있기 때문이다.

### 접근 제한자
- private : 멤버를 선언한 톱레벨 클래스에서만 접근할 수 있다.
- package-private : 멤버가 소속된 패키지 안의 모든 클래스에서 접근할 수 있다. 접근 제한자를 명시하지 않았을 때 적용되는 패키지 접근 수준(단, 인터페이스의 멤버는 기본적으로 public이 적용).
- protected : package-private의 접근 범위를 포함하며, 이 멤버를 선언한 클래스의 하위 클래스에서도 접근할 수 있다.
- public : 모든 곳에서 접근할 수 있다.

**접근 제한자의 기본 원칙은 모든 클래스와 멤버의 접근성을 가능한 한 좁혀야한다.**

-> 클래스의 공개 API를 제외한 모든 멤버는 private로 만든다.

-> 그 후 오직 같은 패키지의 다른 클래스가 접근해야 하는 멤버에 한하여 package-private로 풀어주자.


#### 상위 클래스의 메서드를 재정의할 때는 그 접근 수준을 상위 클래스에서보다 더 좁게 설정할 수 없다.

#### public 클래스의 인스턴스 필드는 되도록 public이 아니어야 합니다.
- 필드가 가변 객체를 참조하거나, final이 아닌 인스턴스 필드를 public으로 선언하면 그 필드와 관련된 모든 것은 불변식을 보장할 수 없게 된다.

#### 클래스에서 public static final 배열 필드를 두거나 이 필드를 반환하는 접근자 메서드를 제공해서는 안 된다.
- 이런 필드나 접근자를 제공한다면 클라이언트에서 그 배열의 내용을 수정할 수 있게 된다.


