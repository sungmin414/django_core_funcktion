## 모델 간 관계
+ 테이블 간에는 관계를 맺을 수 있으며, 장고는 테이블 간의 관계를 3가지로 분류해 제공
+ 1:N(one-to-many), N:N(many-to-money), 1:1(one-to-one) 관계

`2가지 유념해 공부하기`
1. 관계라는 것은 양방향 개념이므로, 양쪽 모델에서 정의가 필요한 게 원칙이지만, 장고에서는 한쪽 클래스에서만 
관계를 정의하면 이를 바탕으로 상대편 정의는 자동으로 정의해줌
( `개발자는 한쪽 클래스에서 관계를 정의했다면 반대 방향의 정의는 명시적으로 보이지 않더라도 이해할 수 있어야함`)

2. 한쪽 방향으로 관계를 생성하거나 변경하면, 반대 방향으로의 관계도 그에 따라 변한다는 것

## 1:N (One-to-Many) 관계
+ 테이블간 1:N 관계를 맺기 위해서는 모델의 필드를 정의할 때 `ForeignKey`필드 타입을 사용
+ `ForeignKey`필드 타입은 필수 인자로 관계를 맺고자 하는 모델 클래스를 지정해야 함
+ N 모델에서 `ForeignKey`필드를 정의하면서, `ForeignKey`필드 필수 인자로 1 모델을 지정하는 방식

## N:N (Many-to-Many) 관계
+ 테이블간 N:N 관계를 맺기 위해서는 모델의 필드를 정의할 때 `ManyToManyField`필드 타입을 사용함
+ ManyToManyField 필드 타입또 ForeignKey 필드와 마찬가지로 관계를 맺고자 하는 모델 클래스를 필수 인자로 지정
+ ManyToManyField 필드 정의는 두모델 중에서 어느 쪽이라도 가능하지만 `한쪽에만 정의`해야지 양쪽에 정의하면 안됨.

## 1:1 (One-to-One) 관계
+ 테이블간 1:1 관계를 맺기 위해서는 모델의 필드를 정의할 때 `OneToOneField`필드 타입을 사용
+ OneToOneField 필드 타입도 ForeignKey 필드와 마찬가지로 관계를 맺고자 하는 모델 클래스를 필수 인자로 지정
+ OneToOneField필드 타입은 ForeignKey 필드 타입에 unique=True 옵션을 준 것과 유사( `반대 방향의 동작은 다름` )
+ ForeignKey 관계에서의 반대 방향의 객체는 복수 개의 객체를 반환하지만, One-to-OneField 관계에서 반대 방향의 객체는 하나의
객체만을 반환하는 점이 다름.


## 관계 매니저
+ Manager란?
    + Manager 클래스는 데이터베이스에 대한 처리, 즉 데이터베이스에 쿼리를 보내고 그 응답을 받는 역활
+ 매니저 중에서 모델 간 관계에 대한 기능 및 데이터베이스 쿼리를 담당하는 클래스를 관계 매니저(`Related Manager`)라고 함.
    + 모델간 관계를 다루기위한 클래스

## 관계 매니저 클래스를 사용하는 경우
+ `1:1 관계에서는 관계 매니저를 사용하지 않음`
    + 관계 매니저 클래스는 객체들의 집합을 다루기 위한 클래스인데 1:1 관계에서는 상대 객체가 하나뿐이기 때문.
   
> 1:N 관계 설명

+ User:Album 모델의 예를 들면, 1에서 N방향으로 엑세스하는 경우에 관계 매니저를 사용
+ N에서 1 방향으로 액세스하는 경우 ForeignKey로 정의한 필드명을 사용해 액세스하면되고 대상 객체가 하나뿐이므로, 관계 매니저는 불필요 
```
예)
user1.alum_set          # album_set은 관계 매니저 클래스의 객체

album.owner             # owner는 ForeignKey 타입의 필드명
```

> N:N 관계 설명

+ Album, Publication 모델의 예를 들면, 양쪽 방향으로 모두 관계 매니저 클래스를 사용
+ 양족 방향으로 모두 대상 객체가 복수 개이므로 관계 매니저 클래스를 사용하는 것
```
album1.publication_set          # publication_set은 관계 매니저 클래스의 객체임

publication1.albums             # albums는 ManyToMantField 타입의 필드명이면서, 관계 맨니저 객체임
```


## 관계 매니저 메소드

