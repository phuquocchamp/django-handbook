### RESPONSE & HTTPRESPONSE TRONG DRF


Trong Django Rest Framework (DRF), `Response` là một class được cung cấp để tạo và trả về các phản hồi (response) từ views của bạn. `HttpResponse` là một class tương tự được sử dụng trong Django cơ bản, nhưng trong ngữ cảnh của DRF, `Response` thường được ưu tiên do có nhiều tính năng hữu ích cho việc xây dựng API.

#### Response trong DRF:

`Response` trong DRF cung cấp một cách tiện lợi để tạo và trả về các phản hồi từ views. Nó tự động xử lý việc chuyển đổi đối tượng Python (như danh sách hoặc đối tượng mô hình) thành dạng dữ liệu tương ứng (JSON) để gửi về client.

**Ví dụ sử dụng Response trong DRF:**

```python
from rest_framework.response import Response
from rest_framework.views import APIView

class SampleView(APIView):
    def get(self, request):
        data = {'message': 'Hello, World!'}
        return Response(data)
```

#### HttpResponse trong Django:

`HttpResponse` là một class trong Django cơ bản, thường được sử dụng để trả về các trang web hoặc dữ liệu không phải là JSON. Khi sử dụng `HttpResponse`, bạn phải tự đảm bảo chuyển đổi dữ liệu thành định dạng phù hợp trước khi trả về.

**Ví dụ sử dụng HttpResponse trong Django:**

```python
from django.http import HttpResponse

def sample_view(request):
    data = '<html><body><h1>Hello, World!</h1></body></html>'
    return HttpResponse(data, content_type='text/html')
```

#### So sánh:

- `Response` trong DRF tự động xử lý chuyển đổi dữ liệu thành JSON và cung cấp nhiều tính năng khác, như việc xử lý lỗi và phân trang.
- `HttpResponse` trong Django cơ bản là một phản hồi đơn giản và yêu cầu bạn tự quản lý định dạng dữ liệu.

Trong phần lớn trường hợp khi xây dựng API với DRF, bạn sẽ ưu tiên sử dụng `Response` để có được tính năng cao cấp và đồng nhất cho việc trả về dữ liệu.
