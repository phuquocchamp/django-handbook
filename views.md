### VIEWS TRONG DJANGO DRF



**Class-Based Views (CBVs) và Function-Based Views (FBVs) trong Django:**

Trong Django, có hai cách chính để xây dựng views: Class-Based Views (CBVs) và Function-Based Views (FBVs).

#### Function-Based Views (FBVs):

FBVs là cách truyền thống để xây dựng views trong Django. Chúng là các hàm Python đơn giản nhận request và trả về response.

**Ví dụ về FBV:**

```python
# views.py
from django.shortcuts import render
from django.http import HttpResponse

def hello_world(request):
    return HttpResponse("Hello, World!")

def greeting(request, name):
    return HttpResponse(f"Hello, {name}!")
```

**URL Configuration cho FBVs:**

```python
# urls.py
from django.urls import path
from .views import hello_world, greeting

urlpatterns = [
    path('hello/', hello_world, name='hello_world'),
    path('greet/<str:name>/', greeting, name='greeting'),
]
```

#### Class-Based Views (CBVs):

CBVs là một cách hiện đại và mạnh mẽ hơn để xây dựng views. Chúng sử dụng các lớp Python để định nghĩa các loại views khác nhau.

**Ví dụ về CBV:**

```python
# views.py
from django.views import View
from django.http import HttpResponse

class HelloWorldView(View):
    def get(self, request):
        return HttpResponse("Hello, World!")

class GreetingView(View):
    def get(self, request, name):
        return HttpResponse(f"Hello, {name}!")
```

**URL Configuration cho CBVs:**

```python
# urls.py
from django.urls import path
from .views import HelloWorldView, GreetingView

urlpatterns = [
    path('hello/', HelloWorldView.as_view(), name='hello_world'),
    path('greet/<str:name>/', GreetingView.as_view(), name='greeting'),
]
```

#### Sự khác biệt giữa FBVs và CBVs:

1. **FBVs:**

   - Dễ đọc và hiểu với cú pháp hàm.
   - Phù hợp cho các views đơn giản.
2. **CBVs:**

   - Cung cấp một cấu trúc tổ chức tốt hơn với việc sử dụng các lớp.
   - Cho phép tái sử dụng code dễ dàng hơn với việc kế thừa và mixins.
   - Có các methods như `get()`, `post()`,... để xử lý từng loại request.

![1700467100979](image/views/1700467100979.png)
