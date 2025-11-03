**PS->도커 안 컨테이너**: ==docker exec -it aiion(정한 이름) bash==
ㄴ> 나올 땐: ==exit==
**컨테이너->도커 안 db**: ==psql -U aiion(정한 이름) -d aidb(정한 db 비번)==
ㄴ> 나올 때: ==\(역슬래시)q==

**PS에서 컨테이너 상태 확인**-> ==docker ps==
**db에서 테이블 상태 확인**-> ==\(역슬래시)d==
**db에서 데이터 확인**-> ==select * from test;==

**데이터 확인 명령어들**
SELECT * FROM test;                    # 전체 레코드 수  
select * from test; LIMIT 5;                   # 처음 5개 레코드 조회  
select * from test;                           # 모든 데이터 조회  
\d test                                       # 테이블 구조 확인

**삭제 명령어**
DROP TABLE test;                  -테이블 자체를 삭제
DELETE FROM test;                -테이블 안에 모든 데이터 삭제(안전한 삭제)
TRUNCATE TABLE test;            -테이블 안에 모든 데이터 삭제(빠름)
DELETE FROM test WHERE date = '==????==';  -특정 데이터만 삭제