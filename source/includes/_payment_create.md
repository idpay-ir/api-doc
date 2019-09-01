# ایجاد تراکنش

با استفاده از آدرس زیر می‌توانید یک تراکنش جدید ایجاد کنید.

```shell
curl -X POST https://api.idpay.ir/v1.1/payment \
  -H 'Content-Type: application/json' \
  -H 'X-API-KEY: 6a7f99eb-7c20-4412-a972-6dfb7cd253a4' \
  -H 'X-SANDBOX: 1' \
  -d '{
  "order_id": 101,
  "amount": 10000,
  "name": "قاسم رادمان",
  "phone": "09382198592",
  "mail": "my@site.com",
  "desc": "توضیحات پرداخت کننده",
  "callback": "https://example.com/callback",
  "reseller": null
}'
```

```php
<?php
$params = array(
  'order_id' => '101',
  'amount' => 10000,
  'name' => 'قاسم رادمان',
  'phone' => '09382198592',
  'mail' => 'my@site.com',
  'desc' => 'توضیحات پرداخت کننده',
  'callback' => 'https://example.com/callback',
  'reseller' => null,
);

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, 'https://api.idpay.ir/v1.1/payment');
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($params));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
  'Content-Type: application/json',
  'X-API-KEY: 6a7f99eb-7c20-4412-a972-6dfb7cd253a4',
  'X-SANDBOX: 1'
));

$result = curl_exec($ch);
curl_close($ch);

var_dump($result);
```

```javascript
var request = require('request');

var options = {
  method: 'POST',
  url: 'https://api.idpay.ir/v1.1/payment',
  headers: {
    'Content-Type': 'application/json',
    'X-API-KEY': '6a7f99eb-7c20-4412-a972-6dfb7cd253a4',
    'X-SANDBOX': 1,
  },
  body: {
    'order_id': '101',
    'amount': 10000,
    'name': 'قاسم رادمان',
    'phone': '09382198592',
    'mail': 'my@site.com',
    'desc': 'توضیحات پرداخت کننده',
    'callback': 'https://example.com/callback',
    'reseller': null,
  },
  json: true,
};

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```

```go
url := "https://api.idpay.ir/v1.1/payment"

data := map[string]string{
  "order_id": "101",
  "amount":   "10000",
  "name":     "قاسم رادمان",
  "phone":    "09382198592",
  "mail":     "my@site.com",
  "desc":     "توضیحات پرداخت کننده",
  "callback": "https://example.com/callback",
  "reseller": "null",
}

payload, _ := json.Marshal(data)

req, _ := http.NewRequest("POST", url, bytes.NewBuffer(payload))

req.Header.Set("Content-Type", "application/json")
req.Header.Set("X-API-KEY", "6a7f99eb-7c20-4412-a972-6dfb7cd253a4")
req.Header.Set("X-SANDBOX", 1)

res, _ := http.DefaultClient.Do(req)

defer res.Body.Close()
body, _ := ioutil.ReadAll(res.Body)

fmt.Println(string(body))
```

**آدرس درخواست**

`POST https://api.idpay.ir/v1.1/payment`

**پارامترهای مورد نیاز**

پارامتر | نوع | ضروری | توضیحات
------- | --- | ----- | -------
order_id | string | بله | شماره سفارش پذیرنده<br/>به طول حداکثر 50 کاراکتر
amount | number | بله | مبلغ مورد نظر به ریال<br/>مبلغ باید بین 1,000 ریال تا 500,000,000 ریال باشد
name | string | خیر | نام پرداخت کننده<br/>به طول حداکثر 255 کاراکتر
phone | string | خیر | تلفن همراه پرداخت کننده<br/>به طول 11 کاراکتر<br/>مثل 9382198592 یا 09382198592 یا 989382198592
mail | string | خیر | پست الکترونیک پرداخت کننده<br/>به طول حداکثر 255 کاراکتر
desc | string | خیر | توضیح تراکنش<br/>به طول حداکثر 255 کاراکتر
callback | string | بله | آدرس بازگشت به سایت پذیرنده<br/>به طول حداکثر 2048 کاراکتر
reseller | number | خیر | شناسه نمایندگی<br>جهت کسب اطلاعات بیشتر با پشتیبانی تماس بگیرید 

**وضعیت پاسخ**

کد وضعیت | توضیحات
-------- | -------
201 | تراکنش با موفقیت ایجاد شد
403 | [لیست خطاها](#d7b83cfb9c)
405 | [لیست خطاها](#d7b83cfb9c)
406 | [لیست خطاها](#d7b83cfb9c)

> وضعیت 200: با اجرای دستور بالا پاسخی مشابه متن زیر با فرمت JSON دریافت می‌شود:

```json
{
  "id": "d2e353189823079e1e4181772cff5292",
  "link": "https://idpay.ir/p/ws-sandbox/d2e353189823079e1e4181772cff5292"
}
```

> وضعیت 406: در صورت بروز خطا، پاسخی مشابه متن زیر با فرمت JSON دریافت می‌شود:

```json
{
  "error_code": 32,
  "error_message": "شماره سفارش `order_id` نباید خالی باشد."
}
```

**پاسخ**

درصورتیکه درخواست موفق باشد، وضعیت پاسخ `201 Created` اعلام خواهد شد.

در پاسخ، مقادیر `id` و `link` باز میگردد که بهتر است آنها را در دیتابیس خود ذخیره کنید.
بعد از ذخیره اطلاعات دریافتی، پرداخت کننده باید به لینک دریافت شده منتقل شود.

پارامتر | نوع | توضیحات
------- | --- | -------
id | string | کلید منحصر بفرد تراکنش
link | string | لینک پرداخت برای انتقال خریدار به درگاه پرداخت

<aside class="notice">
درصورتیکه هر یک از مقادیر <code>name</code> یا <code>phone</code> یا <code>mail</code>
معتبر نباشند، خطایی باز نمی‌گردد و هیچ مقداری برای آن ذخیره نمی‌شود.
</aside>

<aside class="notice">
دامنه آدرس بازگشت به سایت پذیرنده یا <code>callback</code> باید مطابق با آدرسی باشد که در <a href="https://idpay.ir/dashboard/web-services">وب سرویس‌های من</a> تعریف شده است.
</aside>
