# Facades

- [မိတ်ဆက်](#introduction)
- [ရှင်းလင်းချက်](#explanation)
- [လက်တွေ့အသုံးချခြင်း](#practical-usage)
- [ကိုယ်ပိုင် Facades တည်ဆောက်ခြင်း](#creating-facades)
- [Facades တွေကို Mock ပြုလုပ်ပေးခြင်း](#mocking-facades)
- [Facade Class ကိုကား](#facade-class-reference)

<a name="introduction"></a>
## မိတ်ဆက်

Facades (ဖဆော့စ် ဟုအသံထွက်ပါ) က Application ရဲ့ [IoC container](ioc.md) ထဲမှာရှိတဲ့ Class တွေကို static ပုံစံမျိုးသုံးနိုင်အောင် လုပ်ပေးပါတယ်။ Laravel မှာလဲ Facades တွေအများကြီးပါဝင်ပြီးတော့ အဲဒီ Facade တွေကိုလည်း သုံးဖူးပါလိမ့်မယ်။ သင်သုံးဖူးပေမယ့်လည်း သုံးဖူးမှန်းမသိဖြစ်နေတက်ပါတယ်။ Laravel "facades" တွေက Static Proxy တွေအနေနဲ့ ကူညီပေးပါတယ်။ ၄င်းက သာမာန် Static method တွေမဟုတ်ဘဲ ၊ ဖတ်/မှတ်လို့ကောင်းပြီး ပိုပြီးတိုတောင်းတဲ့ Syntax ပုံစံတွေဖြစ်စေတဲ့အပြင် Test လုပ်လို့အဆင်ပြေပြီး ပြောင်းလွယ်ပြင်လွယ်ဖြစ်စေပါတယ်။

အခါအားလျောက်စွာ သင့် Application နဲ့ Package တွေ အတွက် သင့်ကိုယ်ပိုင် Facades တွေတည်ဆောက်နိုင်ပါတယ်။ ဒါ့ကြောင့် ဒီ Class တွေရဲ့ အသုံးပြုပုံတွေ နဲ့ အယူအစတွေကို မွှေနှောက်ကြည့်ကြရအောင်။

> **မှတ်ချက်:** Facades ကိုမလေ့လာခင် ၊ Laravel ရဲ့ [IoC container](ioc.md) နဲ့သေချာရင်းနှီးနေဖို့ အကြံပြုချင်ပါတယ်။

<a name="explanation"></a>
## ရှင်းလင်းချက်

Facade ဆိုတာ Class တစ်ခုဖြစ်ပြီး Container ထဲက Object ကို ခေါ်အသုံးပြုခွင့်ပေးပါတယ်။ ဒီလိုခေါ်သုံးနိုင်တာ `Facade` class ကြောင့်ဖြစ်ပါတယ်။ Laravel ရဲ့ Facade တွေ နဲ့ သင်ကိုယ်ပိုင်ဆောက်ထားတဲ့ Facade တွေအားလုံးဟာ `Facade` class ကို Extend ပြုလုပ်ရပါတယ်။

သင့်ကိုယ်ပိုင် Facade class ဆောက်တော့မယ်ဆိုရင် `getFacadeAccessor` ဆိုတဲ့ method ကိုပဲ implement လုပ်ဖို့လိုပါမယ်။ `getFacadeAccessor` က Container ထဲကနေ ဘယ်ဟာကိုသုံးရမယ်လို့ ဆုံးဖြတ်ပေးပါတယ်။ သင့်ကိုယ်ပိုင် Facade ကနေ Resolved လုပ်ပြီးသား object ထဲကိုရွှေ့ပြောင်းဖို့အတွက် အခြေခံ `Facade` class မှာတော့ `__callStatic()` ဆိုတဲ့ magic-method ကိုသုံးထားပါတယ်။

ဒါ့ကြောင့် သင့်အနေနဲ့ `Cache::get` လိုမျိုး Facade တစ်ခုကို ခေါ်မယ်ဆိုရင် Laravel က Cache manager class ကို IoC container ထဲကနေဆွဲထုတ်ပြီး သူထဲက `get` method ကိုခေါ်ပေးပါတယ်။ နည်းပညာအခေါ်အဝေါ်အရဆိုရင်တော Laravel Facades တွေဆိုတာ Ioc container တွေကို service locator တစ်ခုအနေနဲ့အသုံးပြုနိုင်တဲ့ ရေး/ဖတ်/မှတ်ရလွယ်ကူသော syntax ဖြစ်ပါတယ်။

<a name="practical-usage"></a>
## လက်တွေ့အသုံးချခြင်း

အောက်ကအတိုင်းဆိုရင် ၊ Laravel cache system ကို ခေါ်တာပါ။ သာမာန်အပေါ်ယံအတိုင်း ကြည့်လိုက်မယ်ဆိုရင်တော့ `Cache` class ထဲက `get` ဆိုတဲ့ static method တစ်ခုကို ခေါ်လိုက်တယ်လို့ထင်ရပါတယ်။

	$value = Cache::get('key');

ဒါပေမယ့် `Illuminate\Support\Facades\Cache` class  ကိုကြည့်လိုက်မယ်ဆိုရင် `get` ဆိုတဲ့ static method လုံးဝမရှိပါဘူး

	class Cache extends Facade {

		/**
		 * Get the registered name of the component.
		 *
		 * @return string
		 */
		protected static function getFacadeAccessor() { return 'cache'; }

	}

Cache class က `Facade` class ကို extend လုပ်ထားပြီး `getFacadeAccessor()` ဆိုတာပဲရှိပါတယ်။ အဲဒီ Method ရဲ့တာဝန်က IoC နာမည်ကို return လုပ်ပေးယုံပါပဲ။

User က `Cache` facade ထဲက ဘယ် static method ကိုမဆို သုံးလိုက်မယ်ဆိုတာနဲ့ ၊ Laravel က IoC container ထဲကနေ `cache` ကိုခေါ်ပြီး ၊ ကိုယ်လိုချင်တဲ့ method (အခုအတိုင်းဆို `get`) ကို run ပေးပါတယ်။

ဒါ့ကြောင့် ၊ ကျွန်တော်တို့သုံးထားတဲ့ `Cache::get` ရဲ့ နောက်ကွယ်မှာက အောက်ကအတိုင်းရှိနေပါမယ်။

	$value = $app->make('cache')->get('key');

<a name="creating-facades"></a>
## ကိုယ်ပိုင် Facades တည်ဆောက်ခြင်း

Creating a facade for your own application or package is simple. You only need 3 things:
ကိုယ့် application (ဒါမှမဟုတ်) package အတွက် ကိုယ်ပိုင် facade ဆောက်ရတာလွယ်ကူပါတယ်။ အဆင့် ၃ ဆင့်ပဲလိုပါတယ် :

- An IoC binding
- facade class တစ်ခု
- facade ကိုယ် ခေါ်မယ့် Alia သတ်မှတ်ပေးရန်

ဥပမာတစ်ခုလောက် ကြည့်ကြပါမယ်။ ကျွန်တော်တို့မှာ `PaymentGateway\Payment` ဆိုတဲ့ class တစ်ခုရှိမယ်ဆိုကြပါစို့

	namespace PaymentGateway;

	class Payment {

		public function process()
		{
			//
		}

	}

ဒီ class က `app/models` directory ထဲမှာဖြစ်ဖြစ် (ဒါမှမဟုတ်) တစ်ခြား Composer က auto-load ပြုလုပ်နိုင်တဲ့ မည်သည့်နေရာတွင်မဆို တည်ရှိနိုင်ပါတယ်။

IoC container ထဲအဲဒီ class ကို ထည့်ပေးဖို့အတွက် bind လုပ်ဖို့လိုပါမယ်။

	App::bind('payment', function()
	{
		return new \PaymentGateway\Payment;
	});

ဒီ bind လုပ်ထားတာကို Register လုပ်ဖို့အတွက် အကောင်းဆုံးနည်းကတော့ `PaymentServiceProvider` ဆိုပြီး [service provider](ioc#service-providers.md) တစ်ခုဆောက်ပြီးတော့ အပေါ်က bind လုပ်ထားတာကို `register` ဆိုတဲ့ method ထဲ ထည့်ပေးလိုက်တာပါ။ အခုဆောက်ထားတဲ့ Service Provider ကို Laravel က load လုပ်ဖို့ဆိုရင်တော့ `app/config/app.php` ထဲမှာ သတ်မှတ်ပေးဖို့လိုပါမယ်။

Next, we can create our own facade class:
နောက်တစ်ဆင့်မှာတော့ ကိုယ်ပိုင် facade class ဆောက်နိုင်ပါပြီ -

	use Illuminate\Support\Facades\Facade;

	class Payment extends Facade {

		protected static function getFacadeAccessor() { return 'payment'; }

	}

နောက်ဆုံးအနေနဲ့ ကျွန်တော်တို့ရဲ့ Facade ကို Alia (Shortcut) အနေနဲ့ခေါ်သုံးချင်တယ်ဆိုရင်တော့ `app/config/app.php` ထဲက `aliases` array ထဲမှာ သတ်မှတ်ပေးရပါမယ်။ အခုဆိုရင်တော့ `Payment` class ရဲ့ `process` method ကို အောက်ကအတိုင်း လွယ်လွယ်ကူကူပဲ ခေါ်နိုင်ပါပြီ-

	Payment::process();

### Aliases တွေကို Auto-Load လုပ်တဲ့အခါ သတိထားစရာများ

[PHP က type hint မသက်မှတ်ပေးထားတဲ့ class တွေကို autload လုပ်ပေးမှာမဟုတ်တဲ့အတွက်](https://bugs.php.net/bug.php?id=39003)  `Aliases` array ထဲမှာ ရှိတဲ့ Class တွေကို တစ်ချို့သော instance တွေမှာ သုံးလို့မရပါဘူး။ `\ServiceWrapper\ApiTimeoutException` ကို `ApiTimeoutException` လို့ Alia လုပ်ထားလိုက်မယ်ဆိုရင် `\ServiceWrapper` namespace ရဲ့အပြင်ဖက်မှာ `catch(ApiTimeoutException $e)` လို့ခေါ်မယ်ဆိုရင် thrown လုပ်လိုက်ပေမယ့် ဘယ်တော့မှ catch လုပ်လို့မရပါဘူး။ ဒီလိုပြဿနာမျိုးကိုပဲ Model တွေမှာလဲ ကြုံတွေ့နိုင်ပါတယ်။ တစ်ခုတည်းသော ဖြေရှင်းနည်းကတော့ Alias တွေမသတ်မှတ်ဘဲ file ရဲ့အပေါ်ဆုံးမှာ `use` ဆိုပြီးသတ်မှတ်ပြီးသုံးတာပါပဲ။


<a name="mocking-facades"></a>
## Facades တွေကို Mock ပြုလုပ်ပေးခြင်း
Facade တွေ အဓိကရှိနေရခြင်းရဲ့အကြောင်းရင်းကတော့ Test လွယ်လွယ်ကူကူလုပ်နိုင်ဖို့ပဲဖြစ်ပါတယ်။ Mock လုပ်တဲ့အပိုင်းကိုတော့ [mocking facades](testing#mocking-facades.md) မှာ ပြည့်ပြည့်စုံစုံ ဖော်ပြပေးထားပါတယ်။

<a name="facade-class-reference"></a>
## Facade Class ကိုကား

အောက်ကဇယားမှာတော့ ရှိသမျှ Facade တွေနဲ့ သူရဲ့နောက်ကွယ်က class တွေကို ဖော်ပြပေးထားပါတယ်။ API Documentation ထဲကို သက်ဆိုင်ရာ နေရာလိုက်လဲ ချိတ်ပေးထားပါတယ်။ [IoC binding](ioc.md) key ရှိတဲ့ Facade တွေကိုလဲ သူ့ key တွေရေးပေးထားပါတယ်။

Facade  |  Class  |  IoC Binding
------------- | ------------- | -------------
App  |  [Illuminate\Foundation\Application](http://laravel.com/api/4.1/Illuminate/Foundation/Application.html)  | `app`
Artisan  |  [Illuminate\Console\Application](http://laravel.com/api/4.1/Illuminate/Console/Application.html)  |  `artisan`
Auth  |  [Illuminate\Auth\AuthManager](http://laravel.com/api/4.1/Illuminate/Auth/AuthManager.html)  |  `auth`
Auth (Instance)  |  [Illuminate\Auth\Guard](http://laravel.com/api/4.1/Illuminate/Auth/Guard.html)  |
Blade  |  [Illuminate\View\Compilers\BladeCompiler](http://laravel.com/api/4.1/Illuminate/View/Compilers/BladeCompiler.html)  |  `blade.compiler`
Cache  |  [Illuminate\Cache\Repository](http://laravel.com/api/4.1/Illuminate/Cache/Repository.html)  |  `cache`
Config  |  [Illuminate\Config\Repository](http://laravel.com/api/4.1/Illuminate/Config/Repository.html)  |  `config`
Cookie  |  [Illuminate\Cookie\CookieJar](http://laravel.com/api/4.1/Illuminate/Cookie/CookieJar.html)  |  `cookie`
Crypt  |  [Illuminate\Encryption\Encrypter](http://laravel.com/api/4.1/Illuminate/Encryption/Encrypter.html)  |  `encrypter`
DB  |  [Illuminate\Database\DatabaseManager](http://laravel.com/api/4.1/Illuminate/Database/DatabaseManager.html)  |  `db`
DB (Instance)  |  [Illuminate\Database\Connection](http://laravel.com/api/4.1/Illuminate/Database/Connection.html)  |
Event  |  [Illuminate\Events\Dispatcher](http://laravel.com/api/4.1/Illuminate/Events/Dispatcher.html)  |  `events`
File  |  [Illuminate\Filesystem\Filesystem](http://laravel.com/api/4.1/Illuminate/Filesystem/Filesystem.html)  |  `files`
Form  |  [Illuminate\Html\FormBuilder](http://laravel.com/api/4.1/Illuminate/Html/FormBuilder.html)  |  `form`
Hash  |  [Illuminate\Hashing\HasherInterface](http://laravel.com/api/4.1/Illuminate/Hashing/HasherInterface.html)  |  `hash`
HTML  |  [Illuminate\Html\HtmlBuilder](http://laravel.com/api/4.1/Illuminate/Html/HtmlBuilder.html)  |  `html`
Input  |  [Illuminate\Http\Request](http://laravel.com/api/4.1/Illuminate/Http/Request.html)  |  `request`
Lang  |  [Illuminate\Translation\Translator](http://laravel.com/api/4.1/Illuminate/Translation/Translator.html)  |  `translator`
Log  |  [Illuminate\Log\Writer](http://laravel.com/api/4.1/Illuminate/Log/Writer.html)  |  `log`
Mail  |  [Illuminate\Mail\Mailer](http://laravel.com/api/4.1/Illuminate/Mail/Mailer.html)  |  `mailer`
Paginator  |  [Illuminate\Pagination\Factory](http://laravel.com/api/4.1/Illuminate/Pagination/Factory.html)  |  `paginator`
Paginator (Instance)  |  [Illuminate\Pagination\Paginator](http://laravel.com/api/4.1/Illuminate/Pagination/Paginator.html)  |
Password  |  [Illuminate\Auth\Reminders\PasswordBroker](http://laravel.com/api/4.1/Illuminate/Auth/Reminders/PasswordBroker.html)  |  `auth.reminder`
Queue  |  [Illuminate\Queue\QueueManager](http://laravel.com/api/4.1/Illuminate/Queue/QueueManager.html)  |  `queue`
Queue (Instance) |  [Illuminate\Queue\QueueInterface](http://laravel.com/api/4.1/Illuminate/Queue/QueueInterface.html)  |
Queue (Base Class) |  [Illuminate\Queue\Queue](http://laravel.com/api/4.1/Illuminate/Queue/Queue.html)  |
Redirect  |  [Illuminate\Routing\Redirector](http://laravel.com/api/4.1/Illuminate/Routing/Redirector.html)  |  `redirect`
Redis  |  [Illuminate\Redis\Database](http://laravel.com/api/4.1/Illuminate/Redis/Database.html)  |  `redis`
Request  |  [Illuminate\Http\Request](http://laravel.com/api/4.1/Illuminate/Http/Request.html)  |  `request`
Response  |  [Illuminate\Support\Facades\Response](http://laravel.com/api/4.1/Illuminate/Support/Facades/Response.html)  |
Route  |  [Illuminate\Routing\Router](http://laravel.com/api/4.1/Illuminate/Routing/Router.html)  |  `router`
Schema  |  [Illuminate\Database\Schema\Blueprint](http://laravel.com/api/4.1/Illuminate/Database/Schema/Blueprint.html)  |
Session  |  [Illuminate\Session\SessionManager](http://laravel.com/api/4.1/Illuminate/Session/SessionManager.html)  |  `session`
Session (Instance)  |  [Illuminate\Session\Store](http://laravel.com/api/4.1/Illuminate/Session/Store.html)  |
SSH  |  [Illuminate\Remote\RemoteManager](http://laravel.com/api/4.1/Illuminate/Remote/RemoteManager.html)  |  `remote`
SSH (Instance)  |  [Illuminate\Remote\Connection](http://laravel.com/api/4.1/Illuminate/Remote/Connection.html)  |
URL  |  [Illuminate\Routing\UrlGenerator](http://laravel.com/api/4.1/Illuminate/Routing/UrlGenerator.html)  |  `url`
Validator  |  [Illuminate\Validation\Factory](http://laravel.com/api/4.1/Illuminate/Validation/Factory.html)  |  `validator`
Validator (Instance)  |  [Illuminate\Validation\Validator](http://laravel.com/api/4.1/Illuminate/Validation/Validator.html)
View  |  [Illuminate\View\Factory](http://laravel.com/api/4.1/Illuminate/View/Factory.html)  |  `view`
View (Instance)  |  [Illuminate\View\View](http://laravel.com/api/4.1/Illuminate/View/View.html)  |