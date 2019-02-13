# Form 종류

## 일반폼
+ Form 클래스를 상속받아 정의
```
예)
class PostForm(forms.Form)
    album = forms.ModelChoiceField(queryset=Album.objects.all())
    title = forms.CharField(max_length=50
    image = ImageField()
    ...............
```    
## 모델 폼
+ ModelForm 클래스를 상속받아 정의
+ 폼 필드의 구성을 데이터베이스 모델 정의를 베이스로 폼을 정의하는 경우 사용
+ modelform_factory() 함수를 사용해 모델 폼을 정의할 수도 있음
```
예)
class PostForm(forms.ModelForm):
    class Meta:
        model = Photo
        fields = ['title', 'image', 'description']
        #fields = '__all__'     # 모든 필드 사용
        .............
```    
## 폼셋
+ 일반 폼을 여러개 묶어서 한 번에 보여주는 폼
+ modelformset_factory() 함수를 사용해 모델 폼셋을 정의함


## 인라인 폼셋
+ 두 모델 간의 관계까 1:N 인 경우 
+ N 모델에 기초해 만든 모델 폼을 여러개 묶은 폼셋
+ Inlineformset_factory() 함수를 사용해 인라인 폼셋을 정의

```
예) 
# views.py
class AlbumPhotoCV(LoginRequireMixin, CreateView):
    model = Album
    fields = ['name', 'description']
    
# forms.py
PhotoInlineFormSet = inlineformset_factory(Album, Photo,
    fields = ['image', 'title', 'description'],
    extra = 2)   
```      
        
        
## modelform_factory 함수방식
+ model을 베이스로 ModelForm 클래스를 만들어 리턴
    
```
예)
PhotoForm = modelform_factory(Photo, fields='__all__')
```        
+ fields
    + 리턴하는 ModelForm에 포함될 필드를 지정
    + 지정안하면 model의 필드를 사용함
+ exclude
    + 리턴하는 ModelForm에 제외할 필드를 지정
+ widgets
    + 모델 필드와 위젯을 매핑한 사전
+ formfield_callback
    + 모델의 필드를 받아서 폼 필드를 리턴하는 콜백 함수를 지정
+ localized_fields
    + 로컬 지역값이 필요한 필드를 리스트로 지정
+ labels
    + 모델 필드와 레이블을 매핑한 사전
+ help_texts
    + 모델 필드와 설명 문구를 매핑한 사전
+ error_messages
    + 모델 필드와 에러 메시지를 매핑한 사전    

## formset_factory 함수방식    
```
예) 
PostSearchFormSet = formset_factory(PostSearchForm)
```    
+ form
    + 폼셋을 만들때 베이스가 되는 폼을 지정
+ formset
    + 폼셋을 만들 때 상속받기 위한 부모 클래스를 지정
    + 보통은 BaseFormSet 클래스를 변경 없이 사용
    + 변경이 필요하면 BaseFormSet 클래스를 오버라이딩해 기능을 변경한 후 사용할수 있음
+ extra
    + 폼셋을 보여줄 때 빈 폼을 몇 개 포함할지를 지정
    + 디폴트는 한개
+ can_order
    + 폼셋에 포함된 폼들의 순서를 변경할 수 있는지 여부를 불린으로 지정
+ can_delete
    + 폼셋에 포함된 폼들의 일부를 삭제할 수 있는지 여부를 불린으로 지정
+ max_num
    + 폼셋을 보여줄 때 포함될 폼의 최대 개수를 지정
    + 디폴트는 None으로 지정되는데 1000개를 의미
+ validate_max
    + 폼셋에 대한 유효성 검사를 수행할 때, max_num에 대한 검사도 실시
    + 삭제 표시가된 폼을 제외한 폼의 개수가 max_num보다 작거나 같아야 유효성 검사를 통과함
+ min_num
    + 폼셋을 보여줄 때 포함될 폼의 최소 개수를 지정함
+ validate_min
    + 폼셋에 대한 유효성 검사를 수행할 때 min_num에 대한 검사도 실시함
    + 삭제 표시가된 폼을 제외한 폼의 개수가 min_num보다 크거나 같아야 유효성 검사를 통과함                                
                        
    
## 제네릭뷰에서 폼 정의
+ CreateView, UpdateView뷰는 ModelForm의 기능을 내부에 포함하고 있음
    + 따로 폼 안만들어 줘도 됨
```
예)
class PhotoCreateView(CreateView):
    model = Photo
    fields = '__all__'
    
class PhotoUpdateView(UpdateView):
    model = Photo
    fields = '__all__'    
```    

## 파일 업로드 폼
+ 폼을 정의할때 FileField 또는 ImageField 필드가 들어 있으면 주의
+ 유의해야할 2가지
 
+ 첫번째 `<form>` 요소의 인코딩 속성을 멀티파트로 지정
```
예)
<form enctype="multipart/form-data" method="post" action="/foo/">
```
+ 두번째 폼에 데이터를 바운드할 때 파일명뿐만 아니라 파일 데이터도 같이 바운드해야함
```
예) 웹 요청 데이터로 폼을 바운드하는 경우
f = ContactFormWithMugshot(request.POST, request.FILES)

# 메소드를 사용해 멀티파트 폼인지 확인
f = ContactFormWithMugshot()
f.is_multipart()
True
```

```
예) html
{% if form.is_multipart %}
    <form enctype="multipart/form-data" method="post" action="/foo/">
{% else %}
    <form method="post" action="/foo/">
{% endif %}
{{ form }}        
```