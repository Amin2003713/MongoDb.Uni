بله، بدون مشکل. در اینجا مستندات MongoDB با استفاده از دستورات Shell:

---

# معرفی پایگاه‌داده‌های NoSQL با MongoDB

## مقدمه

پایگاه‌داده‌های NoSQL یک دسته از پایگاه‌داده‌ها هستند که با ساختار مدیریت داده رابطه‌ای سنتی (RDBMS) همخوانی ندارند.
برخلاف پایگاه‌داده‌های رابطه‌ای که داده‌ها را در جداول و ردیف‌ها ذخیره می‌کنند، پایگاه‌داده‌های NoSQL از انواع مدل‌های
داده برای ذخیره و بازیابی داده استفاده می‌کنند. MongoDB یکی از این پایگاه‌داده‌های NoSQL است که به دلیل انعطاف‌پذیری،
قابلیت مقیاس‌پذیری و عملکرد بالا مشهور است.

## مفاهیم کلیدی

1. **گرایش به سند**: MongoDB یک پایگاه‌داده‌ی گرایش به سند است که به این معناست که داده‌ها را در سند‌های انعطاف‌پذیر و
   شبیه JSON ذخیره می‌کند. این سند‌ها می‌توانند ساختارهای متفاوتی داشته باشند که این ویژگی MongoDB را برای کار با
   طرح‌های داده پیچیده و در حال تکامل مناسب می‌کند.

2. **طراحی بدون طرح**: برخلاف پایگاه‌داده‌های رابطه‌ای، MongoDB نیازی به طرح پیش‌فرض ندارد. هر سند می‌تواند ساختار خود
   را داشته باشد که این امکان را برای مدیریت و تکامل طرح‌های داده در طول زمان فراهم می‌کند.

3. **مقیاس‌پذیری**: MongoDB برای مقیاس افقی بر روی چند سرور طراحی شده است که این امر را مناسب برای مدیریت حجم بزرگی از
   داده و بار ترافیک بالا می‌سازد.

4. **زبان پرس‌وجو**: MongoDB از یک زبان پرس‌وجوی غنی بر پایه سینتکس مانند JSON استفاده می‌کند که این امر آشنایی
   برنامه‌نویسان با جاوااسکریپت یا زبان‌های مشابه را تسهیل می‌دهد.

## شروع با MongoDB

بیایید به چند مثال ابتدایی بپردازیم تا نحوه کارکرد MongoDB را بهتر درک کنیم.

### نصب

شما می‌توانید MongoDB را بر روی انواع سیستم‌عامل‌ها نصب کنید با دنبال کردن دستورالعمل‌های موجود در وب‌سایت رسمی
MongoDB: [راهنمای نصب MongoDB](https://docs.mongodb.com/manual/installation/)

### اتصال به MongoDB

```javascript
mongosh
```

### ایجاد پایگاه داده

```javascript
use myDatabase
```

### ایجاد مجموعه داده

```javascript
db.createCollection("myCollection")
```
