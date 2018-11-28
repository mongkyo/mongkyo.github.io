choices 옵션

직렬화, 역직렬화
render 변환기 
parser 해석기
serializer = SnippetSerializer(data=data)
역 직렬화에서는 data=data로 

직렬화에서는 그냥 instance를 넣어준다.(data)


https://meetup.toast.com/posts/92


DB에 변화를 주면 POST요청



요청으로부터 username, password를 받음
받은 내용으로 authenticate를 실행

인증 성공시 

이증된 User에 연결되는 Token을 가져오거나 생성(get_or_create)
Token의 key값을 Response