https://github.com/bobby-didcoding/drf_course/tree/main/steps

https://viblo.asia/p/xay-dung-api-voi-django-rest-framework-Do754PXJ5M6#_27-test-api-13

[/toc/] 
{{toc}} 
[[__TOC__]] 
[toc]

## Setting
```python
INSTALLED_APPS = [
'rest_framework',
'rest_framework.authtoken',

'corsheaders',
'djoser',
]
```

```python
MIDDLEWARE = [
...
'django.contrib.auth.middleware.AuthenticationMiddleware',
'corsheaders.middleware.CorsMiddleware',
]
```

```python
CORS_ALLOWED_ORIGINS = [
    "http://localhost:8080",
]
```
## Model and serializer
product/models.py
```python
from io import BytesIO
from PIL import Image

from django.core.files import File
from django.db import models

class Category(models.Model):
    name = models.CharField(max_length=255)
    slug = models.SlugField()

    class Meta:
        ordering = ('name',)

    def __str__(self):
        return self.name

    def get_absolute_url(self):
        return f'/{self.slug}/'
        
class Product(models.Model):
    category = models.ForeignKey(Category, related_name='products', on_delete=models.CASCADE)
...
...
```

product/serializers.py
```python
from rest_framework import serializers
from .models import Category, Product


class CategorySerializer(serializers.ModelSerializer):

    products = ProductSerializer(many=True)
    class Meta:
        model = Category
        fields = (
            "id",
            "name",
            "get_absolute_url",
            "products",
        )
...
...
```

### class meta
https://www.geeksforgeeks.org/meta-class-in-models-django/
**Model Meta** is basically the inner class of your model class. Model Meta is basically used to change the behavior of your model fields like changing order options,verbose_name, and a lot of other options.


## Test

product/tests.py
```python
from .models import Category, Product

from django.urls import reverse
from rest_framework.test import APITestCase
from rest_framework.test import APIClient
from rest_framework import status
from django.test import TestCase

class ProductApiTestCase(APITestCase):

    def setUp(self):
        testCategory = Category(name="Test Category")
        testCategory.save()
        testProduct = Product(name="Test Product",price =19.99, category = testCategory)
        testProduct.save()
        
        self.client = APIClient()

    def test_models_setup(self):
        self.assertEqual(Product.objects.get().name, "Test Product")
        self.assertEqual(Category.objects.get().name, "Test Category")
        self.assertEqual(Product.objects.count(), 1)
        self.assertEqual(Category.objects.count(), 1)

    def test_get_latest_products(self):
        # response = self.client.get("http://127.0.0.1:8000/api/v1/latest-products/")
        response = self.client.get(reverse('latest-products'))
        self.assertEqual(response.status_code, status.HTTP_200_OK)
        
```



## Url và View
