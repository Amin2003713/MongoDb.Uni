در MongoDB، `writeConcern` یک مفهوم حیاتی است که سطح تاییدیه‌ای که انتظار دارید از سرور پایگاه داده بعد از انجام عملیات
نوشتن (درج، به‌روزرسانی، حذف) را مشخص می‌کند. این اصولاً تضمین‌هایی که MongoDB در مورد دوام و قابلیت مشاهده داده‌های شما
فراهم می‌کند را مشخص می‌کند. در زیر تجزیه و تحلیل دقیقی از `writeConcern` و تنظیمات مختلف آن آمده است:

**گزینه‌های Write Concern:**

1. **`w: Number` (به طور پیش‌فرض اکثریت در MongoDB 4.0 و بعد از آن):** این گزینه حداقل تعداد اعضای حامل داده (اصلی و
   ثانویه) در یک مجموعه نمایندگی را که قبل از بازگشت عملیات نوشتن باید تأیید کنند را مشخص می‌کند. در اینجا برخی تنظیمات
   معمولی آمده است:
    - **`w: 1`**: سرور اصلی عملیات نوشتن را تأیید می‌کند. این امکان را فراهم می‌کند که داده‌ها در صورت خرابی اصلی قبل از
      تکثیر به ثانویه‌ها از دست رفته باشند.
    - **`w: "majority"`**: اکثریت اعضای مجموعه نمایندگی (شامل اصلی) عملیات نوشتن را تأیید می‌کنند. این اطمینان را فراهم
      می‌کند که داده‌ها تا زمانی که اکثریت در دسترس باشند دارای دوام هستند. این پیش‌فرض write concern در نسخه‌های جدید
      MongoDB است.
    - **`w: <تعداد_ثانویه‌ها>`**: تعداد مشخصی از اعضای ثانویه عملیات نوشتن را تأیید می‌کنند. این یک تعادل بین دوام و
      عملکرد فراهم می‌کند. می‌توانید بر اساس ضریب تکثیر خود و تحمل از دست رفتن داده، یک عدد انتخاب کنید.

2. **`wmajority`: (دیپریکیت شده در MongoDB 4.4)** این کوتاه‌نویس معادل `w: "majority"` است و تأییدیه نوشتن از اکثریت
   اعضای مجموعه نمایندگی را تضمین می‌کند.

3. **`w: 0`**: مشتری منتظر هیچ تأییدیه‌ای از سرور نمی‌ماند. این اولویت را به عملکرد می‌دهد اما هیچ گونه تضمینی در مورد
   دوام نوشتن ارائه نمی‌دهد. از این تنظیم با احتیاط استفاده کنید، به ویژه در محیط‌های تولید.

4. **`j: true` (زمینه‌نویسی):** فعال‌سازی زمینه‌نویسی نوشتن در سرور اصلی. این اطمینان را فراهم می‌کند که عملیات نوشتن
   قبل از تأیید به ژورنال پایدار می‌شود، ارائه دهنده دوام اضافی نسبت به تکثیر بر روی دیسک.

5. **`fSync`:** اجبار موتور ذخیره‌سازی برای fsync کردن فایل‌های داده به دیسک پس از تأیید نوشتن. این تضمین می‌کند که
   داده‌ها به طور فیزیکی بر روی دیسک نوشته شده باشند، تضمین‌های دوام قوی‌تری ارائه می‌دهد اما ممکن است عملکرد را تحت
   تأثیر قرار دهد.

**انتخاب صحیح writeConcern:**

تنظیمات ایده‌آل `writeConcern` به ویژگی‌های خاص برنامه‌ی شما بستگی دارد. در اینجا برخی نکات کلیدی را مد نظر قرار دهید:

- **دوام:** اگر از دست دادن داده قابل قبول نیست، تنظیماتی مانند `w: "majority"` یا `j: true` را برای اطمینان از تکثیر و
  پایداری نوشتن قبل از تأیید موفقیت درنظر بگیرید.
- **دسترسی:** اگر دسترسی بالا برایتان حیاتی است و برخی از از دست دادن داده‌های موقت قابل قبول است، تنظیم `w: 1` را با
  پیکربندی مجدد ثانویه‌های خوب برای اهداف تغییر نشان برای اطمینان از ایجاد شکست نظر بگیرید.
