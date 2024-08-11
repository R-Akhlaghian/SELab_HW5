# SELab_HW5

## گزارش آزمایش

### استقرار پروژه
استقرار شما باید نیازهای زیر را برآورده کرده باشد:

۱. شما باید یک فایل docker-compose.yml داشته باشید که شامل ۲ سرویس است؛ یکی مربوط به خود وب‌سرور جنگو است و دیگری مربوط به پایگاه‌داده است.
   
![Screenshot from 2024-08-11 09-53-32](https://github.com/user-attachments/assets/550794a8-18b6-4e70-b1e5-a6bd285b3c87)

توضیح اجزای فایل docker-compose.yml:
- متغیر version: ورژن فایل داکر کامپوز را که باید با ورژن docker engine مورد استفاده سازگار باشد، مشخص می‌کند.
- بخش services: در این بخش سرویس‌های مربوط به برنامه تعریف می‌شوند (سرویس وب و دیتابیس)
- سرویس db: در اینجا سرویس پایگاه داده با نام db تعریف شده که از ایمیچ دیتابیس postgres استفاده می‌کند. در متغیر image نام ایمیج و ورژن آن را برای postgres تعیین می‌کنیم. در بخش environment متغیرهای محیطی را برای کانتینر دیتایبس ست می‌کنیم. (نام کاربر، رمز عبور دیتابیس و نام دیتابیس) در بخش volumes فضایی را برای ذخیره‌سازی دائمی دیتابیس تعریف می‌کنیم تا در صورت متوقف یا پاک شدن کانتینر، دیتاها پاک نشوند. ولوم postgres_data در آدرس تعیین‌شده در کانتینر ایجاد می‌شود و دیتاها را ذخیره می‌کند.
- سرویس web: در اینجا سرویس وب‌سرور جنگو با نام web تعریف شده که از آخرین ورژن ایمیج django موجود در داکر هاب استفاده می‌کند. در بخش ports پورت مورد نظر برای کانتینر و لوکال‌هاست تعریف می‌شود که بتوان مانند حالت لوکال، با ران کردن برنامه با استفاده از داکر هم در آدرس localhost:8000 به وب اپ مورد نظر دسترسی داشت. در بخش environment نیز متغیرهای محیطی را برای وب‌سرور جنگو تعریف می‌کنیم. (DJANGO_SECRET_KEY برای کارهای رمزنگاری که جنگو از آن استفاده می‌کند و DATABASE_URL که آدرس دیتابیسی که جنگو باید به آن وصل شود را مشخص می‌کند.) بخش dependes_on هم مشخص می‌کند که سرویس وب به سرویس دیتابیس وابسته است و سرویس db باید قبل از سرویس web استارت شود.
- بخش volumes: در این بخش هم ولوم‌های تعریف‌شده که بین سرویس‌ها مشترک است، آورده می‌شود که در اینجا ولوم postgres_data که هر دو سرویس وب و دیتابیس از آن استفاده می‌کنند، مشخص شده است.
  متغیرهای محیطی را می‌توان در فایل .env تعریف کرد و در env_file آدرس فایل .env را در docker-compose قرار داد و برای مسائل امنیتی بهتر است فایل .env را فقط در لوکال خود ذخیره و روی ریپو پوش نکنیم.

۲. در صورت نیاز می‌توانید ۱ یا چند فایل داکر (Dockerfile) داشته باشید و از آن‌ها در docker compose استفاده کنید.

   از داکرفایل برای ساخت ایمیج مورد نظر با استفاده از دستوراتی که در داکرفایل نوشته شده است استفاده می‌شود ولی در اینجا برای هر دو سرویس وب و دیتابیس از ایمیج‌های آماده که در داکرهاب موجود است استفاده کرده‌ایم و نیازی به استفاده از داکرفایل نداریم.

۳. وب‌سرور باید از طریق پورت ۸۰۰۰ درخواست‌های ارسالی را دریافت کند.
   - با تعریف ports: - "8000:8000" در سرویس web این مورد اعمال شده است.

۴. شما باید جنگو را طوری تنظیم بکنید که به پایگاه‌داده postgresای که بالا آورده‌اید، متصل شود.
   
![Screenshot from 2024-08-11 10-28-11](https://github.com/user-attachments/assets/6f2c13fe-0eab-4a02-bbd7-68a0ca865114)

در فایل settings.py در قسمت دیتابیس، اطلاعات پایگاه‌داده postgresای را که ایجاد کرده‌ایم وارد می‌کنیم.

در نهایت با دستور زیر سرویس‌ها را استارت می‌کنیم: (وب‌سرویس‌مان در آدرس http://localhost:8000 قابل دسترس است)

$ docker-compose up

برای متوقف کردن سرویس‌ها هم از دستور زیر استفاده می‌کنیم:

$ docker-compose down




### ارسال درخواست به وب‌سرور
به کمک مرورگر، نرم‌افزار Postman، ترمینال یا هر ابزار مناسب دیگری، به وسیله ارسال درخواست مناسب به وب‌سرور، گام‌های زیر را انجام دهید:
1. یک کاربر به نام user1 با رمز ۱۲۳۴ بسازید.
2. یادداشتی با تیتر title1 و بدنه body1 برای user1 بسازید.
3. یادداشتی با تیتر title2 و بدنه body2 برای user1 بسازید.
4. همه یادداشت‌های user1 را دریافت کنید. (باید ۲ یادداشت بالا را به عنوان خروجی دریافت کنید)

**درخواست ارسال شده و خروجی دریافت شده را برای همه گام‌های بالا در گزارش خود قرار دهید.**

### تعامل با داکر
کارهای زیر را انجام دهید:
1. به کمک دستورهای مناسب، image‌ها و containerهای خود را نشان دهید و آن‌هایی که مربوط به این آزمایش هستند را مشخص کنید.
2. دستوری دلخواه را در کانتینر وب‌سرور اجرا کنید. دستور مورد نظر و خروجی آن را در گزارش خود قرار دهید.

### پرسش‌ها

1. وظایف Dockerfile، image و container را توضیح دهید.   
2. از kubernetes برای انجام چه کارهایی می‌توان استفاده کرد؟ رابطه آن با داکر چیست؟
