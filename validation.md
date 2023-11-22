### VALIDATION TRONG DRF

Trong Django Rest Framework (DRF), field-level validation có thể được thực hiện trong serializers bằng cách sử dụng các phương thức `validate_<field_name>` trong serializer. Dưới đây là một ví dụ đơn giản:

```python
from rest_framework import serializers

class MyModelSerializer(serializers.Serializer):
    # Các trường trong serializer
    name = serializers.CharField(max_length=100)
    age = serializers.IntegerField()

    def validate_name(self, value):
        # Kiểm tra giá trị của trường 'name'
        if len(value) < 3:
            raise serializers.ValidationError("Tên phải có ít nhất 3 ký tự.")
        return value

    def validate_age(self, value):
        # Kiểm tra giá trị của trường 'age'
        if value < 0:
            raise serializers.ValidationError("Tuổi không thể là số âm.")
        return value
```

Trong ví dụ trên, `validate_name` và `validate_age` là các phương thức kiểm tra hợp lệ cho các trường tương ứng. Nếu kiểm tra không thành công (ví dụ: nếu tên có ít hơn 3 ký tự hoặc tuổi là số âm), chúng ta sẽ ném một `serializers.ValidationError` để thông báo về lỗi.

Ngoài ra, bạn cũng có thể sử dụng một phương thức `validate` để kiểm tra hợp lệ cho nhiều trường cùng một lúc:

```python
from rest_framework import serializers

class MyModelSerializer(serializers.Serializer):
    # Các trường trong serializer
    name = serializers.CharField(max_length=100)
    age = serializers.IntegerField()

    def validate(self, data):
        # Kiểm tra dữ liệu toàn bộ
        name = data.get('name', '')
        age = data.get('age', 0)

        if len(name) < 3:
            raise serializers.ValidationError("Tên phải có ít nhất 3 ký tự.")

        if age < 0:
            raise serializers.ValidationError("Tuổi không thể là số âm.")

        return data
```

Cả hai cách tiếp cận đều cung cấp một cách linh hoạt để thực hiện kiểm tra hợp lệ cho từng trường trong serializers của bạn.
