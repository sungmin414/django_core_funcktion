## ArchiveIndexView

+ 제네릭 뷰는 여러 개의 객체를 대상으로 날짜를 기준으로 리스팅해주는 뷰
+ 날짜 기반 제네릭 뷰의 초상위 뷰로써 대상이 되는 모든 객체를 날짜 기준 내림차순으로 보여줌
+ 날짜와 관련된 필드들 중에서 어느 필드를 기준으로 정렬할지 결정하는 `date_field` 속성이 가장 중요

+ 아래 예제 PostAV 뷰 살펴보기
    + `model` 속성 지정
    + 정렬 기준 필드 `date_field` 지정
   
```
예)
class PostAV(ArchiveIndexView):
    model = Post
    date_field = 'modify_date'
```

+ 템플릿에 넘겨주는 컨텍스트 변수 중에서 object_list는 객체들의 리스트를 담고 있고 date_list는 대상
객체들의 연도를 담고 있다

## YearArchiveView

+ 제네릭 뷰는 연도가 주어지면 여러 개의 객체를 대상으로 가능한 월(month)을 알려주는 제네릭 뷰
    + 디폴트 동작은 객체들을 출력해주는 것이 아니고 `객체의 날짜 필드를 조사해 월을 추출해줌`
    + 주어진 연도에 해당하는 객체들을 알고 싶으면 `make_object_list` 속성을 `True`로 지정
    
+ 아래 예제 PostYAV 뷰 살펴보기
    + `model` 속성 지정
    + `date_field` 정렬기준 지정
    + 주어진 연도에 해당하는 객체 `make_object_list` 지정
```
예)
class PostYAV(YearchiveView):
    model = Post
    date_field = 'modify_date'
    make_object_list = True
```

+ YearArchiveView 제네릭 뷰도 인자를 URLconf에서 추출

```
예)
path('<year>/', PostYAV.as_view(), name='post_year_archive')
```

## MonthArchiveView

+ 제네릭 뷰는 주어진 연/월에 해당하는 객체를 보여주는 뷰
    + 연/월 인자는 URLconf에 지정
    + `model, date_field` 속성지정, make_object_list 속성은 없음

+ 아래 예제 PostMAV 뷰 살펴보기

```
예)
class PostMAV(MonthArchiveView):
    model = Post
    date_field = 'modify_date'
```    

+ MonthArchiveView 제네릭 뷰도 인자를 URLconf에서 추출(`연과 월 2개인자 필요`)

```
예)
path('<year>/<month>/', PostMAV.as_view(), name='post_month_archive')
```

## WeekArchiveView

+ 연도와 주(week)가 주어지면 그에 해당하는 객체를 보여주는 제네릭 뷰
    + 연/주 인자는 URLconf에 지정
    + 주 인자는 1년을 주차로 표현하므로 1부터 53까지의 값을 가짐
    + `model, date_field` 속성지정
    
+ 아래 예제 PostWAV 뷰 살펴보기

```
예)
class TestWeekArchiveView(WeekArchiveView):
    model = Post
    date_field = 'modify_date'
```    

+ WeekArchiveView 제네릭 뷰도 인자를 URLconf에서 추출

```
예)
path('<year>/week/<week>/', TestWeekArchiveView.as_view(), name='post_week_archive')
```

## DayArchiveView

+ 연/월/일 주어지면 그에 해당하는 객체를 보여주는 제네릭 뷰
    + 연/월/일 인자는 URLconf에서 지정
    + `model, date_field`

+ 아래 예제 PostDAV 뷰 살펴보기

```
예)
class PostDAV(DayArchiveView):
    model = Post
    date_filed = 'modify_date'
```    

+ DayArchiveView 제네릭 뷰도 인자를 URLconf에서 추출

```
예)
path('<year>/<month>/<day>/', PostDAV.as_view(), name='post_day_archive')
```

## TodayArchiveView

+ 오늘 날짜에 해당하는 객체를 보여주는 제네릭 뷰
    + DayArchiveView 와 동일한 제네릭뷰

+ 아래 예제 PostTAV 뷰 살펴보기

```
예)
class PostTAV(TodayArchiveView):
    model = Post
    date_field = 'modify_date'
```    

+ TodayArchiveView 제네릭뷰는 연/월/일 등의 인자가 `필요하지 않음`

```
예)
path('today/', PostTAV.as_view(), name='post_today_archive')
```

## DateDetailView

+ 날짜 기준으로 특정 객체를 찾아서 그 객체의 상세 정보를 보여주는 뷰
    + 특정 객체의 상세 정보를 보여준다는 점에서 DetailView와 동일하지만 객체를 찾는데 사용하는 인자로
    `연/월/일 정보를 추가적으로 사용하는 점이 다름`
    + 기본키 또는 slug 인자도 사용하므로 연/월/일/pk/ 등 4개 인자가 필요
    + `model, date_field` 

+ 아래 예제 PostDDV 뷰 살펴보기

```
예)
class TestDateDetailView(DateDetailView):
    model = Post
    date_field = 'modify_date'
```        

+ DateDetailView 제네릭뷰는 연/월/일 및 기본 키 또는 슬러그 4개의 인자가 필요

```
예)
path('<year>/<month>/<day>/<pk>/, TestDateDetailView.as_view(), name='post_archive_detail')
```

+ 날짜 기반의 다른 제네릭 뷰들은 복수의 객체들을 출력하함
+ DateDetailView 뷰는 `특정 객체 하나만 다룸`
+ 템플릿에 넘겨주는 컨텍스트 변수는 object_list가 아니라 `object 변수를 사용`, date_list 변수 사용안함

