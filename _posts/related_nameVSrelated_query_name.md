# related\_name vs related\_query\_name

related\_name과 related\_query\_name에 아무 값도 지정하지 않았을 때의 기본값은 다음과 같다.

- related\_name = 소문자형\_set
- related\_query\_name = 소문자형

Twitter에대한 예제를 통해 알아보자. 

```
from django.db import models
from django.utils import timezone

__all__ = (
    'TwitterUser',
    'Relation',
)


class TwitterUser(models.Model):
    """
    특정 유저가 다른 유저를 (인스턴스 메서드)
        follow  (팔로우하기)
        block   (차단하기)

    중간 모델이 저장하는 정보
        from_user
            어떤 유저가 '만든' 관계인지
        to_user
            관게의 '대상'이 되는 유저
        relation_type
            follow또는 block (팔로우 또는 차단)


    용어 정리
        자신이 follow하는 다른 사람목록
            followers (팔로워 목록)
        자신이 다른사람을 follow한 목록
            following (팔로우 목록)
        자신이 block하는 다른 사람 목록
            block_list
        A가 B를 follow한 경우
            A는 B의 follower  (팔로워)
            B는 A의 following (팔로우)
    """
    
    name = models.CharField(max_length=50)
    relation_users = models.ManyToManyField(
        'self',
        through='Relation',
        symmetrical=False,
    )

    def __str__(self):
        return self.name

    @property
    def followers(self):
        """
        :return: 1) 나를 follow하는 다른 TwitterUser QuerySet
        """
        return TwitterUser.objects.filter(
            from_user_relation__to_user=self,
            from_user_relation__relation_type='f',
        )


    @property
    def following(self):
        """
        :return: 2) 내가 follow하는 다른 TwitterUser QuerySet
        """
        return TwitterUser.objects.filter(
            to_user_relation__from_user=self,
            to_user_relation__relation_type='f',
        )


    @property
    def block_list(self):
        """

        return: 내가 block하는 다른 TwitterUser QuerySet
        """

        return TwitterUser.objects.filter(
            to_user_relation__from_user=self,
            to_user_relation__relation_type='b',
        )

    def follow(self, user):
        """
        user를 follow하는 Relation을 생성
            1. 이미 존재한다면 만들지 않는다
            2. user가 block_list에 속한다면 만들지 않는다
        :param user: TwitterUser
        :return: tuple(Relation instance
        """
        if not self.from_user_relations.filter(to_user=user).exists():
            self.from_user_relations.create(
                to_user=user,
                relation_type='f',
            )
        return self.from_user_relations.get(to_user=user)

    def block(self, user):
        """
        user를 block하는 Relation을 생성
            1. 이미 존재한다면 만들지 않는다
            2. user가 following에 속한다면 해제시키고 만든다
        :param user: TwitterUser
        :return: tuple(Relation instance
        """
        try:
            # Relation이 존재함
            relation = self.from_user_relations.get(to_user=user)
            if relation.reltaion_type == 'f':
                # 근데 following이라면 block으로 바꾸고, 생성일자를 지금 시간으로 변경 후 저장
                relation.relation_type = 'b'
                relation.created_at = timezone.now()
                relation.save()
        except Relation.DoesNotExist:
            # Relation이 없다면 생성 후 생성여부 값에 True 할당
            relation = self.from_user_relations.create(
            		to_user=user, 
            		relation_type='b'
            )
        # Relation인스턴스와 생성여부를 반환
        return relation
    @property
    def follower_relations(self):
        """
        :return: 나를 follow하는 Relation QuerySet
        """
        return self.to_user_relations.filter(relations_type='f')

    @property
    def followee_relations(self):
        """
        :return: 내가 follow하는 Relation QuerySet
        """
        return self.from_user_relations.filter(relations_type='f')


class Relation(models.Model):
    CHOICES_RELATION_TYPE = (
        ('f', 'Follow'),
        ('b', 'Block'),
    )
    from_user = models.ForeignKey(
        TwitterUser,
        on_delete=models.CASCADE,
        related_name='from_user_relations',
        # related_query_name의 기본값
        # 기본값:
        #  이 모델 클래스명의 소문자화
        # related_name이 지정되어 있을 경우:
        #  related_name의 값
        related_query_name='from_user_relation',
    )
    to_user = models.ForeignKey(
        TwitterUser,
        on_delete=models.CASCADE,
        related_name='to_user_relations',
        related_query_name='to_user_relation',
    )
    relation_type = models.CharField(
        choices=CHOICES_RELATION_TYPE,
        max_length=1,
    )
    created_at = models.DateTimeField(auto_now_add=True)

    # 중복을 없앤다, 이미 존재하면 새로 만들수 없게 만든다
    class Meta:
        unique_together = (
            ('from_user', 'to_user'),
        )

```
--
1. 나를 follow하는 다른 TwitterUser QuerySet

		return TwitterUser.objects.filter(
	            from_user_relation__to_user=self,
	            from_user_relation__relation_type='f',
	        )
위의 코드에서 `from_user_relation__to_user=self`를 보면 `from_user`의 값을 가져오는데 `to_user`의 값이 self인 값을 가져온다는 것이다. 그 다음 아래의 조건 `relation_type`이 `'f'`인 `from_user_relation`의 값을 return한다.

2. 내가 follow하는 다른 TwitterUser QuerySet

		return TwitterUser.objects.filter(
	            to_user_relation__from_user=self,
	            to_user_relation__relation_type='f',
	        )
	        
위의 코드에서는 `to_user_relation`의 값을 가져오는데 `from_user`의 값이 self인 값들을 가져온다. 첫 조건을 만족시키는 값들 중 `relation_type`이 `'f'`인 값들의 `to_user_relation`값들을 return해준다.<br>
> 위의 1.문제와 같은 개념이다. from과 to만 바뀌었을 뿐...
