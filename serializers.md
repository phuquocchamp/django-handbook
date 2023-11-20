### SERIALIZERS TRONG DJANGO DRF

![1700447059587](image/README/1700447059587.png)

![1700447222262](image/README/1700447222262.png)

Trong Django Rest Framework (DRF), serializer là một thành phần quan trọng giúp chuyển đổi dữ liệu từ và đến các định dạng khác nhau, như JSON. Serializer giúp kiểm tra và xác thực dữ liệu đầu vào, cũng như chuyển đổi đối tượng Python thành dữ liệu có thể được truyền đi hoặc lưu trữ.

Dưới đây là một ví dụ cơ bản về cách sử dụng serializer trong Django Rest Framework:

#### 1. Serializers Example

1. **Tạo một model:**

```python
# models.py
from django.db import models

class Book(models.Model):
    title = models.CharField(max_length=100)
    author = models.CharField(max_length=50)
    publication_date = models.DateField()
```

2. **Tạo một serializer cho model trên:**

```python
# serializers.py
from rest_framework import serializers
from .models import Book

class BookSerializer(serializers.ModelSerializer):
    class Meta:
        model = Book
        fields = ['id', 'title', 'author', 'publication_date']
```

Serializer này sử dụng `ModelSerializer` từ DRF để tự động sinh ra các trường dựa trên model `Book`. Bạn có thể tùy chỉnh các trường hoặc thêm các trường mới theo nhu cầu.

3. **Sử dụng serializer trong views hoặc API views:**

```python
# views.py
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from .models import Book
from .serializers import BookSerializer

class BookList(APIView):
    def get(self, request):
        books = Book.objects.all()
        serializer = BookSerializer(books, many=True)
        return Response(serializer.data)

    def post(self, request):
        serializer = BookSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

Trong ví dụ trên, `BookList` là một API view sử dụng serializer để xử lý dữ liệu. Phương thức `get` sử dụng serializer để chuyển đổi danh sách sách thành dữ liệu có thể truyền đi, trong khi phương thức `post` sử dụng serializer để xác thực và lưu dữ liệu mới.

4. **Đăng ký URL cho views:**

```python
# urls.py
from django.urls import path
from .views import BookList

urlpatterns = [
    path('books/', BookList.as_view(), name='book-list'),
]
```

Trên đây là một ví dụ đơn giản về cách sử dụng serializer trong Django Rest Framework. Serializer đóng vai trò quan trọng trong việc xử lý và biến đổi dữ liệu trong ứng dụng Django của bạn.

#### 2. Type of Serializers

##### 1. Serializer.Serializers

Tham khảo:

- [Serializers vs ModelSerializers](https://www.youtube.com/watch?v=3-w6TNbGP3Y)

**Serializer:**

`Serializer` là một lớp chung cho phép bạn kiểm soát chặt chẽ hơn việc định nghĩa các trường và logic. Bạn sẽ cần mô tả cụ thể từng trường và xử lý logic kiểm tra dữ liệu.

1. **Tạo Serializer:**

```python
# serializers.py
from rest_framework import serializers

class CustomSerializer(serializers.Serializer):
    name = serializers.CharField(max_length=50)
    age = serializers.IntegerField()

    def validate_age(self, value):
        if value < 0:
            raise serializers.ValidationError("Age must be a positive number.")
        return value
```

2. **Sử dụng Serializer trong views:**

```python
# views.py
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from .serializers import CustomSerializer

class CustomAPIView(APIView):
    def post(self, request, *args, **kwargs):
        serializer = CustomSerializer(data=request.data)
        if serializer.is_valid():
            # Xử lý logic khi dữ liệu hợp lệ
            return Response(serializer.validated_data, status=status.HTTP_201_CREATED)
        else:
            # Trả về lỗi nếu dữ liệu không hợp lệ
            return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

Trong `CustomSerializer`, bạn phải mô tả từng trường và có thể xử lý logic kiểm tra dữ liệu trong các phương thức như `validate_age`.

