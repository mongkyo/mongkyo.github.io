ask django 
                  

1. ordered by 옵션 

정렬 옵션이다. 최신 순으로 보고싶으면 `-`옵션을 준다 


2. 같은 값들로 db를 갱신하려 할 때

1) 

```
qs = Post.objects.all()
for post in qs:
	post.title = 'changed title'
	post.save() 
```

2)

```
qs = Post.objects.all()
qs.update(title= 'changed title')
```

2)번이 성능이 더 좋음. 1번의 경우에는 DB를 계속 참조하여 하나씩 save를 실행하지만 update의 2)번 경우에는 한번에 실행하여 작업을 끝내서 더 효율적이다. 
