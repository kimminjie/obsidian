**모듈**
서브 모듈을 담고 있는 게 모듈
**모듈**= 서버, 서비스 (=레고)
**서브 모듈**= 서버나 서비스 안에 만든 거

**AOP (Aspect Oriented Programming)** = “관점 지향 프로그래밍”
-공통된 관심사(공통 기능) 를 핵심 로직과 분리해서 관리하는 프로그래밍 방식
==파이프라인(OOP)를 세로로 연결하는 걸 AOP => @Aspect 사용==

|            |                                             |
| ---------- | ------------------------------------------- |
| Aspect     | 공통 기능(예: 로깅, 보안 등)을 정의한 모듈                  |
| Join Point | 공통 기능을 적용할 수 있는 지점 (메서드 실행 시점 등)            |
| Advice     | 실제로 수행되는 공통 기능 코드 (before, after, around 등) |
| Pointcut   | 어떤 메서드에 공통 기능을 적용할지 지정하는 표현식                |
| Weaving    | 공통 기능(Aspect)을 핵심 코드에 연결하는 과정               |
https://x.com/lab_zang/status/1988434068825141593 (<- X 참고)

Aspect= 가만히 있다가 관심사가 생기면 움직임

Join Point 실행 가능한 지점 (메서드 호출, 예외 발생 등)
→ 예: public void save(User user) 호출 순간

Pointcut Join Point 중 실제 관심 있는 지점만 필터링
→ 예: @annotation(RequestMapping) 붙은 메서드만

Advice 실제 수행할 코드 (관심사 로직)
→ 예: “로깅”, “시간 측정”, “트랜잭션 시작/종료”

Aspect
Pointcut + Advice 의 조합/ Weaving Aspect를 실제 코드 실행 시점에 주입하는 과정
→ Spring에서는 프록시(Proxy) 기반으로 Weaving 수행