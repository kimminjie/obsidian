w3schools.com/sql (참고)

|     | SQL                                | 자바     |                                                                                                                               |
| --- | ---------------------------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------- |
| 3   | select                             | return |                                                                                                                               |
| 1   | from                               | import | 자바는 맥락을 위에서 부터 읽는데 패키지는 껍데기고 대문자가 있는 임폴트부터 맥락을 읽음                                                                             |
| 2   | where                              | if 스위치 |                                                                                                                               |
| (4) | oder by --asc(오름)/desc(내림)         |        |                                                                                                                               |
|     | and                                |        | 조건이 2개 이상                                                                                                                     |
|     | in                                 |        | 조건이 3개 이상                                                                                                                     |
|     | between 0 and 0                    |        | 범위                                                                                                                            |
|     | as(aliases) 생략가능           (as 빼기) |        | 별칭 select CustomerID ==as== ID from Customers;-> select CustomerID ID from Customers;-> select c.CustomerID from c.Customers; |
|     | join                               |        | 결합, 테이블 2개를 결합                                                                                                                |
|     | union                              |        | select가 2개                                                                                                                    |
|     | group by                           |        | 속성에 대한 결합                                                                                                                     |
|     | having                             |        | group by로 묶은 그룹에 **조건**을 거는 절                                                                                                 |
|     | on                                 |        | 같은 컬럼이 있을 때 연결해줌                                                                                                              |
|     | view(생략)                           |        | 표로 예쁘게 보임(가짜)/ 진짜는 테이블                                                                                                        |
|     | using                              |        | on 대신 두 번 쓰지 않고 유징으로 합치기                                                                                                      |
![[Pasted image 20251029120849.png]]
ㄴ>테이블 이름 소문자로 생략(지저분하게 쌓이는 거 안 좋아함)

**join** ㄱ
![[Pasted image 20251029121438.png]] **self**는 join 안 씀. (self는 나를 복제.지만 하나 외에 모두 다름/self는 특이한 사항)
**left 와 right**는 같은 거 보통 레프트를 씀.
**inner**는 교집합-많이 씀(압도적으로 많기 때문에 그냥 join만 쓰면 이너)
**full**은 웬만하면 쓰지 않음(욕먹음)

**left join**은 빈 자리가 얼마나 남았는지 같은 거 알아야 할 때 사용

---
**SQL 서브쿼리 종류**
1. 스칼라 서브쿼리  (select 뒤에 ())
2. 인라인 뷰          (from 뒤에 ())
3. (중첩)서브쿼리   (where 뒤에 ()) -- 이걸 해야함

![[Pasted image 20251029160203.png]]
조인 쓸지 서브쿼리 쓸지 고민해야 함.

 LIKE는  '고%' 느림
 =은 정확한 이름. 더 빠름

as 뒤는 스키마에 찍히는 이름

is NULL or p.position = '' 써주면 오류방지