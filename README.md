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
به کمک نرم‌افزار Postman  درخواست‌ها را ارسال کردیم:
1. یک کاربر به نام user1 با رمز ۱۲۳۴ بسازید.
![image](https://github.com/user-attachments/assets/5064f8ec-6951-453e-bc57-36c40f6de0b2)
![image](https://github.com/user-attachments/assets/5fe3acfe-a8be-4505-822d-99465a2bd1df)
2. یادداشتی با تیتر title1 و بدنه body1 برای user1 بسازید.
![image](https://github.com/user-attachments/assets/0d558d2f-ecbf-440f-b9d8-9ddeae51aff0)
3. یادداشتی با تیتر title2 و بدنه body2 برای user1 بسازید.
![image](https://github.com/user-attachments/assets/fdd61a80-8d64-4f4a-a26c-74453ef98d1f)
4. همه یادداشت‌های user1 را دریافت کنید. (باید ۲ یادداشت بالا را به عنوان خروجی دریافت کنید)
![image](https://github.com/user-attachments/assets/80250f88-f456-4f18-a01e-d245aa776dd3)

### تعامل با داکر
کارهای زیر را انجام دهید:
<div dir="rtl">1. به کمک دستورهای مناسب، image‌ها و containerهای خود را نشان دهید و آن‌هایی که مربوط به این آزمایش هستند را مشخص کنید.<br> ابتدا از دستور docker images استفاده می‎‌کنیم که لیستی از تمام image های دانلود شده در سیستم را نمایش می‌دهد.<br></div>

![Screenshot 2024-08-12 220837](https://github.com/user-attachments/assets/61a532e0-b1e7-45d0-afc5-5dfd15502a8b)

<div dir="rtl">برای کانتینر ها نیز از دستور docker ps استفاده می‌کنیم که کانتینر های در حال اجرا را نشان می‌‌دهد</div>
<div dir="rtl">(برای نمایش همه کانتینر ها باید از دستور docker ps -a  استفاده کرد). همانطور که مشاهده می‌شود هر دو کانتینر مربوط به پروژه این آزمایش است.<br></div>

![Screenshot 2024-08-12 221024](https://github.com/user-attachments/assets/a720495e-223d-4ab5-8c87-4bcdbeec2446)

<div dir="rtl">2. دستوری دلخواه را در کانتینر وب‌سرور اجرا کنید. دستور مورد نظر و خروجی آن را در گزارش خود قرار دهید.<br></div>
  <div dir="rtl"> با سه دستور ls و ps و bash خروجی زیر را می‌گیریم:<br></div>
  
![Q2](https://github.com/user-attachments/assets/252f00fd-00a8-4416-9d7c-a9bca10d02c5)


### پرسش‌ها

1. وظایف Dockerfile، image و container را توضیح دهید.

<div dir="rtl">image:<br></div>
<div dir="rtl">Image یک الگوی بسیار سبک و قابل حمل برای اجرای نرم‌افزار است که شامل تمام اطلاعات لازم برای اجرای یک برنامه از جمله کد، کتابخانه‌ها، ابزار سیستم، تنظیمات و داده‌های اجرایی است. به عبارت ساده‌تر، image مانند یک snapshot از محیط است که در آن برنامه باید اجرا شود.</div>

ساخت: تصاویر معمولاً از روی Dockerfile ساخته می‌شوند.

استفاده: تصاویر به عنوان پایه‌ای برای ایجاد و اجرای کانتینرها استفاده می‌شوند.

قابلیت حمل: به دلیل اینکه همه چیز درون یک image قرار دارد، می‌توان آن را به راحتی بین سیستم‌ها منتقل کرد.

<div dir="rtl">Dockerfile:<br></div>
<div dir="rtl">Dockerfile یک فایل متنی است که حاوی دستورات ساخت یک Docker Image می‌باشد. این فایل به داکر می‌گوید چگونه باید تصویر را بسازد.</div>

دستورات: Dockerfile شامل مجموعه‌ای از دستورات مانند FROM، RUN، COPY، EXPOSE و غیره است که هر کدام وظایف خاصی را برای ساخت تصویر انجام می‌دهند.

اتوماتیک کردن ساخت: Dockerfile اجازه می‌دهد تا فرآیند ساخت image ها خودکار شود، به طوری که هر بار که نیاز به image مشابه داریم، می‌توان این کار را با اجرای یک فرمان ساده انجام داد.

<div dir="rtl">container:<br></div>
<div dir="rtl">Container یک نمونه اجرایی از یک Docker image است که به عنوان محیطی ایزوله برای اجرای نرم‌افزار عمل می‌کند.</div>

اجرا: کانتینرها از روی image ها ساخته می‌شوند و شامل تمامی منابع لازم برای اجرای نرم‌افزار هستند.

ایزولاسیون: کانتینرها محیط‌های مجزایی ایجاد می‌کنند که در آنها نرم‌افزار می‌تواند بدون تداخل با سایر کانتینرها یا سیستم میزبان اجرا شود.

کاربردپذیری: کانتینرها به گونه‌ای طراحی شده‌اند که به صورت سریع و کارآمد هستند، به طوری که می‌توان آن‌ها را به سرعت شروع، متوقف و حذف کرد.


2. از kubernetes برای انجام چه کارهایی می‌توان استفاده کرد؟ رابطه آن با داکر چیست؟

   مدیریت و ارکستراسیون کانتینرها:

استقرار (Deployment): با استفاده از Kubernetes می‌توان کانتینرها را به‌صورت خودکار استقرار داد. Kubernetese وظایف حساس به منابع را در گره‌های مختلف توزیع می‌کند.

مقیاس‌پذیری (Scalability): افزایش یا کاهش تعداد کانتینرها به صورت پویا با توجه به بار کاری.

بازیابی خودکار (Self-healing): در صورت بروز خطا یا خرابی در کانتینرها، Kubernetes به‌صورت خودکار کانتینرهای خراب را با نمونه‌های جدید جایگزین می‌کند.

موازنه بار (Load Balancing): توزیع ترافیک و بار کاری بین کانتینرها به‌صورتی که بهره‌وری و عملکرد بهینه حفظ شود.


مدیریت منابع و خوشه‌ها:

نام‌گذاری و کشف سرویس‌ها (Service Discovery): Kubernetes سرویس‌های در حال اجرا در کانتینرها را شناسایی و نام‌گذاری می‌کند.

مدیریت پیکربندی (Configuration Management): مدیریت و به‌روز‌رسانی پیکربندی‌ها بدون نیاز به توقف یا راه‌اندازی مجدد سرویس‌ها.

مجموعه‌های حجم (Volumes): مدیریت پایداری داده‌ها و اتصال حجم‌های ذخیره‌سازی به کانتینرها.


امنیت و کنترل دسترسی:

مدیریت هویت و دسترسی: کنترل دسترسی به منابع و سرویس‌های Kubernetes با استفاده از سیستم‌های احراز هویت و کنترل دسترسی.

جداسازی منابع (Namespace Isolation): فراهم کردن محیط‌های مجزا برای اعضای مختلف تیم‌ها یا بخش‌های مختلف یک سازمان.


خودکارسازی و پیاده‌سازی CI/CD:

ادامه‌دهی یکپارچه (Continuous Integration/Continuous Deployment): خودکارسازی فرآیندهای CI/CD برای تسهیل در انتشار سریع و مستمر کدها.



رابطه Kubernetes و Docker:

مکمل بودن: Docker یک پلتفرم برای ساخت، اجرا و مدیریت کانتینرها است، در حالی که Kubernetes یک پلتفرم برای مدیریت و ارکستراسیون کانتینرها است. به عبارت دیگر، Docker کانتینرها را ایجاد و اجرا می‌کند و Kubernetes این کانتینرها را مدیریت و هماهنگ می‌کند.


استفاده از Docker به‌عنوان Runtime: Kubernetes از Docker به عنوان یکی از محیط‌های زمان اجرای کانتینرها استفاده می‌کند. در حالی که Docker کانتینرها را ارائه می‌دهد، Kubernetes به عنوان یک لایه بالاتر، مسئول هماهنگی و مدیریت این کانتینرها در مقیاس بزرگ است.
