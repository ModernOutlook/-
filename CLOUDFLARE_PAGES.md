# انتشار نسخه وب پویش زمین با Cloudflare Pages

این مخزن عمومی فقط مخزن انتشار است. کدهای اصلی Android/Windows و Worker در این مخزن قرار نمی‌گیرند.

## ساختار مورد انتظار

فایل‌های انتشار باید در مسیر `web-release/` قرار بگیرند:

- `web-release/index.html`
- `web-release/admin.html`
- `web-release/patrol.html`
- `web-release/subscriber.html`
- `web-release/assets/`
- `web-release/manifests/`
- `web-release/sw.js`

## ساخت پروژه Cloudflare Pages

1. در Cloudflare Dashboard به Workers & Pages بروید.
2. یک Pages project با نام دقیق `pooyeshzamin-web` بسازید.
3. چون انتشار با GitHub Actions انجام می‌شود، لازم نیست Git integration داخل Pages فعال باشد.
4. در GitHub مخزن `ModernOutlook/-`، از Settings → Secrets and variables → Actions این دو Repository Secret را تعریف کنید:
   - `CLOUDFLARE_API_TOKEN`: توکن Cloudflare با دسترسی حداقلی موردنیاز برای مدیریت و انتشار Pages.
   - `CLOUDFLARE_ACCOUNT_ID`: شناسه حساب Cloudflare.
5. workflow موجود در `.github/workflows/deploy-cloudflare-pages.yml` با Push به شاخه `main` و تغییر فایل‌های `web-release/**` اجرا می‌شود.

## نشانی‌ها

پس از اولین Deploy موفق، نشانی پیش‌فرض پروژه:

- `https://pooyeshzamin-web.pages.dev/`
- `https://pooyeshzamin-web.pages.dev/admin.html`
- `https://pooyeshzamin-web.pages.dev/patrol.html`
- `https://pooyeshzamin-web.pages.dev/subscriber.html`

این نشانی‌ها تا ساخته‌شدن پروژه و موفقیت اولین Deploy فعال نیستند.

## انتشار نسخه جدید

1. محتوای کامل نسخه جدید را در `web-release/` جایگزین کنید.
2. تغییرات را به شاخه `main` ارسال کنید.
3. GitHub Actions به‌صورت خودکار فایل‌ها را به Cloudflare Pages می‌فرستد.
4. نتیجه را در تب Actions و Cloudflare Pages → Deployments بررسی کنید.

## نکات امنیتی و عملیاتی

- هرگز API Token، رمز عبور، توکن کاربران یا Secret را در فایل‌های مخزن قرار ندهید.
- این مخزن عمومی است؛ فقط فایل‌های مجاز برای انتشار عمومی را در آن قرار دهید.
- داده‌های خصوصی API نباید توسط Service Worker یا CDN به‌صورت عمومی cache شوند.
- پیش از استفاده عملی، CORS در Worker باید فقط origin نهایی Pages را مجاز کند. تغییر Worker در این مخزن اعمال نمی‌شود.
- GPS در مرورگر به HTTPS و مجوز کاربر نیاز دارد و ردیابی پس‌زمینه مانند برنامه Native تضمین نمی‌شود.
- انتشار فعلی صرفاً مسیر استقرار را آماده می‌کند؛ تا وقتی پوشه `web-release/` بارگذاری نشده و Secrets تعریف نشده‌اند، Deploy موفق نخواهد شد.
