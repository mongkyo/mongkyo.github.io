404 응답하는 예 중에서 try/except -> DoesNotExist를 사용하는 이유 

Model.objects.get(pk=pk) 해서 가져올때 아예 없는 데이터이거나 두 개 이상인 데이터의 경우 클라이언트 오류로 보여줘야한다. 

하지만 이러한 경우 500 에러를 띄우기 때문에 try/except를 통해 raise http404를 해준다. 

```
tyr:
	item = Item.objects.get(pk=100)
except Item.DoesNotExist:
	raise Http404
```
위의 코드를 줄여서 쓰면

```
item = get_object_or_404(Item, pk=100)
```
