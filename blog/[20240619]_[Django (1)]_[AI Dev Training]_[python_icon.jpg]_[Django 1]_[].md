# Django
반드시 장고절차.md 참고

```python
장고 디자인 패턴
(model => 데이터베이스 => models.py)
(template => 화면단 => index.html)
(view => 컨트롤러 / 비즈니스 로직 => views.py)
```

## 기본 작업
```python 
# 프로젝트 시작 명령
django-admin startproject djangoweb

# 실행준비 및 실행
make migrations
python manage.py migrate
python manage.py runserver

# 장고에서 다룰 파일 흐름
1. urls.py
2. views
3. model
4. template


# 웹앱 시작명령
django-admin startapp blog

=> app은 urls 파일을 만들어주지 않는다. (web의 urls과 통합관리가 가능하기 때문에.)
=> 그러나 일반적으로 web(project)과 app의 url은 분리해서 관리하므로 별도 생성

* 많이 실수하는 부분
=> 앱을 만들고 나서 프로젝트의 settings.py에 앱을 추가한 사실을 알려야한다. (settings.py => ISNTALLED_APPS)
=> project의 (urls.py => urlpattern)에 경로추가
```


## 웹앱 추가 후 프로젝트 파일 수정
```python
=> 프로젝트 settings.py에서 일부 요소 추가
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "blog",
    "furni",
]

=> urls.py에 경로추가
urlpatterns = [
    # 프로젝트의 urls 경로 뒤에 / 붙인다
    # 앱의 urls 경로 뒤에 / 안붙인다
    path("admin/", admin.site.urls),
    path("blog/", include("blog.urls")),
    path("furni/", include("furni.urls")),
    path('api-auth/', include('rest_framework.urls'))
]
```

## 웹앱 파일 수정 (urls.py)
```python 
urlpatterns = [
    # 프로젝트의 urls 경로 뒤에 / 붙인다
    # 앱의 urls 경로 뒤에 / 안붙인다
    # http://localhost:8000/blog/create
    path("create", views.create),
    path("create_insert", views.create_insert)
]
```

## 웹앱 파일 수정 (views.py)
```python
def create(requests):
    return render(requests, "blog_create.html")
    
    
def create_insert(requests):
    if requests.method == "POST":
        name = requests.POST['name']
        email = requests.POST['email']
        password = requests.POST['password']
        phone_number = requests.POST['phone_number']
    
    user = User(name=name, email=email, password=password, phone_number=phone_number)
    user.save()
    
    context = {"name":name,
               "email":email,
               "password":password,
               "phone_number":phone_number}
    return render(requests, "create_insert.html", context=context)

```

## 웹앱 파일 수정 (templates)
```html
<!-- blog_create.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1> 블로그에기록할내용 </h1>
    <form method="POST" action="create_insert">
        {% csrf_token %}
        <input type="text" name="name"/>
        <input type="text" name="email"/>
        <input type="text" name="password"/>
        <input type="text" name="phone_number"/>
        <input type="submit"/>

    </form>
</body>
</html>

```


```html
<!-- blog_create.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1> 당신이 전달한 값은 {{ name }} </h1>
    <h1> 당신이 전달한 값은 {{ email }} </h1>
    <h1> 당신이 전달한 값은 {{ password }} </h1>
    <h1> 당신이 전달한 값은 {{ phone_number }} </h1>
    <h1> 잘인서트되었습니다. </h1>
    
</body>
</html>
```

## 웹앱 파일 수정 (models.py)
```python
from django.db import models

# Create your models here.
class User(models.Model):
    id = models.AutoField(primary_key=True)
    name = models.CharField(max_length=30)    
    email = models.CharField(max_length=100)
    password = models.CharField(max_length=100)
    phone_number = models.CharField(max_length=100)
    created_at = models.DateTimeField (auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    class Meta:
        db_table = "users"

```

# 기타
```python
"furni"의 경우
https://themewagon.com/
에서 다운받은 웹 템플릿
```
