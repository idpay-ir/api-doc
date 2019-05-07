# تایید تراکنش

بعد از دریافت اطلاعات به سایت پذیرنده و اعتبار سنجی اطلاعات توسط پذیرنده،
پذیرنده باید تراکنش را تایید کند تا پرداخت بصورت سیستمی تکمیل شود
و از بازگشت پول به پرداخت کننده جلوگیری شود.

```shell
curl -X POST https://api.idpay.ir/v1.1/payment/verify \
  -H 'Content-Type: application/json' \
  -H 'X-API-KEY: 6a7f99eb-7c20-4412-a972-6dfb7cd253a4' \
  -H 'X-SANDBOX: 1' \
  -d '{
  "id": "d2e353189823079e1e4181772cff5292",
  "order_id": "101"
}'
```

```php
<?php
$params = array(
  'id' => 'd2e353189823079e1e4181772cff5292',
  'order_id' => '101',
);

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, 'https://api.idpay.ir/v1.1/payment/verify');
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($params));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
  'Content-Type: application/json',
  'X-API-KEY: 6a7f99eb-7c20-4412-a972-6dfb7cd253a4',
  'X-SANDBOX: 1',
));

$result = curl_exec($ch);
curl_close($ch);

var_dump($result);
```

```javascript
var request = require('request');

var options = {
  method: 'POST',
  url: 'https://api.idpay.ir/v1.1/payment/verify',
  headers: {
    'Content-Type': 'application/json',
    'X-API-KEY': '6a7f99eb-7c20-4412-a972-6dfb7cd253a4',
    'X-SANDBOX': 1,
  },
  body: {
    'id': 'd2e353189823079e1e4181772cff5292',
    'order_id': '101',
  },
  json: true,
};

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```

```go
url := "https://api.idpay.ir/v1.1/payment/verify"

data := map[string]string{
  "id":       "d2e353189823079e1e4181772cff5292",
  "order_id": "101",
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

`POST https://api.idpay.ir/v1.1/payment/verify`

**پارامترهای مورد نیاز**

پارامتر | نوع | ضروری | توضیحات
------- | --- | ----- | -------
id | string | بله | کلید منحصر بفرد تراکنش که در مرحله [ایجاد تراکنش](#2c82b7acb2) دریافت شده است
order_id | string | بله | شماره سفارش پذیرنده که در مرحله [ایجاد تراکنش](#2c82b7acb2) ارسال شده است

**وضعیت پاسخ**

کد وضعیت | توضیحات
-------- | -------
200 | تایید تراکنش به موفقیت انجام شد
400 | [لیست خطاها](#d7b83cfb9c)
403 | [لیست خطاها](#d7b83cfb9c)
405 | [لیست خطاها](#d7b83cfb9c)
406 | [لیست خطاها](#d7b83cfb9c)

> با اجرای دستور بالا پاسخی مشابه متن زیر با فرمت JSON دریافت می‌شود:

```json
{
  "status": "100",
  "track_id": "10012",
  "id": "d2e353189823079e1e4181772cff5292",
  "order_id": "101",
  "amount": "10000",
  "date": "1546288200",
  "payment": {
    "track_id": "888001",
    "amount": "10000",
    "card_no": "123456******1234",
    "hashed_card_no": "e59fa6241c94b8836e3d03120df33e80fd988888bba0a122240c2e7d23b48295",
    "date": "1546288500"
  },
  "verify": {
    "date": "1546288800"
  }
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

پارامتر | نوع | توضیحات
------- | --- | -------
status | number | [وضعیت تراکنش](#ad39f18522)
track_id | number | کد رهگیری آیدی پی
id | string | کلید منحصر بفرد تراکنش که در مرحله [ایجاد تراکنش](#2c82b7acb2) دریافت شده است
order_id | string | شماره سفارش پذیرنده که در مرحله [ایجاد تراکنش](#2c82b7acb2) ارسال شده است
amount | number | مبلغ ثبت شده هنگام [ایجاد تراکنش](#2c82b7acb2)
date | timestamp | زمان ایجاد تراکنش
payment | object | اطلاعات پرداخت تراکنش
<span class="indent">track_id</span> | string | کد رهگیری پرداخت
<span class="indent">amount</span> | number | مبلغ قابل پرداخت
<span class="indent">card_no</span> | string | شماره کارت پرداخت کننده با فرمت `123456******1234`
<span class="indent">hashed_card_no</span> | string | هش شماره کارت پرداخت کننده با الگوریتم SHA256
<span class="indent">date</span> | timestamp | زمان پرداخت تراکنش
verify | object | اطلاعات تایید تراکنش
<span class="indent">date</span> | timestamp | زمان تایید تراکنش

<aside class="notice"> بعد از پرداخت تراکنش توسط پرداخت کننده، تراکنش باید
ظرف مدت حداکثر <b>10 دقیقه</b> تایید شود.
در غیر اینصورت مبلغ به کارت پرداخت کننده برگردانده خواهد شد.</aside>

<aside class="warning"> جهت جلوگیری از دوبار مصرف شدن یک پرداخت (Double Spending)،
پذیرنده موظف است کلیدهای منحصر بفردی که از طریق API آیدی پی دریافت می‌کند را (مثل <code>id</code> و <code>track_id</code>)
در دیتابیس خود ذخیره کند و از یکتا بودن آنها اطمینان حاصل فرماید.
<br/>
توجه داشته باشید که ممکن است یک مشتری رسید پرداخت آیدی پی را ذخیره کند و برای یک خرید دیگر از آن استفاده کند.
<br/>
مسئولیت بررسی و شناسایی Double Spending کاملا به عهده پذیرنده می‌باشد.</aside>