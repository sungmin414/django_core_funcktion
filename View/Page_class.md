## Page 클래스
+ Paginator 객체에 의해 생성된 단위 페이지를 나타내는 객체로 Page 객체를 생성하는 방법은
생성자 메소드 호출보다는 Paginator.page() 메소드를 호출하는 방법을 더 많이 사용

### 인자(Argument)
+ object_list
    + Paginator 클래스의 object_list 인자와 동일
+ number
    + 몇 번째 페이지인지를 지정하는 페이지 인덱스
+ paginator
    + 페이지를 생성해주는 Paginator 객체


### 메소드(method)
+ Page.has_next()
    + 다음 페이지가 있으면 True반환
+ Page.has_previous()
    + 이전 페이지가 있으면 True반환
+ Page.has_other_pages()
    + 다음 또는 이전 페이지가 있으면 True반환
+ Page.next_page_number()
    + 다음 페이지 번호를 반환
    + 없으면 InvalidPage 익셉션 발생
+ Page.Previous_page_number()
    + 이전 페이지 번호를 반환
    + 없으면 InvalidPage 익셉션 발생
+ Page.start_index()
    + 해당 페이지의 첫 번째 항목의 인덱스를 반환
    + 인덱스는 1부터 카운트
    + 예를 들어 총 5개 항목이 있고 페이지당 2 항목씩 포함된다면 두번쨰 페이지의 start_index()는 3이 된다
+ Page.end_index()
    + 해당 페이지의 마지막 항목의 인덱스를 반환
    + 인덱스는 1부터 카운트
    + 예를 들어 총 5개 항목이 있고 페이지당 2 항목씩 포함된다면 두번쨰 페이지의 end_index()는 4가 된다
    

### 속성(Attribute)
+ Page.object_list
    + 현재 페이지의 객체 리스트
+ Page.number
    + 현재 페이지의 번호
+ Page.paginator
    + 현재 페이지를 생성한 Paginator객체                