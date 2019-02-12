# 속성오버라이딩
## model

+ 기본 뷰(`View, TemplateView, RedirectView`) 3개를 제외하고 모든 제네릭 뷰에서 사용하는 속성
 
## queryset

+ 기본 뷰(`View, TemplateView, RedirectView`) 3개를 제외하고 모든 제네릭 뷰에서 사용하는 속성
+ 출력 대상이 되는 QuerySet 객체를 지정
+ queryset 속성을 지정하면 `model 속성은 무시`

```
예) views.py
class TestPostLV(ListView):
    #model = Post               # 주석 처리
    queryset = Post.objects.all()[:5]
```

## template_name

+ 모든 제네릭뷰에서 사용하는 속성
+ 템플릿 파일명을 문자열로 지정


## context_object_name

+ 기본 뷰(`View, TemplateView, RedirectView`) 3개를 제외하고 모든 제네릭 뷰에서 사용하는 속성
+ 템플릿 파일에서 사용할 컨텍스트 변수명을 지정

## paginate_by

+ ListView와 날짜 기반 뷰에서 사용
+ 페이징 기능이 활성화된 경우에 페이지당 몇개 항목을 출력 할것인지 정수로 지정

## date_field

+ 날짜 기반 뷰에서 기준이 되는 필드를 지정
+ 필드 기준으로 년/월/일을 검사
+ 필드 타입은 `DateField, DateTimeField`이어야 함

## make_object_list

+ YearArchiveView 사용 시 해당 년에 맞는 객체들의 리스트를 생성할지 여부를 지정
+ True면 객체들의 리스트를 만들고 그 리스트를 템플릿에서 사용할 수 있다.
+ False면 queryset 속성에 None이 할당

## form_class

+ FormView, CreateView, UpdateView에서 사용
+ 폼을 만드는데 사용할 클래스를 지정

## initial

+ FormView, CreateView, UpdateView에서 사용
+ 폼에 사용할 초기 데이터를 사전 ({})으로 지정

## fields

+ CreateView, UpdateView에서 사용
+ 폼에 사용할 필드를 지정
+ ModelForm 클래스의 Meta.fields 속성과 동일한 의미

## success_url

+ FormView, CreateView, UpdateView, DeleteView 에서 사용
+ 폼에 대한 처리가 성공한 이후에 리다이렉트될 URL을 지정