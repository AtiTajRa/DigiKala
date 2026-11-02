# تحلیل داده‌های دیجی‌کالا | Digikala Data Analysis

##

### هدف پروژه
این پروژه برای بررسی داده‌های دیجی‌کالا انجام شده و دو بخش اصلی دارد:

1. **تحلیل فروش:** شناسایی شهرها و کالاهای پرفروش، بررسی فروش ماهانه و پیدا کردن ماه‌هایی که مبلغ فروششان ممکن است غیرعادی باشد.
2. **تحلیل کامنت‌ها:** ترکیب کامنت‌های فایل‌های `comment` و `keifiat`، پاک‌سازی و نرمال‌سازی متن فارسی، و استخراج واژه‌های مهم با استفاده از TF-IDF.

### محاسبه‌ی مبلغ فروش ماهانه
فایل سفارش‌ها تعداد کالای فروخته‌شده را دارد، اما قیمت هر سفارش در آن ثبت نشده است. قیمت‌ها از فایل تاریخچه‌ی قیمت گرفته شده‌اند.

برای تخمین مبلغ فروش ماهانه:

1. تعداد فروش هر کالا در هر شهر و ماه جمع شده است.
2. آخرین قیمت ثبت‌شده‌ی هر کالا در همان ماه انتخاب شده است.
3. تعداد فروش در آن قیمت ضرب شده است.

`مبلغ فروش تخمینی = تعداد فروش ماهانه × آخرین قیمت ثبت‌شده در همان ماه`

از آخرین قیمت ماه به‌عنوان قیمت نماینده استفاده شده، چون قیمت دقیق هر سفارش در فایل سفارش‌ها وجود ندارد. این مبلغ تخمینی است؛ اگر قیمت کالا در طول ماه تغییر کرده باشد، مبلغ به‌دست‌آمده ممکن است با درآمد واقعی تفاوت داشته باشد.

### تحلیل کامنت‌ها
کامنت‌های فایل‌های `comment` و `keifiat` با هم ترکیب می‌شوند. ردیف‌هایی که متن کامنت یا شناسه‌ی کالا ندارند حذف می‌شوند. سپس متن فارسی نرمال‌سازی و توکن‌سازی می‌شود و واژه‌های توقف حذف می‌شوند. در پایان، با TF-IDF واژه‌های مهم کامنت‌ها استخراج می‌شوند.

امتیاز TF-IDF میزان اهمیت نسبی یک واژه را در مجموعه‌ی کامنت‌ها نشان می‌دهد؛ این امتیاز به‌تنهایی مثبت یا منفی بودن نظر کاربران را مشخص نمی‌کند.

### محدودیت‌ها و نقص داده‌ها

- فایل سفارش‌ها از سپتامبر ۲۰۱۳ تا دسامبر ۲۰۱۸ را پوشش می‌دهد، اما فایل تاریخچه‌ی قیمت فقط از ژوئن تا نوامبر ۲۰۱۸ داده دارد.
- هنگام اتصال سفارش و قیمت، اگر برای کالا و ماه موردنظر قیمت متناظر وجود نداشته باشد، آن سفارش در ادغام `inner` وارد نتیجه نمی‌شود.
- آخرین قیمت ماه به تمام فروش‌های همان ماه نسبت داده می‌شود؛ بنابراین مبلغ فروش، درآمد واقعی و دقیق نیست.
- به‌دلیل پوشش زمانی و تطبیق محدود شناسه‌ی کالاها، محاسبه‌ی مبلغ فروش ممکن است فقط بخشی از سفارش‌ها را شامل شود.
- استخراج واژه‌های مهم با TF-IDF تحلیل احساسات نیست؛ برای تشخیص مثبت یا منفی بودن کامنت‌ها به روش جداگانه‌ای نیاز است.
- در کد انتخاب کامنت‌های کالاهای پرفروش، باید اطمینان حاصل شود که کامنت‌های همه‌ی شهرهای موردنظر جمع‌آوری می‌شوند، نه فقط شهر آخر حلقه.

---


### Project Objective
This project analyzes Digikala data in two main areas:

1. **Sales analysis:** identifying top-selling cities and products, examining monthly sales, and finding months whose sales amounts may be unusual.
2. **Comment analysis:** combining comments from the `comment` and `keifiat` files, cleaning and normalizing Persian text, and extracting important terms using TF-IDF.

### Monthly Sales Amount
The orders file contains the quantity sold, but not the price charged for each order. Prices are taken from the separate price-history file.

To estimate monthly sales amount:

1. The quantity sold for each product, city, and month is summed.
2. The latest recorded price for each product in that month is selected.
3. The monthly quantity is multiplied by that price.

`Estimated sales amount = Monthly quantity sold × Latest recorded price in that month`

The latest price is used as a representative price because the order file does not contain the exact price for each order. This is an estimate; if the product price changed during the month, the result may differ from actual revenue.

### Comment Analysis
Comments from the `comment` and `keifiat` files are combined. Rows without a comment or product ID are removed. Persian text is then normalized and tokenized, stop words are removed, and TF-IDF is used to extract important terms.

A TF-IDF score indicates the relative importance of a term in the comment collection. It does not, by itself, determine whether a comment is positive or negative.

### Data Limitations

- The orders file covers September 2013 through December 2018, while the price-history file contains data only from June through November 2018.
- When joining orders and prices, an `inner` merge excludes orders without a matching price for the same product and month.
- The latest monthly price is applied to all sales in that month, so the calculated sales amount is not exact actual revenue.
- Due to limited date coverage and limited product-ID matches, the calculated sales amount may represent only a subset of all orders.
- TF-IDF term extraction is not sentiment analysis; a separate method is required to classify comments as positive or negative.
- The code that selects comments for top-selling products should be checked to ensure it collects comments for every intended city, rather than only the last city in the loop.
