## Prompt #1
الأصلي: اعملي login
الناقص: السياق - المدخلات - القيود - النطاق
النسخة الكاملة:
Role: senior Laravel dev
Task: make a session login for a web app
Context: Laravel 12 project with MySQL, App\Models\User and the users table already exist
Input: email + password + remember me
Constraints: use Breeze (Blade) only, don't create a second User model, no Passport, no extra packages, follow the official Laravel 12 docs
Output format: 1) install commands in order 2) files to change with the code 3) manual test steps
Scope: login/logout and protecting /dashboard only, no register or reset password
...
مخرج الأصلي (ملخص): سأل أنهي نوع login (صفحة ويب - شاشة موبايل - HTML - نظام كامل) ومادّاش كود.
مخرج المحول (ملخص): [يتحط بعد التشغيل]
الفرق:
1. الأصلي سأل أسئلة، والمحول المفروض يدّي حل مباشر لأن السياق اتحدد.
2. القيد (Breeze بس) بيمنع التشتت بين Breeze وPassport.
3. صيغة المخرج المرتبة بتخلي الرد قابل للتنفيذ خطوة بخطوة.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Prompt #3
الأصلي: اكتبلي دالة تحسب الضريبة
الناقص: السياق - المدخلات - القيود - صيغة المخرج - النطاق
النسخة الكاملة:
Role: senior Laravel dev
Task: write a function that calculates the yearly tax on my store sales
Context: e-commerce app on Laravel 12 / PHP 8.3
Input: annualRevenue (float) and taxRate (percentage, e.g. 14)
Constraints: typed params, throw InvalidArgumentException if the amount is negative or the rate is outside 0-100, round to 2 decimals, no packages
Output format: one class App\Services\TaxCalculator that returns an array (subtotal / tax_rate / tax / total), plus a controller usage example and a numeric example
Scope: one flat rate only, no tax brackets or exemptions
...
مخرج الأصلي (ملخص): سأل اللغة المطلوبة وقواعد/نسبة الضريبة، ومادّاش كود.
مخرج المحول (ملخص): [يتحط بعد التشغيل]
الفرق:
1. الأصلي رجّع سؤال، والمحول المفروض يرجّع كود جاهز.
2. القيود بتضيف validation وexceptions بدل دالة ساذجة.
3. صيغة المخرج بتحدد شكل الـ array ومثال رقمي يتراجع عليه الناتج.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Prompt #4
الأصلي: حسنلي الأداء
الناقص: السياق - المدخلات - القيود - صيغة المخرج - النطاق
النسخة الكاملة:
Role: senior Laravel dev and MySQL performance specialist
Task: optimize a slow endpoint that returns the orders list
Context: Laravel 12 + MySQL 8, about 500k rows, the endpoint takes more than 3 seconds
Input: [paste the controller/query] + [paste the EXPLAIN output] + [paste the query count from Debugbar/Telescope]
Constraints: keep the response shape, no schema change except adding indexes, no external cache (cPanel without Redis)
Output format: 1) problems ranked by impact 2) code after the fix 3) migration for the indexes 4) how to measure before/after
Scope: this endpoint only
...
مخرج الأصلي (ملخص): [يتحط بعد التشغيل]
مخرج المحول (ملخص): [يتحط بعد التشغيل]
الفرق:
1. الأصلي مفيهوش كود ولا مقاييس، والمحول بيشتغل على الكود والـ EXPLAIN الحقيقي.
2. القيد (indexes بس، بدون Redis) بيمنع حلول مش هتشتغل على الاستضافة.
3. صيغة المخرج بتطلب قياس قبل/بعد.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Prompt #6
الأصلي: ليه الاختبار بيفشل
الناقص: السياق - المدخلات - القيود - صيغة المخرج - النطاق
النسخة الكاملة:
Role: senior Laravel dev, expert in Pest/PHPUnit
Task: find why the test fails and fix it
Context: Laravel 12, a Feature test on an API, uses RefreshDatabase on MySQL
Input: [test code] + [full error message and stack trace] + [the code under test] + [the command: php artisan test --filter=...]
Constraints: fix the code, or the test only if the test itself is wrong, don't change the API behavior without telling me, don't delete assertions
Output format: 1) root cause in two lines 2) the fix as a diff 3) the command to verify
Scope: this test only, don't rewrite other tests
...
مخرج الأصلي (ملخص): [يتحط بعد التشغيل]
مخرج المحول (ملخص): [يتحط بعد التشغيل]
الفرق:
1. الأصلي مفيهوش الخطأ ولا الكود، فمفيش تشخيص ممكن.
2. القيد "متحذفش assertions" بيمنع الحل الكسول.
3. صيغة المخرج (سبب - diff - تحقق) بتخلي الرد قصير وقابل للتطبيق.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Prompt #7
الأصلي: اكتب SQL يجيب الطلبات
الناقص: السياق - المدخلات - القيود - النطاق
النسخة الكاملة:
Role: database engineer
Task: write a query that returns the orders made by active users
Context: Laravel 12 + MySQL 8, orders(id, user_id, status, total, created_at) and users(id, name, is_active)
Input: orders from the last 30 days only
Constraints: SELECT only, use JOIN (not subquery), pick columns instead of SELECT *, prefer indexed columns
Output format: 1) raw SQL 2) the same query in Laravel Query Builder 3) a suggested index
Scope: read only, no data changes, no pagination
...
مخرج الأصلي (ملخص): رجّع query بسيط على جدول orders لوحده (أعمدة محددة وترتيب بـ id) من غير JOIN ولا فلتر على المستخدمين النشطين.
مخرج المحول (ملخص): [يتحط بعد التشغيل]
الفرق:
1. الأصلي تجاهل شرط "النشطين"، والمحول فيه الـ JOIN والفلتر.
2. تحديد أسماء الجداول والأعمدة منع التخمين.
3. صيغة المخرج بتدّي SQL وQuery Builder في رد واحد.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Prompt #9
الأصلي: ترجم الملف ده
الناقص: السياق - المدخلات - القيود - صيغة المخرج - النطاق
النسخة الكاملة:
Role: technical translator for Laravel language files
Task: translate the attached file from German to Arabic
Context: lang/de.json in a Laravel 12 project, it will become lang/ar.json
Input: [attach the file]
Constraints: don't change the keys, keep placeholders (:name, :count) as they are, keep the same JSON structure, simple formal Arabic
Output format: the full valid ar.json inside one code block, no explanation
Scope: values only, don't add or remove keys
...
مخرج الأصلي (ملخص): [يتحط بعد التشغيل]
مخرج المحول (ملخص): [يتحط بعد التشغيل]
الفرق:
1. الأصلي مفيهوش لغة الهدف، فممكن يترجم لأي لغة (حصل في تجربة سابقة: رجّع الإنجليزي بدل العربي).
2. قيد الـ keys والـ placeholders بيحمي الملف من الكسر.
3. صيغة المخرج JSON صالح بتخليه ينسخ مباشرة في المشروع.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Prompt #10
الأصلي: اشرح Laravel
الناقص: السياق - المدخلات - القيود - صيغة المخرج - النطاق
النسخة الكاملة:
Role: backend teacher explaining to a PHP developer
Task: explain the Service Container and Dependency Injection in Laravel
Context: I work on Laravel 12, I write Services and inject them into Controllers, and I want to understand what happens behind the scenes
Input: I know PHP and OOP well, no need to explain classes or interfaces
Constraints: no framework history, no comparison with other frameworks, no marketing talk
Output format: Egyptian Arabic, 3 sub-headings, one code example, then 3 common mistakes, around 400 words
Scope: the Service Container only, not the other parts of Laravel
...
مخرج الأصلي (ملخص): [يتحط بعد التشغيل]
مخرج المحول (ملخص): [يتحط بعد التشغيل]
الفرق:
1. الأصلي واسع جداً (إطار كامل)، والمحول محدد بموضوع واحد.
2. تحديد مستوى القارئ بيشيل شرح الأساسيات.
3. حدود الطول والصيغة بتخلي الرد منظم وقصير.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
