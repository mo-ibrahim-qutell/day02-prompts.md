## Prompt 1 #N

الأصلي: make a login
الناقص: all of them
النسخة الكاملة: make a login for laravel project and use the user model and make it by breeze or passport
use the document of them
...
مخرج الأصلي: Sure — what kind of login do you want me to make?

* Website login page (email/username + password)
* App login screen (mobile-style UI)
* HTML/CSS/JS login (ready-to-run code)
* Login system (authentication + database)

مخرج المحول:
Flow: User → /login → Authentication → App\\Models\\User → Session → /dashboard

**Option 1 — Breeze** (يستخدم نفس `App\\Models\\User`، مفيش User model تاني):

```bash
composer require laravel/breeze --dev
php artisan breeze:install
php artisan migrate
npm install \&\& npm run dev
```

بيعمل login وregister وreset password وemail verification وprofile.

حماية الـ dashboard وجلب اليوزر:

```php
Route::get('/dashboard', fn () => view('dashboard'))->middleware('auth');
$user = Auth::user();
```

**Option 2 — Passport** (API login للموبايل: POST /oauth/token → Access Token):

```bash
php artisan install:api --passport
```

```php
class User extends Authenticatable implements OAuthenticatable
{
    use HasApiTokens, Notifiable;
}
```

```php
// config/auth.php
'api' => \['driver' => 'passport', 'provider' => 'users'],

Route::middleware('auth:api')->get('/user', fn (Request $r) => $r->user());
```

## الـ Header: `Authorization: Bearer YOUR\_ACCESS\_TOKEN`

## Prompt 2 #N

الأصلي: الكود ده فيه غلط صلحه

```php
class Blog extends BaseModel
{
    protected $filter = BlogFilter::class;
    protected $fillable = \['name', 'slug', 'description', 'tags', 'quote', 'reading\_time', 'is\_active', 'published\_at', 'added\_by\_id', 'owner\_name', 'job', 'owner\_email', 'owner\_image', 'owner\_facebook', 'owner\_twitter', 'owner\_instagram', 'blog\_category\_id', 'meta\_title', 'meta\_description', 'meta\_keywords'];

    public $translatable = \['description', 'meta\_title', 'meta\_description', 'meta\_keywords', 'tags', 'quote'];

    protected $casts = \[
        'is\_active' => 'boolean',
        'published\_at' => 'date',
        'meta\_keywords' => 'array',
        'tags' => 'array',
        'reading\_time' => 'integer',
    ];

    public function registerMediaCollections(): void { /\* blog\_images, public disk, jpeg/png/webp \*/ }

    public function category() { return $this->belongsTo(BlogCategory::class, 'blog\_category\_id'); }
}
```

الناقص: (Role) \&\& (context)
النسخة الكاملة: consider your self senior Laravel dev the name not saved with translated i have error here \[نفس الكود أعلاه]
...
مخرج الأصلي: هم تعديل: translatable المفروض تكون protected مش public، واستيراد BlogCategory من نفس الـ namespace مش ضروري.

مخرج المحول: المشكلة إن `name` مش موجود في `$translatable`. ضيفه:

```php
public $translatable = \['name', 'description', 'meta\_title', 'meta\_description', 'meta\_keywords', 'tags', 'quote'];
```

\---

## Prompt 3 #N

الأصلي: Write me a that calculates the tax
الناقص: role - context - output format
النسخة الكاملة: Write me a function that calculates the tax to calculate the year tax of my product selling in my e-commerce app and consider ur self as a senior laravel dev useing laravel 12
...
مخرج الأصلي: Sure. What language should the function be in, and what tax rules/rate should it use?

مخرج المحول:

```php
namespace App\\Services;

class TaxCalculator
{
    public function calculate(float $amount, float $taxRate): array
    {
        if ($amount < 0) throw new \\InvalidArgumentException('Amount cannot be negative.');
        if ($taxRate < 0 || $taxRate > 100) throw new \\InvalidArgumentException('Tax rate must be between 0 and 100.');

        $tax = $amount \* ($taxRate / 100);

        return \[
            'subtotal' => round($amount, 2),
            'tax\_rate' => $taxRate,
            'tax'      => round($tax, 2),
            'total'    => round($amount + $tax, 2),
        ];
    }

    public function calculateAnnualTax(float $annualRevenue, float $taxRate): array
    {
        return $this->calculate($annualRevenue, $taxRate);
    }
}
```

## مثال: 1,000,000 بنسبة 14% → tax = 140000، total = 1140000

## Prompt 4 #N

