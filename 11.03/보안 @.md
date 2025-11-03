컬럼명(그래들)과 필드명(자바)이 다를 땐 @Column을 써줘야함.
-@컬럼 안 써주는 게 좋음. 

@NoArgsConstructor는
하나씩 만드는 거 (제조 커피)
@AllArgsConstructor는
이미 만들어져 있는 거 (캔 커피)

나중에 추가할 거(사용자가 뭘 필요할 지 모를 때)엔 세터가 필요
-@Setter가 없으면 @AllArgsConstructor가 필요없음

properties보다 yml이 나음