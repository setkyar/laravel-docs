# Request Lifecycle

- [Overview](#overview)
- [Request Lifecycle](#request-lifecycle)
- [Start Files](#start-files)
- [Application Events](#application-events)

<a name="overview"></a>
## Overview

သင် tools တစ်ခုကို တကယ်လက်တွေ့သုံးပြီဆိုရင် အဲ့ဒီ tool က ဘယ်လိုအလုပ်လုပ်တယ်ဆိုတာကိုသိရင် သင်ပိုပြီး ယုံကြည်မှူရှိလာပါလိမ့်မယ်။ development tools တွေရဲ့ function တွေဘယ်လိုအလုပ်လုပ်လဲသင်သိလာတဲ့အခါမှာ သင်အဲဒါတွေကိုအသုံးပြုတဲ့အခါပိုပြီးတော့ အဆင်ပြေ ယုံကြည်လာပါလိမ့်မယ်။ ဒီ document ရဲ့ အဓိကရည်ရွယ်ချက်က  Laravel Framework ဘယ်လိုအလုပ်လုပ်လဲဆိုတာကို ကောင်းမွန်တဲ့ hight-level overview တစ်ခုပေးဖို့ပါ။ Framework အကြောင်း overview ကောင်းကောင်းသိသွားတဲ့အချိန်မှာ "magical" လို့ထင်တာတွေနည်းသွားပြီးတော့ သင် application တည်ဆောက်ရာမှာပိုပြီးတော့ confident ရှိလာပါလိမ့်မယ်။ Request Lifecycle  ရဲ့ hight level overview ရဲ့ဖြည့်စွတ်ချက်မှာတော့ "start" files နဲ့ application events ကိုပါ cover လုပ်ထားပါတယ်။

တကယ်လို့သင့်အနေနဲ့ terms အားလုံးကိုနားမလည်ဘူးဆိုရင်စိတ်ထဲမထားပါနဲ့ ။ အခြေခံအားဖြင့် ဘယ်လိုလုပ်နေလဲဆိုတာကို ကြိုးစားကြည့်ပြီး documencation ရဲ့တစ်ခြား အပိုင်းတွေကို ဖတ်ပြီး သင့်ပိုပြီးသိလာပါလိမ့်မယ်။

<a name="request-lifecycle"></a>
## Request Lifecycle

သင့် application ရဲ့ Request အားလုံးကို `public/index.php` ဆီကို redirect လုပ်ပါတယ်။  Apache ကိုအသုံးပြုတဲ့အခါမှာ `.htaccess` files က request အားလုံးကို `index.php` စီ redirect လုပ်ပေးပါတယ်။ အဲ့ဒီ့ကနေစပြီးတော့ Laravel က request တွေကိုလက်ခံတာ  response တွေကို client ဆီပြန်ပေးတာတွေကို handles လုပ်ပေးသွားတာပါ၊ Laravel ရဲ့  bootstrap general idea က အသုံးဝင်ပါလိမ့်မယ် ၊ ဒါကြောင့်ကျွန်တော်တို့အခု အောက်မှာရှင်းပြပါ့မယ်။

Laravel ရဲ့ bootstrap process လေ့လာတဲ့နေရာမှာ **Service Providers**  ကအဓိကဖြစ်ပါတယ်။ Services Providers တွေရဲ့  Lists တွေကို  `app/config/app.php` ကိုဖွင့်ပြီး `providers` arrays မှာရှာတွေ့နိုင်ပါတယ်။ ဒီ providers တွေက Laravel ကို bootstrap လုပ်ဖို့ အဓိက ဖြစ်ပါတယ်။ သင့် `index.php` file ကို request တစ်ခုလုပ်လိုက်တာနဲ့ `bootstrap/start.php` က load လုပ်ပါမယ်။ အဲ့ဒီ့ file က Laravel `Application` object တွေကို create လုပ်ပါ့မယ်၊ နောက် [Ioc container](ioc.md) ကိုလည်း serve လုပ်ပါတယ်။

`Application` ရဲ့ object တွေကို create လုပ်ပြီးပြီဆိုရင်တော့ project ရဲ့ paths အချို့ကိုစတင်ပြီး တပ်ဆင်ပါ့မယ်၊ နောက် [environment detection](configuration#environment-configuration.md) တွေကိုဆက်လက်လုပ်ဆောင်ပါတယ်။ ဒါပြီးရင်တော့ Laravel bootstrap script တွေကို call လုပ်ပါ့မယ်။ Laravel source ရဲ့တွင်းပိုင်း File တွေထိ live ဖြစ်သွားပြီဆိုရင် သင့်ရဲ့ configuration ပေါ်မူတည်ပြီး setting တွေကို တပ်ဆင်ပါလိမ့်မယ်။ timezoneတို့၊ error reporting နဲ့ အခြား လိုအပ်တဲ့ setting တွေပေါ့။ ဒါပေမယ့် သင့် Application လိုအပ်တဲ့ Service Provider များအားလုံးကို register လုပ်ထားဖို့ကလည်း အခြား configuration တွေအားလုံးလိုပဲ အရေးကြီးပါတယ်။

Simple service providers only have one method: `register`. This `register` method is called when the service provider is registered with the application object via the application's own `register` method. Within this method, service providers register things with the [IoC container](ioc). Essentially, each service provider binds one or more [closures](http://us3.php.net/manual/en/functions.anonymous.php) into the container, which allows you to access those bound services within your application. So, for example, the `QueueServiceProvider` registers closures that resolve the various [Queue](/docs/queues.md) related classes. Of course, service providers may be used for any bootstrapping task, not just registering things with the IoC container. A service provider may register event listeners, view composers, Artisan commands, and more.

Service Providers တွေအကုန်လုံး register လုပ်ပြီးရင် သင့်ရဲ့ `app/start` file loadလုပ်ပါလိမ့်မယ်။ နောက်ဆုံးအနေနဲ့သင့်ရဲ့ `app/routes.php` ကို load လုပ်ပါ့မယ်။ နောက်တစ်ခါသင့် application ရဲ့ `route.php` load လုပ်ပြီးရင် request objects တွေသင့် application ဆီကိုပို့ပါမယ်၊ ဒါက route တွေကိုစေလွှတ်ခြင်းဖြစ်ပါလိမ့်မယ်။

ကဲဒါဆိုရင်အတိုချုပ်လိုက်ကြရအောင်:

1.  Request တွေက `public/index.php` file ဆီကို ဝင်ရောက်လာတယ်
2.  `bootstrap/start.php` file က  Application ကို create လုပ်ပြီးတော့ environment ကို detect လုပ်တယ်
3. အတွင်းပိုင်း `framework/start.php` file က setting တွေကို configure လုပ်တယ်နောက်တော့ service providers တွေကို load လုပ်တယ်
4. Application ရဲ့ `app/start` file တွေ load လုပ်တယ်
5. Application ရဲ့ `app/route` file load လုပ်တယ်
6. Request objects တွေကို application ဆီကို ပို့တယ်၊ အဲဒီ့ကနေ object တွေ Response ပြန်လာတယ်
7. ပြန်လာတဲ့ Response တွေကို client ဆီကိုပြန်ပို့တယ်

အခု Laravel က application ရဲ့ Request  တွေကိုဘယ်လိုဖြေရှင်းသွားတယ်ဆိုတာသိပြီးသွားပြီ `start`  file အကြောင်းကို နည်းနည်းအသေးစိတ်ဆက်လေ့လာလိုက်ကြအောင်။

<a name="start-files"></a>
## Start Files

သင့် Application ရဲ့ Start Files တွေက `app/start` ထဲမှာပါ။ Default အရဆိုရင် သင့် application ရဲ့ `global.php`,`local.php` နဲ့ `artisan.php` တို့ပါဝင်ပါတယ်။ artisan အကြောင်းအသေးစိတ်သိလိုတယ်ဆိုရင်တော့ [Artisan command line](command#registering-commands.md) ကိုဖတ်ဖို့ညွှန်းပရစေ။

Default အရ`global.php` မှာ basic items တွေပါဝင်ပါတယ်၊ registration တွေရဲ့ [logger](errors.md) တို့... နောက်  `app/filters.php` တို့လည်းပါဝင်ပါသေးတယ်။ ဒါပေမယ့်လည်း ဒီ `global.php` မှာ သင်ကြိုက်တဲ့ File တွေထက်ထည့်လို့ရပါတယ်။ တကယ်လို့ထက်ထည့်လိုက်ရင် အဲ့ဒီ့ File က  သင့် application ရဲ့ request တိုင်းမှာ auto ပါဝင်နေမှာပါ။ `local.php` file ကတော့ `local` environment မှာမှ call လုပ်မှာပါ၊
Environment configuration အကြောင်းအသေးစိတ်သိလိုတယ်ဆိုရင်တော့  [configuration](configuration.md) ကိုဖတ်ဖို့ ညွှန်းပရစေ။

ဟုတ်တာပေါ့ သင့်မှာ `local` environment တစ်ခုအပြင်အခြား environment တစ်ခုရှိတယ်ဆိုရင် အဲ့ဒီ့ environment အတွက် start file တစ်ခု create လုပ်ရမှာပေါ့။ နောက်အဲ့ဒီ့ start မှာပါတာတွေက သင်အဲ့ဒီ့ environment မှာအလုပ်လုပ်တဲ့အခါမှာ အလိုလိုပါလာမှပါ။ ဒါကြောင့် ..... ဥပမာ- သင့်မှာ `developemt` environment တစ်ခုရှပြီးတော့ `bootstrap/start.php` မှာ configre လုပ်ပြီးပြီဆိုရင် သင်အနေနဲ့ `app/start/development.php` file တစ်ခု create လုပ်ထားတယ်ဆိုရင် သင့် application က အဲ့ဒီ့ environment မှာ run ရင် `app/start/development.php` ကအလိုလိုပါဝင်နေမှာပါ။

### What To Place In Start Files

Start files ကရိုးရိုးနေရာပါဘဲ...."bootstrapping" code တွေထည့်ရတဲ့နေရာပေါ့ ။ ဥပမာ၊  View composerတို့၊ logging preferences တွေကို configure လုပ်တာတို့ PHP Setting တွေပြောင်းတာ..နဲ့အခြားလိုအပ်တာတွေကို သင့် register လုပ်ချင်ရင်လဲလုပ်နိုင်ပါတယ်။ ဘာတွေကို register လုပ်ချင်လဲဆိုတာကတော့ သင့်အပေါ်မှာဘဲမူတည်ပါတယ်။ ဟုတ်တာပေါ့ "bootstrapping code" တွေအကုန်လုံးကိုသင့်ရဲ့ start file ထဲကိုထည့်လိုက်ရင်  သင့်ရဲ့ start file တွေရှုပ်ပွကုန်မှာပေါ့။Application နည်းနည်းကြီးလာပြီဆိုရင် ဒါမှမဟုတ် သင့်ရဲ့ start files နည်းနည်းရှုပ်လာပြီလို့ခံစားရပြီဆိုရင်... bootstrapping code တွေကို [service providers](ioc#service-providers.md) တွေဆီရွှေ့လိုက်ပါ။

<a name="application-events"></a>
## Application Events

#### Registering Application Events

သင့်အနေနဲ့ pre request ၊ post request တွေစနစ်တစ်ကျသွားဖို့အတွက် before, after, finish, and shutdown application events တွေကိုသုံးရပါ့မယ်

	App::before(function($request)
	{
		//
	});

	App::after(function($request, $response)
	{
		//
	});

အဲ့ဒီ့ event တွေပေါ်မူတည်ပြီးတော့ `before` နဲ့  `after` request တွေကို တစ်လှည့်ဆီသင့် application က run မှာပါ။ ဒီ events တွေက global filtering နဲ့ global modification တွေရဲ့ responses တွေအတွက်အလွန်အသုံးဝင်ပါလိမ့်မယ်။ သင့်အနေနဲ့ အဲ့ဒါတွေကို `start` files ဒါမှမဟုတ် [service provider](ioc#service-providers.md) မှာ register လုပ်ထားနိုင်ပါတယ်။

`matched` event ပေါ်က listener တစ်ခုကိုလည်း register လုပ်နိုင်ပါတယ်၊ request အဝင်တစ်ခုနဲ့ route တစ်ခုနဲ့  matched ဖြစ်သွားပြီဆိုရင် အဲဒါက fired လုပ်လိုက်တယ် ဒါပေမယ့် အဲ့ဒီ့ route က excute ဖြစ်မသွားပါဘူး။

	Route::matched(function($route, $request)
	{
		//
	});

သင် application က client ဆီကို sent လုပ်ပြီးသွားပြီဆိုရင်  နောက်ဆုံး `finish` event ကို call လုပ်ပါတယ်။ သင် application ရဲ့နောက်ဆုံးမိနစ်လိုအပ်ချက်တွေကိုလုပ်ဖို့ဒါကနေရာကောင်းတစ်ခုပါ။ `finish` event handlers က အားလုံးပြီးသွားပြီဆိုရင် `shutdown` event ကိုချက်ချင်းခေါ်လိုက်ပါတယ်၊ ဒါကနောက်ဆုံး script အလုပ်မလုပ်ခင်  လုပ်စရာရှိတာလုပ်ထားဖို့ နောက်ဆုံးအခွင့်အရေးပါ။
