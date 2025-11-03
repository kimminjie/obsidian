**DTO(Data Transfer Object)** 는 개발에서 **데이터를 전달할 때 사용하는 객체**

**VO(Value Object)** 는 “값 자체를 표현하는 객체”

DTO는 “전달용”  
VO는 “의미 있는 값 그 자체”

보안때매 나뉨

![[Pasted image 20251015105546.png]]
private가 없으면 아무나 볼 수 있음
노란 밑 줄=오류 (저 두개가 도메인)
오류 해결 프롬폰트 ㄱ
@LoginDTO.java 여기에 게터와 세터를 작성해줘

![[Pasted image 20251015111317.png]]
get-이메일을 읽는다
set-이메일을 쓴다

LoginController.java ㄱ
![[Pasted image 20251015111906.png]]
![[Pasted image 20251015111921.png]]오류 부분에 마우스오버 (클릭x)-> Quick Fix-> 맨 위

![[Pasted image 20251015113802.png]]
프러퍼티=속성
위에 두 줄 중 이메일은 속성
아래 이메일은 기능
()가 없으면 속성 있으면 기능

LoginDTO loginDTO = ==new== LoginDTO();
저장소 만드는게 new
ㄴ>앞(클래스 이름) 뒤 LoginDTO 같지만 뒤는 ()가 붙으면서 기능화 시킴
