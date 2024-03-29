## اسناد تو در تو (Embedded Documents) و آرایه ها (Arrays) در MongoDB

در MongoDB اسناد (Documents) می توانند حاوی داده های پیچیده باشند. برای مدل سازی روابط بین داده ها، MongoDB از دو ویژگی
قدرتمند پشتیبانی می کند: اسناد تو در تو و آرایه ها.

### اسناد تو در تو (Embedded Documents)

فرض کنید می خواهیم اطلاعات یک کاربر را همراه با آدرس او در یک سند ذخیره کنیم. به جای اینکه برای آدرس یک سند جداگانه
داشته باشیم، می توانیم از یک سند تو در تو برای ذخیره اطلاعات آدرس درون سند کاربر استفاده کنیم.

به مثال زیر توجه کنید:

```javascript
db.users.insert({
  "name": "علی رضوی",
  "age": 30,
  "email": "ali.rezaei@example.com",
  "address": {
    "city": "تهران",
    "street": "خیابان آزادی",
    "postalCode": 12345
  }
})
```

در این مثال، سند کاربر یک سند تو در تو به نام `address` دارد که حاوی اطلاعات مربوط به آدرس کاربر است. برای دسترسی به
فیلدهای درون سند تو در تو از علامت نقطه (.) استفاده می کنیم. مثلا برای دسترسی به شهر:

```javascript
db.users.find({ "address.city": "تهران" })
```

این دستور اسنادی را پیدا می کند که شهر کاربر در سند تو در تو برابر با "تهران" باشد.

### آرایه ها (Arrays)

آرایه ها برای ذخیره کردن لیستی از مقادیر همگن یا ناهمگن استفاده می شوند. فرض کنید می خواهید مهارت های یک کاربر را در یک
سند ذخیره کنید. می توانید از یک آرایه برای لیست کردن مهارت ها استفاده کنید.

مثال:

```javascript
db.users.insert({
  "name": "مرجان حسینی",
  "age": 25,
  "email": "marjan.hosseini@example.com",
  "skills": ["برنامه نویسی جاوااسکریپت", "طراحی وب", "سیستم های پایگاه داده"]
})
```

در این مثال، سند کاربر یک آرایه به نام `skills` دارد که لیستی از مهارت های کاربر را نگه می دارد. برای دسترسی به یک عنصر
خاص در آرایه از indexing (با شروع از صفر) استفاده می کنیم:

```javascript
db.users.find({ "skills.0": "برنامه نویسی جاوااسکریپت" })
```

این دستور اسنادی را پیدا می کند که اولین مهارت کاربر (با index 0) برابر با "برنامه نویسی جاوااسکریپت" باشد.

## مزایای استفاده از اسناد تو در تو و آرایه ها

* **مدل سازی روابط پیچیده:**  با استفاده از اسناد تو در تو و آرایه ها می توانید به راحتی روابط بین داده ها را مدل سازی
  کنید.
* **کاهش冗اندگی داده ها:**  ذخیره سازی داده های مرتبط در یک سند باعث کاهش تکرار داده ها (Redundancy) می شود.
* **بهبود کارایی پرس و جو:**  پرس و جو بر روی اسناد تو در تو و آرایه ها می تواند کارآمدتر از پرس و جو بر روی اسناد
  جداگانه باشد.

## نکات مهم

* می توانید تا 100 سطح از اسناد را به صورت تو در تو در هم قرار دهید.
* اندازه کل یک سند (با احتساب اسناد تو در تو) نباید از 16 مگابایت تجاوز کند.

## نتیجه گیری

اسناد تو در تو و آرایه ها ابزارهای قدرتمندی برای سازماندهی داده های پیچیده در MongoDB هستند. با درک نحوه استفاده از این
ویژگی ها، می توانید ساختار پایگاه داده خود را بهبود بخشیده و کارایی پرس و جوها را افزایش دهید.
