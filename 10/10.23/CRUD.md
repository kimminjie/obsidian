**C R U D** = Create, Read, Update, Delete  
→ 데이터(객체)를 **생성, 조회, 수정, 삭제** 하는 기본 동작
(jpa 책 p.542)

CRUD는 "무엇을 해야 하는가"를 정하고,  
서비스 인터페이스는 "그 일을 어떻게 할 건지 설계하는 곳"

쿼리-find(찾는 기능, 검색)로 시작(jpa책 p.546)
![[Pasted image 20251023152905.png]]
==이제 서비스는 무조건 클래스 말고 인터페이스==
save(@PostMapping), update(@PutMapping), delete(@DeleteMapping), findById(@GetMapping), findAll(@GetMapping)가 기본 CRUD
![[Pasted image 20251023154040.png]]
ㄴ> 각 CRUD에 맞는 매핑

![[Pasted image 20251023154934.png]]
@RequestMapping("/members")를 넣었기 때문에 각 매핑 옆에 /members 지워도 됨.
![[Pasted image 20251023155331.png]]  매핑이 다르기 때문에 뒤에도 지워도 됨.
                   (겟은 2개라 파인드만 지움)
![[Pasted image 20251023160736.png]]
이렇게 매핑 정리   -""사이에 {}는 변수