>### add(*objs)
+ 인자로 주어진 모델 객체들을 관계 객체의 집합에 추가한다.
+ 두 모델 객체 간에 관계를 맺어주는 역할을 함
```
예)
b = Blog.objects.get(id=1)
e = Entry.objects.get(id=234)
b.entry_set.add(e)      # Entry e 객체를 Blog b 객체에 연결함
```    
+ ForeignKey 관계에서 관계 매니저는 자동으로 e.save()를 호출해서 데이터베이스 업데이트를 수행
+ N:N 관계에서 add() 메소드를 사용하는 경우, save() 메소드를 호출하는 대신 QuerySet.bulk_create()메소드를 호출해
관계를 생성. `bulk_create()메소드는 한번의 쿼리로 여러 개의 객체를 데이터베이스에 삽입`


>###create(**kwargs)
+ 새로운 객체를 생성해서 이를 데이터베이스에 저장하고 관계 객체 집합에 넣는다.
+ 새로 생성된 객체를 반환, 상대방 모델 객체를 새로 만들어서 두 객체 간에 관계를 맺어줌

```
예)
b = Blog.objects.get(id=1)
e = b.entry_set.create(
    headline="Hello",
    body_text='hi',
    pub_date=datetime.date(2019, 1, 1)
)
```
+ create() 메소드를 사용하면 add() 메소드처럼 e.save() 메소드를 호출하지 않아도 자동으로 데이터베이스에 저장됨.
+ create() 메소드는 blog 인자를 사용하지 않는다는 점을 유의

```
예)
b = Blog.objects.get(id=1)
e = Entry(
    headline="Hello",
    body_text='hi',
    pub_date=datetime.date(2019, 1, 1)
)
e.save(force_insert=True)       # Entry e 객체에서 Blog b 객체와 관계를 생성함
```

>###remove(*objs)
+ 인자로 지정된 모델 객체들을 관계 객체 집합에서 삭제
+ 두 모델 객체 간의 관계를 해제하는 역할
```
예)
b = Blog.objects.get(id=1)
e = Entry.objects.get(id=234)
b.entry_set.remove(e)       # Entry e 객체에서 Blog b 객체와의 관계를 끊음
```

+ add() 메소드와 비슷하게 remove() 메소드도 업데이트를 위해 자동으로 e.save() 메소드를 호출
+ N:N 관계에서 remove() 메소드를 사용하면 save() 메소드가 아니라 QuerySet.delete() 메소드를 호출해 관계를 삭제
+ 위 예제를 보면 b.entry_set에서 e 객체를 삭제하는 것은 e.blog=None (None은 NULL과 동일한 의미) 문장을 실행하는 것과 동일
+ ForeignKey 관계에서는 null=True 인 경우만 이메소드를 사용할 수 있음
+ 이 메소드는 `bulk` 인자를 가질수 있는데 bulk 인자에 따라 실행 방법이 달라짐
    + bulk인자가 True(디폴트 값)이면 QuerySet.update() 메소드가 사용됨.
    + 만일 False이면 모델 객체마다 save() 메소드를 호출
    
>###clear()
+ 관계 객체 집합에 있는 모든 객체를 삭제
+ 해당 모델 객체가 맺고 있는 다른 객체들과의 관계를 모두 제거
```
예)
b = Blog.objects.get(id=1)
b.entry_set.clear()
```

+ remove() 메소드처럼 clear() 메소드도 ForeignKey에 사용될 때는 null=True일 경우만 사용가능하고
또한  bulk 인자에 따라 실행 방법이 달라짐


>remove(), clear()

`remove(), clear() 메소드는 관계가 맺어진 상대 객체를 삭제하는것이 아니고 상대 객체와의 관계만 끊는 것`

> 데이터베이스에 반영

`add(), create(), remove(), clear() 메소드들은 실행 즉시 데이터베이스에 반영, save() 메소드를 호출할 필요가 없음`

+ 메소드를 사용하지 않고 직접 대입하는 방식으로 관계 객체 집합의 내용을 바꿀 수도 있음
+ 방법은 리스트 등과 같은 파이썬 이터러블(iterable) 타입의 객체를, 관계 매니저 객체에 대입하면 됨.

```
예)
new_list = [obj1, obj2, obj3]
e.related_set = new_list
```

```
ForeignKey 관계가 null=True면, 관계 매니저는 먼저 clear() 메소드를 호출해서 관계 객체 집합에 들어 있는 기존 객체들을 삭제한후, 
new_list 내용을 새로 추가함, 만일 null=True가 아니면, 기존 관계 객체 집합에 new_lisst에 들어 있는 객체들을 추가
```