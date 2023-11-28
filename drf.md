### DJANGO REST FRAMEWORK

#### Một số nội dung quan trọng trong DRF

##### Các thuộc tính của Request.

- request.data : dữ liệu của request body (Trong postman).
- request.query_params: các tham số truyền từ request get
- request.method: trả về phương thức (chữ in hoa) thực hiện request (GET, POST, PUT, DELETE, PATCH).
- request.content_type: trả về chuôĩ đại diện cho media type cửa request body.
- request.user : các user đã chứng thực thì nó trả về thể hiện của `django.contrib.auth.models.User`. Ngược lại, nó là thể hiện của `django.contrib.auth.models.AnonymousUser`
- request.auth : chứa thông tin bổ sung chức thực, nó phụ thuojc cách chứng thực, nhưng nó thường chứa thông tin token cửa request.
- request.authenticators: các chính sác chứng thực, trong các lớp APIVIew hoặc @api_view thuộc tính được thiết lập thông qua thuộc tính authentication_classes hoặc giá trị thuộc tính cấu hình DEFAULT_AUTHENTICATORS

##### Các thụôc tính Response

- REST Framework cung cấp lớp Response trả về nội dung với nhiều dạng `media type` phụ thuộc vào client request. Lớp Response là lớp con cửa Django SimpleTemplateResponse.
- Cú pháp tạo response:

```python

Response(data, status=None, template_name = None, headers=None, content_type=None)
```

+ data : dữ liệu đã được serialize.
+ status : status code cho response, mặc định là 200
+ headers : từ điển HTTP cho reponse.
+ content_type : content type cho response.
+ template_name: tên template sử dụng nếu HTMLRenderer được chọn.

##### Các thuộc tính Serializer

- Serializer cho phép chuyển các dữ liệu phức tạp như `querysets` hoặc thể hiện của model thành các kiểu dữ liệu JSON, XML hay một content type nào khác.
- Nó cũng cung cấp khẳ năng deserializer để chuyển dữ liệu đã được chuyển về kiểu dữ liêu phức tạp ban đầu sau khi đã kiêm tra (validate).
