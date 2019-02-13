## Paginator 클래스

+ 페이징 기능의 메인 클래스이며, 주요 역할은 객체의 리스트와 페이지당 항목 수를 필수 인자로 받아서
각 페이지 객체를 생성해주는 것

### 인자(Argument)
+ object_list
    + 페이징 대상이 되는 객체 리스트
    + 객체 리스트는 파이썬의 리스트 또는 튜플 타입이면 가능하고 장고의 QuerySet 객체도 가능
    + 중요한점 ( count(), __len__() 메소드를 갖고 있는 객체여야 함)
+ per_page
    + 페이지당 최대 항목 수
+ orphans
    + 마지막 페이지에 넣을 수 있는 항목의 최소 개수
    + 마지막 페이지의 항목 개수는 orphans보다 커야함
    + 인자는 마지막 페이지의 항목 개수가 너무 적은 경우 그전 페이지에 포함되도록 하기 위해 사용함
    + 예) 총 23개 항목이있고 per_page=10, orphans=3이면, 첫페이지는 10개 항목을
    2번째 페이지는 13개 항목을 갖는다
+ allow_empty_first_page
    + 첫 페이지가 비어 있어도 되는지를 결정하는 불린 타입 인자
    + 항목 개수가 0인경우 인자가 True면 정상 처리하지만 False면 EmptyPage에러 발생

### 메소드(method)
+ Paginator.page(number)
    + Page 객체를 반환함
    + number 인자는 1부터 시작
    + 인자로 주어진 페이지가 존재하지 않으면 InvalidPage 인셉션 발생


### 속성(attribute)
+ Paginator.count
    + 항목의 총 개수
+ Paginator.num_pages
    + 페이지의 총 개수
+ Paginator.page_range
    + 1부터 시작하는 페이지 범위               