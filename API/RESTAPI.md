# 🌼 REST API

> REST API는 URL을 통해 통신하고 데이터를 받아온다.

## REST API + HTTP methods

- url에 create, update 등 동사를 써서 통신하는 방식은 매번 어떤 동작을 하는지 해당 문서를 찾아봐야하기 때문에 효율적이지 못했다.
- 따라서 동작을 의존하기 위한 HTTP methods와 함께 쓰이게 되었다.
- HTTP methods로는 주로 GET, POST, PUT, DELETE가 쓰인다.

## REST API의 문제점 2가지

### Overfetching

> REST API는 데이터의 사용 여부에 관계없이 모든 데이터를 가져온다.

### Underfetching

> 대상과 관련된 정보가 분산되어 있는 경우 여러 번의 API 요청을 통해 데이터 패칭이 이루어지는 경우도 있다.  
> ex) 영화 리스트와 각 항목별 상세 정보
