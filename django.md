# Django 정리
## 1. Django 설치
pip install django

## 2. 프로젝트 생성
django-admin startproject 프로젝트이름

## 3. 앱 생성
python manage.py startapp 앱이름

## 4. URL 및 뷰 정의
- urls.py 파일에서 urlpattern[ ]과 해당하는 views를 정의
- views는 요청을 처리하고 응답을 반환 (함수가 정의되어 있는 곳)
  
### 4.1. app 하위 urls.py
- 자동 생성되지 않아 수동으로 파일 생성 필요
- from django.urls import path/ from . import views 작성하여 views.py와 연결
- url 쓸 일이 있으면 '<app_name>:<name>'로 작성하여 추후 변경 시 용이하게 함 
- urlpatterns = [] 의 세번째 인자에 name 지정  
    
    ```
    var_routing 예시) 
    path('<int:pk>/', views.detail, name='detail'),
    ```
 
### 4.2. app 하위 views.py

- from django.shortcuts import render, redirect / from .models import Article 와 같이 작성하여 함수를 정의하는데 모델과 함수 사용
- redirect로 이동하는 url 지정가능
- 특정 pk값에 해당하는 인스턴스를 꺼내기 위해서는 함수를 정의할 때 pk도 인자로 받음

    ```
    예시)
    def update(request, pk):
        article = Article.objects.get(pk = pk)   
        article.title= request.POST['title']
        article.content=request.POST['content']
        article.save()
        return redirect('board:detail', article.pk)
    ```
 
## 5. templates 폴더 하위에 app이름 폴더생성 후 하위에 html 작성
- 만약 겹치는 템플릿이 있다면 프로젝트 하위에 templates 폴더 따로 생성 후 base.html에 내용 넣어놓기
- 이후 사용할 html 파일에서는 아래와 같이 작성하여 base.html 내용을 불러올 수 있음
  
    ```
    {% extends "base.html" %}
    {% block content %}
    <p> 예시 </p>
    {% endblock content %}
    ```

### 5.1. form 

### 5.2. input
- input 태그 안에 value = ""를 작성하여 미리 값 채워 넣어 놓수 있음 (수정시 사용 가능)
- input 태그 안에 required 작성 시 -> 작성해야 제출 가능하게 함  
- input 태그 안에 autocomplete ="off" 작성 시 -> 자동완성 안함 (이전에 작성한 내용 안보이게)
  
### 5.3. textarea
- textarea 태그를 통해 엔터를 입력한 경우 화면에서 br로 표시
- | linebreaksbr -> 엔터친 지점에 들어가서 엔터 나오게 함! 원래는 html에서는 한 칸 띄어쓰기만 됨
  ```
  예시)
  {{article.content | linebreaksbr }}
  ```
- textarea는 input과 다르게 태그 사이에 미리 채워 넣을 값 쓸 수 있음
- textarea는 내용 작성하는 위치 반영 -> 엔터치면 맨 앞과 스페이스만큼 띄어지므로 태그 양옆에 이어붙여서 쓰기

### 5.4. button
- a 태그 안에 button 태그 넣기! 아님 밑줄 나옴
- button 클릭 후 메시지 뜨게 하려면 onclick="return confirm('') 작성 -> javascript 섞어쓴 것
  
    ```
    예시)
    <a href="{% url "board:delete" article.pk %}">
    <button onclick="return confirm('정말 삭제하시겠습니까?')">삭제</button>
    </a>
    ``` 
### 5.5 a href
- app_name 사용시 url 탭해서 "" 안에 쓰기
    ```
    예시)
    <a href="{% url "board:index" %}">Home</a>    
    ``` 
    
## 6. 서버 실행
- python manage.py runserver 명령으로 실행
- http://127.0.0.1:8000/ (localhost:8000 -> 8000은 포트 넘버)로 접속하여 개발 서버가 실행 중인지 확인


## 7. 모델 정의 및 마이그레이션
- 모델은 models.py 파일에서 정의하고, python manage.py makemigrations와 python manage.py migrate 명령어로 데이터베이스에 적용
- 특정 model 확인하고 싶을 시 뒤에 app이름 붙이기
- Class 정의 후 models.CharField(),  models.IntegerField() 등 필드 타입을 사용하여 모델 정의 (참고. text.field()는 내용 길이 무제한으로 작성가능)
  

## ORM (Object-Relational Mapping)
- 모델을 사용하여 데이터베이스와 상호 작용. QuerySet을 사용하여 데이터 검색 및 필터링
- filter(), exclude(), get() 등 다양한 쿼리 메서드 활용


