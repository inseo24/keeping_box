### long type validation

javax validation 을 사용할 때, long 타입은 @NotNull 이런 어노테이션은 작동을 안한다.
long 타입은 primitive type 이기 때문에 null 일 수가 없고, request에 null이 넘어오더라도 자동으로 0으로 입력된다.
id를 long으로 받을 때 validation은 양수 값을 받도록 설정하는게 좋지 않을까? 라고 생각해서 @Positive 를 썼다. 
에러가 발생할 경우 http not readable exception 이 발생하는데 이건 exception handler 로 type을 확인하라는 문구로 넘겼다. 근데 좀 더 다양한 경우에 발생할 수 있을 거 같아서 서버에 log를 남기는 설정은 추가하는 게 좋은 것 같다.