![alt text](https://i.imgur.com/bmVCvl8.jpg)

[![Downloads](https://pepy.tech/badge/paycomuz)](https://pepy.tech/project/paycomuz)
![alt text](https://img.shields.io/badge/code%20style-black-000000.svg)
[![Downloads](https://img.shields.io/pypi/v/paycomuz)](https://pypi.org/project/PaycomUz)
[![Downloads](https://black.readthedocs.io/en/stable/_static/license.svg)](https://github.com/begyy/PaycomUz/blob/master/LICENSE)

### Requirements
````
pip install django
pip install djangorestframework
pip install PaycomUz 
pip install requests

# supported versions
python 3.5 +
django 2 +
djangorestframework 3.7 +
PaycomUz 2 +
````

**settings.py**

```python
PAYCOM_SETTINGS = {
    "PAYCOM_ENV": False,  # test host
    "TOKEN": "token",  # token
    "SECRET_KEY": "password",  # password
    "ACCOUNTS": {
        "KEY_1": "order_id",
        "KEY_2": None  # or "type"
    }
}

INSTALLED_APPS = [
    'rest_framework',
    'paycomuz',
    ...
]
```

```
python manage.py migrate
```

### Create paycom user
```python
python manage.py create_paycom_user
```

### view.py
```python
from paycomuz.views import MerchantAPIView
from paycomuz.methods_subscribe_api import Paycom
from django.urls import path

class CheckOrder(Paycom):
    def check_order(self, amount, account):
        return self.ORDER_FOUND

class TestView(MerchantAPIView):
    VALIDATE_CLASS = CheckOrder

urlpatterns = [
    path('paycom/', TestView.as_view())
]
```

### create_initialization.py
https://help.paycom.uz/uz/initsializatsiya-platezhey/otpravka-cheka-po-metodu-get
```python
from paycomuz.methods_subscribe_api import Paycom
paycom = Paycom()
url = paycom.create_initialization(amount=5.00, order_id='197', return_url='https://example.com/success/')
print(url)
```
![alt text](https://help.paycom.uz/images/ru/payment_initialization/checkout-get-method-response.png)
