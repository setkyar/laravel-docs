# Errors & Logging

- [Configuration](#configuration)
- [Error တွေကို ထိန်းချုပ်ခြင်း](#handling-errors)
- [HTTP Exceptions](#http-exceptions)
- [404 Errors များကို ထိန်းချုပ်ခြင်း](#handling-404-errors)
- [Logging](#logging)

<a name="configuration"></a>
## Configuration

Application ရဲ့ Logging Handler ကို `app/start/global.php` [start file](lifecycle#start-files) ထဲမှာ Registered လုပ်ထားပါတယ်။ နဂိုအတိုင်းကတော့ File တစ်ဖိုင်ထဲကိုပဲ အသုံးပြုခိုင်းထားပါတယ်။ သို့သော်လည်း သင့်စိတ်ကြိုက် ပြင်ဆင်နိုင်ပါတယ်။ Laravel က နာမည်ကြီး  Loggin Library တစ်ခုဖြစ်တဲ့ [Monolog](https://github.com/Seldaek/monolog.md) ကိုသုံးထားတဲ့အတွက်  Monolog မှာပါဝင်တဲ့ အမျိုးအမျိုးသော handler များကိုအသုံးပြုနိုင်ပါတယ်။

ဥပမာ - Log File တစ်ခုတည်းမထားဘဲ နေ့စဉ်အလိုက် Log file တွေခွဲထားချင်တယ်ဆိုရင် ၊ start file မှာအောက်ကအတိုင်း ပြောင်းရေးလိုက်လို့ရပါတယ်

	$logFile = 'laravel.log';

	Log::useDailyFiles(storage_path().'/logs/'.$logFile);

### Error အသေးစိတ်

အရင်အတိုင်းဆို ၊ Error ရဲ့အသေးစိတ်ကို ဖော်ပြပါလိမ့်မယ်။ ဆိုလိုတာက Application မှာ Error တစ်ခုတက်နေမယ်ဆိုရင် ၊ အဲဒီ Error ရဲ့အသေးစိတ်နဲ့ ၊ အဲဒီ Error နဲ့ပတ်သက်နေတဲ့ ဖိုင်တွေနဲ့ အသေးစိတ်အချက်အလက်တွေကို ဖော်ပြပေးပါလိမ့်မယ်။ ဒီ Error အသေးစိတ်ပြတဲ့ Feature ကို ပိတ်ချင်တယ်ဆိုရင်တော့ `app/config/app.php` ထဲမှာ `debug` option ကို `false` လို့ လုပ်ပေးလိုက်ရုံပါပဲ။

> **မှတ်ချက်:** Application တကယ် Run ပြီဆိုရင်တော့ ဒီ Feature ကို ပိတ်ထားဖို့အတွက် အကြံပြုချင်ပါတယ်။

<a name="handling-errors"></a>
## Error တွေကို ထိန်းချုပ်ခြင်း

Default အနေနဲ့က `app/start/global.php` ထဲမှာ Exception တွေတိုင်းအတွက် Error Handler တစ်ခုပါရှိပါတယ်။

	App::error(function(Exception $exception)
	{
		Log::error($exception);
	});

ဒါကတော့ အရမ်းရိုးရှင်းတဲ့ Error Handler တစ်ခုပဲဖြစ်ပါတယ်။ တကယ်လို့ လိုအပ်မယ်ဆိုရင်တော့ ရှုပ်ထွေးတဲ့ Handler တွေကို သတ်မှတ်ပေးနိုင်ပါတယ်။ Exception တွေရဲ့နာမည်ပေါ်မူတည်ပြီး Handler တွေကိုခေါ်ပါတယ်။ ဥပမာပေးရမယ်ဆိုရင် ၊ `RunetimeException` အတွက်ပဲ handle လုပ်တဲ့ handler ကို အောက်ကအတိုင်း ရေးရပါမယ်။

	App::error(function(RuntimeException $exception)
	{
		// Handle the exception...
	});

Exception Handler တစ်ခုက Response တစ်ခု Return ပြန်မယ်ဆိုရင် အဲဒီ Response ကိုပဲ Browser မှာဖော်ပြမှာဖြစ်ပြီး ၊ တစ်ခြားသော Error Handler တွေကိုခေါ်မှာမဟုတ်ပါဘူး

	App::error(function(InvalidUserException $exception)
	{
		Log::error($exception);

		return 'Sorry! Something is wrong with this account!';
	});

PHP fatal error ဖြစ်တဲ့အချိန်ကို စောင့်ဖမ်းချင်ရင်တော့ `App::fatal` method ကိုသုံးရပါမယ်

	App::fatal(function($exception)
	{
		//
	});

Handler တွေအများကြီးရှိတယ်ဆိုရင်တော့ General ကြတဲ့ Handler တွေမှ အသေးစိတ်ကျတဲ့ handler တွေအထိအစဉ်လိုက် သတ်မှတ်ပေးသင့်ပါတယ်။ ဥပမာ - `Exception` တွေအားလုံးကို handler လုပ်တဲ့ handler တွေကိုအရင်ဆုံး သတ်မှတ်ပါ၊ ပြီးမှ `Illuminate\Encryption\DecryptException` လိုမျိုး အသေးစိတ် exception ကိုတော့ နောက်မှသတ်မှတ်ပေးပါ။

### Error Handlers တွေကို ဘယ်မှာရေးရမလဲ

Error Handler တွေကို သတ်မှတ်ပေးရမယ့် နေရာဆိုပြီးမသတ်မှတ်ထားပါဘူး။ ဒါနဲ့ပတ်သက်ပြီးလို့ကတော့ Laravel က လွတ်လပ်ခွင့်ပေးထားပါတယ်။ နည်းလမ်းတစ်ခုကတော့ `start/global.php` ထဲမှာ ထည့်ရေးနိုင်ပါတယ်။ အဲဒီနေရာက Application စစ Run ချင်း Code တွေထည့်ရေးသင့်တဲ့ အကောင်းဆုံးနေရာပါဘဲ။ အဲဒီဖိုင်ထဲမှာ တစ်ခြားရေးထားတာတွေ များနေတယ်ဆိုရင်တော့ `app/errors.php` ဆိုပြီး ဖိုင်ဆောက်လိုက်ပြီးတော့ `start/global.php` ထဲမှာ `require` လုပ်ပြီးရေးလို့ရပါတယ်။ တတိယနည်းလမ်းကတော့ Handler တွေအားလုံးကို ထိန်းချုပ်ပေးမယ့် [service provider](ioc#service-providers.md) တစ်ခု ဖန်းတီးလိုက်ပါ။ နောက်ထပ်တစ်ခေါက်ထပ်ပြောချင်ပါတယ် ၊ အဖြေမှန်ဆိုပြီးရယ်လို့ မရှိပါဘူး။ သင်နဲ့အကိုက်ညီဆုံးပုံစံအသုံးပြုပါ။

<a name="http-exceptions"></a>
## HTTP Exceptions

အချို့ Exception တွေက Server ကနေပြီးတော့ HTTP error code တွေဖော်ပြပေးပါတယ်။ ဥပမာ - "page not found" error (404), "unauthorized error" (401) သို့မဟုတ် 500 error လိုမျိုးဖြစ်ပါတယ်။ ဒီလို Response အတွက်တွေဆို အောက်ကအတိုင်းသုံးပါ။

	App::abort(404);

ကိုယ်ပိုင် message နဲ့ response လုပ်ပေးချင်လဲရပါတယ်။

	App::abort(403, 'Unauthorized action.');

အဲဒီ method ကို Application တစ်ခုလုံးရဲ့ request တွေအားလုံးမှာ အသုံးပြုမှာပါ။

<a name="handling-404-errors"></a>
## 404 Errors များကို ထိန်းချုပ်ခြင်း

"404 Not Found" error တွေအားလုံးကို ထိန်းချုပ်ပေးမယ့် handler ကိုလဲ ကိုယ့်စိတ်ကြိုက်ပုံစံနဲ့ အလွယ်တကူသတ်မှတ်ပေးနိုင်ပါတယ်။

	App::missing(function($exception)
	{
		return Response::view('errors.missing', array(), 404);
	});

<a name="logging"></a>
## Logging

အရမ်းလန်းတဲ့ [Monolog](http://github.com/seldaek/monolog) library ကို သုံးရပိုလွယ်အောင်လို့ Laravel logging အထောက်အပံ့တွေက ကူညီပေးပါတယ်။ Default အနေနဲ့ Log File တစ်ခုတည်းကိုပဲ သုံးအောင်လို့ သတ်မှတ်ပေးထားပါတယ်။ အဲဒီဖိုင်က `app/storage/logs/laravel.log` ဖြစ်ပါတယ်။ Log file ထဲကို အောက်ကအတိုင်း Log တွေရိုက်ထည့်နိုင်ပါတယ်

	Log::info('This is some useful information.');

	Log::warning('Something could be going wrong.');

	Log::error('Something is really going wrong.');

Logger အနေနဲ့  [RFC 5424](http://tools.ietf.org/html/rfc5424) ကသတ်မှတ်ပေးထားတဲ့အတိုင်း **debug**, **info**, **notice**, **warning**, **error**, **critical**, and **alert** ဆိုပြီး level ၇ ခုရှိပါတယ်။


Array ပုံစံနဲ့လည်း ထည့်ပေးလိုက်လို့ရပါတယ်

	Log::info('Log message', array('context' => 'Other helpful information'));

Monolog မှာ တစ်ခြား handler တွေ အများကြီးပါဝင်ပါတယ်။ လိုအပ်ရင် Laravel သုံးထားတဲံ Monolog instance ကိုသုံးနိုင်ပါတယ်။

	$monolog = Log::getMonolog();

Log ဖိုင်ထဲကို ထည့်သမျှ message တွေအားလုံးကို စောင့်ဖမ်းဖို့အတွက်လဲ event ရေးထားလို့ရပါတယ်။

#### Registering A Log Listener

	Log::listen(function($level, $message, $context)
	{
		//
	});
