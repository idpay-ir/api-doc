# استعلام وضعیت تراکنش

با استفاده از آدرس زیر می‌توانید آخرین وضعیت یک تراکنش را دریافت نمایید.

```shell
curl -X POST https://api.idpay.ir/v1.1/payment/inquiry \
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
curl_setopt($ch, CURLOPT_URL, 'https://api.idpay.ir/v1.1/payment/inquiry');
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($params));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
  'Content-Type: application/json',
  'X-API-KEY: 6a7f99eb-7c20-4412-a972-6dfb7cd253a4',
  'X-SANDBOX: 1',
));

$result = curl_exec($ch);
$httpcode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
curl_close($ch);

var_dump($httpcode);
var_dump($result);
```

```javascript
var request = require('request');

var options = {
  method: 'POST',
  url: 'https://api.idpay.ir/v1.1/payment/inquiry',
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
url := "https://api.idpay.ir/v1.1/payment/inquiry"

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

`POST https://api.idpay.ir/v1.1/payment/inquiry`

**پارامترهای مورد نیاز**

پارامتر | نوع | ضروری | توضیحات
------- | --- | ----- | -------
id | string | بله | کلید منحصر بفرد تراکنش که در مرحله [ایجاد تراکنش](#2c82b7acb2) دریافت شده است
order_id | string | بله | شماره سفارش پذیرنده که در مرحله [ایجاد تراکنش](#2c82b7acb2) ارسال شده است

**وضعیت پاسخ**

کد وضعیت | توضیحات
-------- | -------
200 | استعلام با موفقیت پاسخ داده شد
400 | [لیست خطاها](#d7b83cfb9c)
403 | [لیست خطاها](#d7b83cfb9c)
406 | [لیست خطاها](#d7b83cfb9c)

> با اجرای دستور بالا پاسخی مشابه متن زیر با فرمت JSON دریافت می‌شود:

```json
{
  "status": "100",
  "track_id": "10012",
  "id": "d2e353189823079e1e4181772cff5292",
  "order_id": "101",
  "amount": "10000",
  "wage": {
    "by": "payee",
    "type": "percent",
    "amount": "2500"
  },
  "date": "1546288200",
  "payer": {
    "name": "قاسم رادمان",
    "phone": "09382198592",
    "mail": "my@site.com",
    "desc": "توضیحات پرداخت کننده"
  },
  "payment": {
    "track_id": "888001",
    "amount": "10000",
    "card_no": "123456******1234",
    "date": "1546288500"
  },
  "verify": {
    "date": "1546288800"
  },
  "settlement": {
    "track_id": "12345678900",
    "amount": "7500",
    "date": "1546398000"
  }
}
```

> وضعیت 406: در صورت بروز خطا پاسخی مشابه متن زیر با فرمت JSON دریافت می‌شود:

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
wage | object | اطلاعات کارمزد تراکنش
<span class="indent">by</span> | string | دریافت کارمزد از پذیرنده یا پرداخت کننده<br/>- پذیرنده: payee<br/>- پرداخت کننده: payer
<span class="indent">type</span> | string | نوع کارمزد تراکنش<br/>- مبلغ ثابت: amount<br/>- درصدی: percent<br/>- پلکانی: stair
<span class="indent">amount</span> | number | مبلغ کارمزد تراکنش
date | timestamp | زمان ایجاد تراکنش
payer | object | اطلاعات پرداخت کننده تراکنش
<span class="indent">name</span> | string | نام پرداخت کننده
<span class="indent">phone</span> | string | شماره تلفن همراه پرداخت کننده
<span class="indent">mail</span> | string | پست الکترونیک پرداخت کننده
<span class="indent">desc</span> | string | توضیحات پرداخت کننده
payment | object | اطلاعات پرداخت تراکنش
<span class="indent">track_id</span> | string | کد رهگیری پرداخت
<span class="indent">amount</span> | number | مبلغ قابل پرداخت
<span class="indent">card_no</span> | string | شماره کارت پرداخت کننده با فرمت `123456******1234`
<span class="indent">date</span> | timestamp | زمان پرداخت تراکنش
verify | object | اطلاعات تایید تراکنش
<span class="indent">date</span> | timestamp | زمان تایید تراکنش
settlement | object | اطلاعات واریز تراکنش
<span class="indent">track_id</span> | number | کد رهگیری واریز
<span class="indent">amount</span> | number | مبلغ قابل واریز
<span class="indent">date</span> | timestamp | زمان واریز تراکنش به حساب بانکی پذیرنده
