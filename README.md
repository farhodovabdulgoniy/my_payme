# my_payme
Payme integration with DjangoRestFramework
# Payme Integration

```
pip install -r requirements.txt
```

###### # settings.py
```
INSTALLED_APPS = [
     ... 
    'paymeuz',
    'rest_framework',
     ...
]
```
**SECRET_KEY** va **ID** PAYME subscribe api dan olinadi

```
PAYME_SETTINGS = {
    'DEBUG':True,   #True - test mode, False - production mode
    'ID':'',  
    'SECRET_KEY':'',
    'ACCOUNTS':{
        'KEY_1':'order_id',
        'KEY_2':'',
    }
}
```
###### # urls.py
```
urlpatterns = [
    ...
    path('api/payme/',include('paymeuz.urls')) #86004564341954340
]

```

# card create
```
POST HTTP/1.1
Host: http://127.0.0.1:8000/api/payme/card/create/
{
    "id": 123,
    "params": {
        "card": { "number": "4444444444444444", "expire": "0420"},
        "amount": 500000, 
        "save": true
    }
}
{
    "id": 123,
    "params": {
        "card": { "number": "8600490417195930", "expire": "1126"},
        "amount": 500000, 
        "save": true
    }
}
```
500000 -> 5000 uzs

# Response
```
{
    "jsonrpc": "2.0",
    "result": {
        "sent": true,
        "phone": "99890*****89",
        "wait": 60000
    },
    "token": "5f460c3d4e6d0841074e7457_YgQMNCttjxKTMKcfN0GPSaaKyV4zPnJqRP4iezHfBGJBpfAyjJf0onx5QXIkmChPDdGJrUpXj2EqWFnTicR4W7p1nXFVvKPegirWSYObyNvrcz18IQbbAVXPTOq1cFQQVrfN1tBM3XdQChu3yr1kTokO7vmeGyCyPZzdO0G4SJeKIwsJiJJk8jvGYpYk0csZh0OhTd01sXIu1qQ4H79qN5vIi5U9rpQcwWra9ueCgJqgU4XgWE2OaGjY4G3qpDHr7ezOUg4Ud3M7S8A1CnsubOD0rhUnOdwWhIU6wuNVJX6xNYD5vjRd4W1StByQeEgIFWHTe4md6nCpSKANPUCH7xnfa3UUu2gz9WJ0PDmOoPwdVo53v9OpQ23kta0sUzMJgSJt"
}
```
# Verify

Token yuqorida kelgan response dan nusxa olib yoziladi yani verify api dagi token bilan card create api dan kelgan respose dagi token bir xil bo'lishi kerak
```
POST HTTP/1.1
Host: http://127.0.0.1:8000/api/payme/card/verify/
{
    "id": 123,
    "params": {
        "token": "5f460c3d4e6d0841074e7457_YgQMNCttjxKTMKcfN0GPSaaKyV4zPnJqRP4iezHfBGJBpfAyjJf0onx5QXIkmChPDdGJrUpXj2EqWFnTicR4W7p1nXFVvKPegirWSYObyNvrcz18IQbbAVXPTOq1cFQQVrfN1tBM3XdQChu3yr1kTokO7vmeGyCyPZzdO0G4SJeKIwsJiJJk8jvGYpYk0csZh0OhTd01sXIu1qQ4H79qN5vIi5U9rpQcwWra9ueCgJqgU4XgWE2OaGjY4G3qpDHr7ezOUg4Ud3M7S8A1CnsubOD0rhUnOdwWhIU6wuNVJX6xNYD5vjRd4W1StByQeEgIFWHTe4md6nCpSKANPUCH7xnfa3UUu2gz9WJ0PDmOoPwdVo53v9OpQ23kta0sUzMJgSJt",
        "code": "666666"
    }
}
```
# verify respose:
```
{
    "jsonrpc": "2.0",
    "id": 123,
    "result": {
        "card": {
            "number": "860006******6311",
            "expire": "03/99",
            "token": "5f460c3d4e6d0841074e7457_YgQMNCttjxKTMKcfN0GPSaaKyV4zPnJqRP4iezHfBGJBpfAyjJf0onx5QXIkmChPDdGJrUpXj2EqWFnTicR4W7p1nXFVvKPegirWSYObyNvrcz18IQbbAVXPTOq1cFQQVrfN1tBM3XdQChu3yr1kTokO7vmeGyCyPZzdO0G4SJeKIwsJiJJk8jvGYpYk0csZh0OhTd01sXIu1qQ4H79qN5vIi5U9rpQcwWra9ueCgJqgU4XgWE2OaGjY4G3qpDHr7ezOUg4Ud3M7S8A1CnsubOD0rhUnOdwWhIU6wuNVJX6xNYD5vjRd4W1StByQeEgIFWHTe4md6nCpSKANPUCH7xnfa3UUu2gz9WJ0PDmOoPwdVo53v9OpQ23kta0sUzMJgSJt",
            "recurrent": true,
            "verify": true,
            "type": "22618"
        }
    }
}
```
# payment request
```
POST HTTP/1.1
Host: http://127.0.0.1:8000/api/payme/payment/
{
    "id": 123,
    "params": {   	"token":"5f460c3d4e6d0841074e7457_YgQMNCttjxKTMKcfN0GPSaaKyV4zPnJqRP4iezHfBGJBpfAyjJf0onx5QXIkmChPDdGJrUpXj2EqWFnTicR4W7p1nXFVvKPegirWSYObyNvrcz18IQbbAVXPTOq1cFQQVrfN1tBM3XdQChu3yr1kTokO7vmeGyCyPZzdO0G4SJeKIwsJiJJk8jvGYpYk0csZh0OhTd01sXIu1qQ4H79qN5vIi5U9rpQcwWra9ueCgJqgU4XgWE2OaGjY4G3qpDHr7ezOUg4Ud3M7S8A1CnsubOD0rhUnOdwWhIU6wuNVJX6xNYD5vjRd4W1StByQeEgIFWHTe4md6nCpSKANPUCH7xnfa3UUu2gz9WJ0PDmOoPwdVo53v9OpQ23kta0sUzMJgSJt",
        "amount": 500000,
        "account": {
            "order_id": 1
        }
    }
}
```
# response:
```
{
  "jsonrpc": "2.0",
  "id": 123,
  "result": {
    "receipt": {
      "_id": "2e0b1bc1f1eb50d487ba268d",
      "create_time": 1481113810044,
      "pay_time": 1481113810265,
      "cancel_time": 0,
      "state": 4,
      "type": 1,
      "external": false,
      "operation": -1,
      "category": null,
      "error": null,
      "description": "",
      "detail": null,
      "amount": 500000,
      "commission": 0,
      "account": [
        {
          "name": "order_id",
          "title": "Код заказа",
          "value": "5"
        }
      ],
      "card": {
        "number": "444444******4444",
        "expire": "0420"
      },
      "merchant": {
        "_id": "100fe486b33784292111b7dc",
        "name": "Online Shop LLC",
        "organization": "ЧП «Online Shop»",
        "address": "",
        "epos": {
          "merchantId": "106600000050000",
          "terminalId": "20660000"
        },
        "date": 1480582278779,
        "logo": null,
        "type": "Shop",
        "terms": null
      },
      "meta": null
    }
  }
}
```

# DONE! ))
