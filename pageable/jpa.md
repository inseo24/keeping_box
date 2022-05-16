## jpa - pageable

offset 방식이라 데이터가 많아지면 엄청 비효율적이게 된다. 

하지만 데이터가 많지 않으면 그냥저냥 무난히 쓰기 좋음.(mysql이 알잘딱깔센 일을 해주니까)

데이터가 많으면 no offset 방식을 생각하면서 paging 성능 쿼리를 좀 더 고민해보는 걸로 하고

일단 pageable 도 잘 몰라서 발생했던 오늘의 문제


```kotlin
@Transactional(readOnly = true)
override fun getList(userId: Long, pageable: Pageable): PageImpl<PostListResponse> {
    return@getList port.getPostList(userId, pageable).run {
        PageImpl(
            this.content.map { Response(it.id, it.createdAt, ... },
            this.pageable,
            this.totalElements
        )
    }
}
```

대충 위처럼 썼었는데(변수명은 다 변경했지만,,,)

이걸 굳이 이렇게 쓸 필요가 없는게 port.getPostList 여기서 리턴되는 데이터가 Page 였다. 

원래는 querydsl에서 PageImpl로 fetchResults()로 받아서 PageImpl 이었는데 이게 deprecated 되서 나중에 수정하면서 리턴 데이터를 Page로 변경했다.



```kotlin
@Transactional(readOnly = true)
override fun getList(userId: Long, pageable: Pageable): Page<PostListResponse> {
    return@getList port.getPostList(userId, pageable).map {
            Response(it.id, it.createdAt, ... ))
    }
}
```

여튼 그걸 반영해서 Page로 변경도 하고 저기 pageable, totalElements도 굳이 써줄 필요가 없다. 

왜냐면 안에 content만 알아서 convert되서 들어가고 pageable, totalElements도 들어가니까. 

저 위에 코드가 별로인 게 하나 더 있는데 컨버트하면서 계속 PageImpl을 만들어주고 있다.

그럴 필요가 없는데 ㅇ...ㅇ



좀 더 추가적으로 학습해야 할 부분을 찾자면
- Page
- pageable
- PageImpl
- offset vs no-offset

등등이 있겠다. 