- **عملکرد:** برای بار‌های نوشتن سنگین، `w: 0` ممکن است جذاب باشد، اما پیش از استفاده از آن در محیط تولیدی، ریسک‌های
  احتمالی از از دست دادن داده را ارزیابی کنید.

**نکات اضافی:**

- **آگاهی از مجموعه نمونه:** تنظیمات `writeConcern` به ویژه برای مجموعه‌های نمونه حیاتی هستند. در یک استقرار مستقل (
  mongod)، عملیات نوشتن به محض اعمال آن به حافظه سرور به عنوان تأیید شناور می‌شود.
- **خوشه‌های شارد:** خوشه‌های شارد دارای تنظیمات write concern خود هستند که به شاردها منتقل می‌شود.

با درک این گزینه‌ها و ملاحظات، می‌توانید writeConcern را به درستی پیکربندی کنید تا با نیازهای برنامه‌ی شما همخوانی داشته
باشد و تعادلی بین دوام داده، دسترسی و عملکرد در استقرار MongoDB خود برقرار کنید.

---
به طبقهٔ معاملهٔ بالا (تراکنش‌های تجاری آنلاین) که اطمینان از دوام بالا می‌خواهد، توجه کنید:

```bash
mongo mystore

# Order placement with high durability requirements
db.orders.insertOne({
  customer: "Alice Doe",
  items: [ { name: "Shirt", price: 19.99 }, { name: "Hat", price: 14.50 } ]
}, { writeConcern: { w: "majority", j: true } })

{ "acknowledged" : true, "insertedId" : ObjectId("63d2cbef1234567890abcdef") }
```

**توضیحات:**

- این دستور شل به پایگاه دادهٔ mystore متصل شده و یک سند سفارش را درج می‌کند.
- پارامتر writeConcern به `{ w: "majority", j: true }` تنظیم شده است برای دوام بالا.
- خروجی مورد انتظار نشان می‌دهد که تأیید شناور موفقیت ثبت را (`"acknowledged" : true`) نشان می‌دهد و ObjectID تولیدشده
  را (`"insertedId" : ObjectId("...")`) برای سند سفارش درج شده فراهم می‌کند.

در طبقهٔ دوم (تعادل بین دوام و دسترسی) که پست‌های رسانه‌های اجتماعی را مورد بررسی قرار می‌دهد:

```bash
mongo social_media

# New social media post with moderate durability requirements
db.posts.insertOne({
  "author": "Bob",
  "content": "Hello, world!"
}, { writeConcern: { w: 1 } })

{ "acknowledged" : true, "insertedId" : ObjectId("63d2cbef1234567890abcdef") }
```

**توضیحات:**

- این دستور به پایگاه دادهٔ social_media متصل می‌شود و یک سند پست جدید را درج می‌کند.
- پارامتر writeConcern به `{ w: 1 }` تنظیم شده است برای تأیید در سرور اصلی.
- خروجی مورد انتظار مشابه نمونه اول است که تأیید شناور موفقیت ثبت را (`"acknowledged" : true`) نشان می‌دهد و ObjectID
  برای سند درج شده را فراهم می‌کند.

در طبقهٔ سوم (سناریوی تمرکز بر عملکرد) که واردات و خروجی‌های لاگ را بررسی می‌کند:

```bash
mongo myapp

# Log message insertion (for informational purposes only, prioritize durability for critical data)
db.logs.insertOne({ message: "Application started" }, { writeConcern: { w: 0 } })

{ "acknowledged" : false}
```

**توضیحات:**

- این دستور یک پیام لاگ را در مجموعهٔ logs در پایگاه دادهٔ myapp درج می‌کند.
- **هشدار:** پارامتر writeConcern به `{ w: 0 }` تنظیم شده است که منجر به عدم دریافت تأییدیه‌ای از سرور می‌شود. ممکن است
  هیچ خروجی‌ای نداشته باشید به دلیل عدم دریافت تأییدیه.

**توجه مهم:**

- به یاد داشته باشید که `writeConcern: { w: 0 }` باید با احتیاط برای سناریوهای خاص استفاده شود.
- خروجی‌ها ممکن است بسته به نسخهٔ و پیکربندی MongoDB شما متغیر باشد.

با استفاده موثر از `writeConcern` در دستورات پایهٔ شل MongoDB، می‌توانید تعادل مطلوبی بین اصالت داده، عملکرد و دسترسی
برای عملیات‌های نوشتن برنامه‌ی خود را دست‌یابی کنید.