الأصلي: as an laravel senior dev make a index-user-page
i want to make it table with search and the table is name - email - phone - is\_active
الناقص: the contxt - result format
النسخة الكاملة: as an laravel senior dev make a index-user-page
i want to make it table with search and the table is name - email - phone - is\_active
i want it for api json
...
مخرج الأصلي: Controller بيرجع `view('admin.users.index')` مع بحث (name/email/phone) و paginate(15).

مخرج المحول:

```php
namespace App\\Http\\Controllers\\Api\\Admin;

class UserController extends Controller
{
    public function index(Request $request): JsonResponse
    {
        $users = User::query()
            ->select('id', 'name', 'email', 'phone', 'is\_active')
            ->when($request->filled('search'), function ($q) use ($request) {
                $s = $request->string('search')->trim()->toString();
                $q->where(fn ($q) => $q->where('name', 'like', "%{$s}%")
                    ->orWhere('email', 'like', "%{$s}%")
                    ->orWhere('phone', 'like', "%{$s}%"));
            })
            ->when($request->has('is\_active'),
                fn ($q) => $q->where('is\_active', $request->boolean('is\_active')))
            ->latest('id')
            ->paginate(min($request->integer('per\_page', 15), 100))
            ->withQueryString();

        return response()->json(\[
            'success' => true,
            'message' => 'Users retrieved successfully.',
            'data'    => $users->items(),
            'meta'    => \[
                'current\_page' => $users->currentPage(),
                'per\_page'     => $users->perPage(),
                'total'        => $users->total(),
                'last\_page'    => $users->lastPage(),
            ],
            'links'   => \[
                'prev' => $users->previousPageUrl(),
                'next' => $users->nextPageUrl(),
            ],
        ]);
    }
}
```

\---

## Prompt 5 #N

الأصلي: consider u are a database engineer write a sql query to return orders
in laravel 12 project
الناقص: example
النسخة الكاملة: consider u are a database engineer write a sql query to return orders that active users make use join
in laravel 12 project
...
مخرج الأصلي:

```php
DB::table('orders')->select('id', 'user\_id', 'status', 'total', 'created\_at', 'updated\_at')->orderByDesc('id')->get();
```

مخرج المحول:

```php
DB::table('orders')
    ->join('users', 'users.id', '=', 'orders.user\_id')
    ->where('users.is\_active', true)
    ->select('orders.\*')
    ->orderByDesc('orders.id')
    ->get();
```

\---

## Prompt 6 #N

الأصلي: translate this
Es gibt zahlreiche Ressourcen, um deutsche Texte zu lesen und das Leseverständnis zu verbessern, von einfachen Anfängertexten bis zu fortgeschrittenen Texten mit interaktiven Übungen.
الناقص: the result
النسخة الكاملة: translate this \[نفس النص الألماني] i want Arabic translated
...
مخرج الأصلي: There are numerous resources available for reading German texts and improving reading comprehension, ranging from simple beginner-level texts to advanced texts with interactive exercises.
مخرج المحول: هناك العديد من الموارد لقراءة النصوص الألمانية وتحسين فهم القراءة، من النصوص البسيطة للمبتدئين إلى النصوص المتقدمة مع تمارين تفاعلية.
---

## Prompt 7 #N

الأصلي: make api for all project
الناقص: the role - context - result - example
النسخة الكاملة: consider ur self as senior Laravel dev i wanna u to make api for simple-ecommerce project and i want u to use hmvc
...
مخرج الأصلي: Sure. Send me the project files (source code, database schema, stack, and any frontend screens) and I can map the features into endpoints (auth, users, products, orders...) with CRUD, validation, auth, and docs.

مخرج المحول: **HMVC Laravel E-commerce API Architecture**

```
app/
├── Modules/
│   ├── Auth/  Users/  Categories/  Products/  Cart/
│   ├── Orders/  Payments/  Addresses/  Reviews/
│   └── <Module>/
│       ├── Controllers/Api/   (ProductController.php)
│       ├── Models/            (Product.php)
│       ├── Requests/          (Store/UpdateProductRequest.php)
│       ├── Resources/         (ProductResource.php)
│       ├── Services/          (ProductService.php)
│       ├── Routes/api.php
│       └── Providers/         (ProductServiceProvider.php)
├── Http/ (Middleware, Resources)
├── Exceptions/
└── Support/ (ApiResponse.php, Constants, Traits, Helpers)
database/ (migrations, seeders, factories)
routes/ (api.php, web.php)
tests/ (Feature/Modules/..., Unit)
```

Request flow:
`Route → Controller → Form Request → Service → Model/DB → Resource → JSON`

API versioning: `/api/v1/` → auth, users, categories, products, cart, orders, payments, addresses, reviews

## كل business domain معزول، مع معمارية **HMVC + Service + API Resource**.

