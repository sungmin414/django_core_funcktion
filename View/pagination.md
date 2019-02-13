## 제네릭 뷰의 페이징 처리

+ 페이지별로 보여주는 기능( 페이징, 페이지네이션이라함 )
+ 클래스형 뷰나 함수형 뷰에서도 사용가능

## 페이징 기능 활성화

+ ListView 처럼 객체 리스트를 처리하는 제네릭뷰는 `paginate_by(페이지당 객체의 개수를 의미)` 속성을 가짐
+ 몇 번째 페이지를 화면에 보여줄지는 웹 요청 URL에서부터 지정되고 이를 뷰에서 처리
+ URL페이지를 지정하는 방법은 2가지 아래 예제 살펴보기

```
예) 첫번째 방법 = URL 경로에 페이지 번호를 지정하고 URLconf에서 이를 추출해 뷰에 넘겨주는 방법
path('objects/page(<page>)/, PaginatedView.as_view())

# URL이 /objects/page3/ 이라면 클래스형 뷰 PaginatedView에 페이지 번호 3을 page파라미터로 넘겨줌
```
```
예) 두번째 방법 = URL 쿼리 문자열에 페이지 번호를 지정하는 방법
/objects/?page=3

# 쿼리 문자열의 page 파라미터에 페이지 번호를 지정하고 뷰가 직접 request.GET.get('page')와 같은
구문으로 페이지 번호를 추출
# page라는 파라미터 이름을 변경할수 있는데 변경한 경우에는 뷰에 page_kwarg속성으로 알려줘야함
```

+ URL에 페이지 번호가 지정되면 뷰는 페이징 처리를 하고 나서 그 다음 화면에 보여주는데 필요한
컨텍스트 변수를 템플릿에 넘겨줌

## 컨텍스트 변수
+ object_list
    + 화면에 보여줄 객체의 리스트, context_object_name 속성으로 지정된 컨텍스트 변수도
    object_list와 동일한 값을 갖는다
+ is_paginated
    + 출력 결과가 페이징 처리되는지 여부를 알려주는 불린 변수
    + 페이지 크기가 지정되지 않거나 대상 객체 리스트가 페이지로 구분되지 않는 경우는 값이 False가됨
+ paginator
    + django.core.paginator.Paginator 클래스의 객체
    + 페이징 처리가 안되는 경우에 이값을 None으로 세팅됨
+page_obj
    + django.core.paginator.Page 클래스의 객체
    + 페이징 처리가 안되는 경우에 이값은 None로 세팅됨
    
> 페이징 기능을 사용하기 위해서는 제네릭 뷰에서 paginate_by 속성을 지정하고, 원하는 페이지를
URL에 지정하고 템플릿 파일에서 컨텍스트 변수만 적절히 사용하면 됨

            