##### 2. serializers.ModelSerializers

**ModelSerializer:**

`ModelSerializer` là một lớp con của `Serializer` được tối ưu hóa cho việc tương tác với các model trong Django. Nó giảm bớt công việc lặp lại khi bạn chỉ cần tạo một serializer dựa trên model.

1. **Tạo Model:**

```python
# models.py
from django.db import models

class Book(models.Model):
    title = models.CharField(max_length=100)
    author = models.CharField(max_length=50)
    publication_date = models.DateField()
```

2. **Tạo ModelSerializer:**

```python
# serializers.py
from rest_framework import serializers
from .models import Book

class BookSerializer(serializers.ModelSerializer):
    class Meta:
        model = Book
        fields = ['id', 'title', 'author', 'publication_date']
```

3. **Sử dụng ModelSerializer trong views:**

```python
# views.py
from rest_framework.generics import ListAPIView
from .models import Book
from .serializers import BookSerializer

class BookListView(ListAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
```

Với `ModelSerializer`, bạn không cần phải tự định nghĩa từng trường, nó tự động tạo các trường dựa trên model.

##### 3. Sự khác nhau và các sử dụng ?

`Serializers` và `ModelSerializers` là hai loại serializer trong Django Rest Framework (DRF), và lựa chọn giữa chúng phụ thuộc vào nhu cầu cụ thể của bạn.

1. **Serializers:**

   - **Khi sử dụng với dữ liệu không phải là model:** Sử dụng `Serializers` khi bạn đang làm việc với dữ liệu không phải là model Django. ***Ví dụ, khi bạn có dữ liệu từ nguồn không phải là cơ sở dữ liệu hoặc từ một nguồn dữ liệu khác.***
   - **Khi cần kiểm soát linh hoạt hơn đối với quá trình chuyển đổi dữ liệu:** `Serializers` cho phép bạn kiểm soát linh hoạt hơn quá trình chuyển đổi dữ liệu, đặc biệt là khi bạn muốn thêm logic xử lý đặc biệt.
   - **Khi muốn tùy chỉnh cách dữ liệu được trả về:** Nếu bạn muốn tùy chỉnh cách dữ liệu được trả về trong response, bạn có thể sử dụng `Serializers` để tạo ra một định dạng dữ liệu tuỳ chỉnh.

   ```python
   from rest_framework import serializers

   class MySerializer(serializers.Serializer):
       field1 = serializers.CharField()
       field2 = serializers.IntegerField()
   ```
2. **ModelSerializers:**

   - **Khi làm việc với model Django:** Sử dụng `ModelSerializers` khi bạn làm việc với các model Django. `ModelSerializers` tự động tạo ra một serializer dựa trên các trường của model, giúp giảm bớt code lặp và tăng khả năng bảo trì.
   - **Khi muốn thao tác CRUD cơ bản với model:** `ModelSerializers` cung cấp các phương thức CRUD cơ bản như tạo, đọc, cập nhật và xóa dữ liệu mà không cần triển khai lại nhiều mã.
   - **Khi muốn đơn giản hóa code:** `ModelSerializers` giúp đơn giản hóa quá trình tạo serializer cho các model bằng cách tự động xác định trường và các quy tắc chuyển đổi.

   ```python
   from rest_framework import serializers
   from .models import MyModel

   class MyModelSerializer(serializers.ModelSerializer):
       class Meta:
           model = MyModel
           fields = '__all__'
   ```
   Trong nhiều trường hợp, bạn có thể bắt đầu với `ModelSerializers` vì chúng giúp giảm bớt công việc lập trình và giúp bạn nhanh chóng tạo ra một API CRUD hoạt động cho model của mình. Tuy nhiên, khi bạn cần linh hoạt lớn hơn và muốn kiểm soát đầy đủ hơn quá trình chuyển đổi dữ liệu, bạn có thể quay trở lại sử dụng `Serializers`.
