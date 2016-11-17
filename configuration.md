# Configuration လုပ်ခြင်း

- [မိတ်ဆက်](#introduction)
- [Environment ပြင်ဆင်ခြင်း](#environment-configuration)
- [Provider ပြင်ဆင်ခြင်း](#provider-configuration)
- [အမှားခံ၊ အသိခံ၍ မရသော အချက်အလက်များအား ကာကွယ်ခြင်း](#protecting-sensitive-configuration)
- [Application အားပြုပြင်ထိန်းသိမ်းမှု အခြေအနေ](#maintenance-mode)

<a name="introduction"></a>
## မိတ်ဆက်

Laravel framework မှာရှိတဲ့ configuration ဖိုင်အားလုံးကို `app/config` လမ်းကြောင်းအောက်မှာသိမ်းထားပါတယ်။ ဖိုင်အားလုံးမှာပါတဲ့ option တစ်ခုချင်းစီအတွက် documentation မှာ ရေးထားပီးသားပါ။ အသုံးပြုနိုင်တဲ့ options တွေကို documentation နဲ့တွဲပြီး လေ့လာ နိုင်ပါတယ်။

Application run နေတဲ့အချိန်တွေမှာ configuration values တွေကို အသုံးပြုဖို့လိုအပ်လာရင် `Config` class ကိုအသုံးပြုပြီး ဆွဲယူနိုင်ပါတယ်။

#### Configuration Value များကို အသုံးပြုခြင်း

	Config::get('app.timezone');

ဆွဲယူအသုံးပြုလိုက်တဲ့ configuration option မရှိတဲ့အခြေအနေအတွက် default value ကို return ပြန်အောင် သတ်မှတ်ပေးထားနိုင်ပါတယ်။

	$timezone = Config::get('app.timezone', 'UTC');

#### Configuration value သတ်မှတ်ခြင်း

Configuration ဖိုင်တွေထဲမှာရှိတဲ့ value တွေကို "dot” ကိုအသုံးပြုပြီး (eg. filename.value) access လုပ်နိုင်ပါတယ်။ Application run-time ကာလမှာ configuration တွေသတ်မှတ်ဖို့အတွက်လည်း အသုံးပြုနိုင်ပါတယ်။

	Config::set('database.default', 'sqlite');

Applicastion run-time ကာလမှာ သတ်မှတ်ထားတဲ့ configuration values တွေဟာ app ရဲ့ လက်ရှိ request အပေါ်မှာပဲသက်ရောက်မှုရှိပါတယ်။ နောက်ပိုင်းထပ်ဖြစ်လာမဲ့ requests တွေအထိ ယူဆောင်သွားမှာမဟုတ်ပါဘူး

<a name="environment-configuration"></a>
## Environment ပြင်ဆင်ခြင်း

Application run နေတဲ့ environment အပေါ်အခြေခံပီး configuration ဖိုင်တွေ သတ်မှတ်ထားခြင်းဟာ အထောက်အကူ အများကြီးဖြစ်စေပါတယ်။ ဥပမာ - ကိုယ့်ရဲ့ local machine ပေါ်မှာ မတူညီတဲ့ cache driver တွေအသုံးပြုချင်တယ်ဆိုရင် ဒီ environment based configuration ကိုအသုံးပြုရုံနဲ့ လွယ်ကူ ပြီးမြောက်စေနိုင်ပါတယ်။ 

`config` ဖိုဒါထဲမှာ ကိုယ့်ရဲ့ environmen  လိုက်ဖက်မဲ့ directory တစ်ခုကို ဆောက်လိုက်ပါ။ ဥပမာ - `local`။ ပြီးရင် အဲ့ဒီ environment အတွက် override လုပ်သွားမဲ့ config တွေ၊ ထပ်မံသတ်မှတ်ချင်တဲ့ options တွေကို configuration ဖိုင်တွေပြုလုပ်ပီးသတ်မှတ်နိုင်ပါပြီ။ ဥပမာ - local environment အတွက် cache driver ကို override လုပ်ချင်တယ်ဆိုရင်၊ `app/config/local` ဖိုဒါထဲမှာ `cache.php` ဖိုင်ဆောက်ပီး အောက်မှာပေးထားတဲ့ code တွေနဲ့ ပြုလုပ်လိုက်ပါ။ 

	<?php

	return array(

		'driver' => 'file',

	);

> **သတိပြုရန်:** `testing` ဆိုတဲ့ အမည်နဲ့ environment name ကို မသတ်မှတ်ပါနဲ့။ အဲ့ဒီအမည်ဟာ unit testing အတွက် သီးသန့်သတ်မှတ်ထားတဲ့ အမည်ဖြစ်ပါတယ်။

base configuration ဖိုင်မှာပါတဲ့ option အားလုံးကို ပြန်လည်သတ်မှတ်ပေးဖို့ မလိုအပ်ပါ လိုအပ်ပြီး ကိုယ့်အနေနဲ့ override လုပ်ချင်တဲ့ option တွေကိုသာသတ်မှတ်ပေးရန်။ Base configuration files တွေကို environment configuration files တွေက "cascade” လုပ်သွားပါလိမ့်မယ်။

ပြီးရင်တော့ ဘယ် environment မှာ run နေတယ်ဆိုတာ framework ကနေ သိနိုင်ဖို့အတွက် သတ်မှတ်ထားပေးရမှာပါ။ Default environment ကတော့ `production` ဖြစ်ပါတယ်။ အခြား environment တွေအတွက် setup ပြုလုပ်ရမဲ့ နေရာက root directory အောက်မှာရှိတဲ့ `bootstrap/start.php` ဖိုင်ထဲမှာပြုလုပ်ပေးရပါမယ်။ အဲ့ဒီဖိုင်ထဲမှာရှိတဲ့ `$app->detectEnvironment` ဆိုတဲ့  method ထဲကို သတ်မှတ်ထားတဲ့ environment တွေပါတဲ့ array တစ်ခု passing လုပ်ထားပါတယ်။ အဲ့ဒီ array ကိုအသုံးပြုပြီး လက်ရှိ environment ကို ဆုံးဖြတ်တာဖြစ်ပါတယ်။ လိုအပ်လာလို့ရှိရင် အဲ့ဒီ array ထဲကို နောက်ထပ် environment တွေ ထပ်ထည့်နိုင်ပါတယ်။

    <?php

    $env = $app->detectEnvironment(array(

        'local' => array('your-machine-name'),

    ));

အပေါ်မှာပြထားတဲ့ ဥပမာမှာ `local` က environment အမည်ဖြစ်ပြီး `your-machine-name` က server ရဲ့ hostname ဖြစ်ပါတယ်။ Linux နဲ့ Mac ကွန်ပျူတာတွေမှာဆိုရင် `hostname` ဆိုတဲ့ terminal command ကိုအသုံးပြုပြီး hostname ကိုသတ်မှတ်ပေးနိုင်ပါတယ်။

အကယ်၍ ပိုပြီးထိရောက်တဲ့ environment သိရှိမှုကို လိုအပ်တယ်ဆိုရင်တော့ `detectEnvironment` method ထဲကို ကိုယ်လိုအပ်သလိုအသုံးပြုနိုင်တဲ့ environment သိရှိမှုတွေကိုပြုလုပ်ပေးနိုင်မဲ့ `Closure` တစ်ခုကို passing ပေးဖို့လိုအပ်ပါတယ်။

	$env = $app->detectEnvironment(function()
	{
		return $_SERVER['MY_LARAVEL_ENV'];
	});

#### Application ရဲ့ လက်ရှိ Environment ကိုအသုံးပြုခြင်း။

Application ရဲ့ လက်ရှိအသုံးပြုနေတဲ့ environment ကို `environment` method ကိုအသုံးပြုပြီး ရယူနိုင်ပါတယ်။

	$environment = App::environment();

ကိုယ်အသုံးပြုချင်တဲ့ environment ဟုတ်/မဟုတ် ကိုလည်း `environment` method ထဲကို arguments တွေ passing ပေးပြီး စစ်ကြည့်နိုင်ပါတယ်။

	if (App::environment('local'))
	{
		// Local environment ဖြစ်တယ်
	}

	if (App::environment('local', 'staging'))
	{
		//Local သို့မဟုတ် staging environment ဖြစ်တယ်
	}

<a name="provider-configuration"></a>
### Provider ပြင်ဆင်ခြင်း

Environment configuration ကို အသုံးပြုပြီဆိုလို့ရှိရင်၊ ကိုယ့်ရဲ့ ပင်မ `app` configuration ဖိုင်ထဲမှာ environment [service providers](ioc#service-providers.md) ကိုထည့်ပေါင်းထည့်ဖို့ လိုအပ်လာတဲ့ အခြေအနေတွေ ရှိလာနိုင်ပါတယ်။ အကယ်၍ ကိုယ်က ထပ်ပေါင်းထည့်ထားတယ်ဆိုလို့ရှိရင်, the environment `app` providers are overriding the providers in your primary `app` configuration file ဆိုပြီး သတိပေးပါလိမ့်မယ်။ အဲ့လိုအခြေအနေမျိုးမှာ providers ကို မရမကထပ်ပေါင်းထည့်စေဖို့အတွက် `append_config` ဆိုတဲ့ helper method ကို ကိုယ့်ရဲ့ environment `app` configuration ဖိုင်ထဲမှာ အသုံးပြုနိုင်ပါတယ်။

	'providers' => append_config(array(
		'LocalOnlyServiceProvider',
	))

<a name="protecting-sensitive-configuration"></a>
## အမှားခံ၊ အသိခံ၍ မရသော အချက်အလက်များအား ကာကွယ်ခြင်း

အမှန်တကယ်အသုံးပြုမဲ့ application တွေအတွက်၊ ကိုယ့်ရဲ့ အမှားမခံ၊ အသိခံလို့ မရတဲ့ configuration တွေကို configuration ဖိုင်ထဲမှာ မသိမ်းပဲနဲ့ အခြားတစ်နေရာမှာထားတာက ပိုပြီးသင့်တော်ပါတယ်။ ဘယ်လိုအမျိုးအစားတွေလဲဆိုတော့ database passwords, Stripe API keys, and encryption keys စတာတွေကို ဖြစ်နိုင်လို့ရှိရင် configuration ဖိုင်ထဲမှာမသိမ်းသင့်ပါဘူး။ ဒါဆိုဘယ်နေရာမှာသိမ်းမလဲ? အဲ့ဒီအတွက် Laravel ကဖြေရှင်းပေးပြီးသားဖြစ်ပါတယ်။ အဲ့ဒီလို configuration အမျိုးအစားတွေအတွက် "dot" files တွေကိုအသုံးပြုပြီး ကာကွယ်ထားနိုင်ပါတယ်။

ပထမဆုံးအနေနဲ့ ကိုယ့်ရဲ့စက်ဟာ local မှာ run နေတာပါဆိုတာကို application ကသိအောင် [configure](configuration#environment-configuration.md) လုပ်ပေးရပါမယ်။ ပြီးရင် `.env.local.php` ဆိုတဲ့ ဖိုင်အသစ်ကို `composer.json` ဖိုင်ရှိတဲ့ ဖိုဒါအောက်မှာ ဆောက်ပေးလိုက်ပါ။ အဲ့ဒီ `.env.local.php` ဖိုင်ဟာ အခြား laravel configuration ဖိုင်တွေလိုပဲ key-value pairs ဖြစ်တဲ့ array တစ်ခု return ပြန်ရပါမယ်။ 

	<?php

	return array(

		'TEST_STRIPE_KEY' => 'super-secret-sauce',

	);

အဲ့ဒီ ဖိုင်ထဲကနေ return ပြန်လာတဲ့ key-value pairs တွေဟာ PHP "superglobals" တွေဖြစ်တဲ့ `$_ENV` နဲ့ `$_SERVER` တွေဆီကို auto ရောက်သွားပါလိမ့်မယ်။ အဲ့ဒီ "superglobals" တွေကနေတစ်ဆင့် ကိုယ့်ရဲ့ configuration ဖိုင်ထဲမှာ ပြန်လည်အသုံးပြုနိုင်ပြီဖြစ်ပါတယ်။ 

	'key' => $_ENV['TEST_STRIPE_KEY']

သေချာအောင်လုပ်ဖို့လိုအပ်တာတစ်ခုက အဲ့ဒီ `.env.local.php` ဖိုင်ကို `.gitignore` လုပ်ထားပေးရပါမယ်။ အဲ့ဒီတော့မှ ကိုယ့်ရဲ့ team မှာရှိတဲ့ကျန်တဲ့ developers တွေဟာ သူတို့ရဲ့ ကိုယ်ပိုင် local configuration တွေကိုပြုလုပ်နိုင်မည့်အပြင် ကိုယ့်ရဲ့ sensitive configuration တွေကိုလဲ source control မှာမပါအောင် ကာကွယ်ပြီးသားဖြစ်မှာပါ။

Production environment အတွက်လည်း လိုအပ်တဲ့ configuration တွေပါတဲ့ `.env.php` ဖိုင်ကို project root ဖိုဒါထဲမှာ ဆောက်လိုက်ပါ။ `.env.local.php` ဖိုင်လိုပဲ production environment မှာ အသုံးပြုမဲ့`.env.php` ဖိုင်ဟာ source control ထဲမှာ မပါသင့်ပါဘူး။

> **သတိပြုရန်:** Application ကနေ support လုပ်တဲ့ environment တစ်ခုချင်းစီအတွက် `.env` ဖိုင်တွေ တည်ဆောက်လာနိုင်ပါတယ်။ ဥပမာ - `development` environment အတွက်ဆိုရင် `.env.development.php` ဖိုင်က ရှိနေလို့ရှိရင် load လုပ်သွားပါလိမ့်မယ်။ 

<a name="maintenance-mode"></a>
## Application အားပြုပြင်ထိန်းသိမ်းမှုအခြေအနေ

Application ဟာ ပြုပြင်ထိန်းသိမ်းမှု ပြုလုပ်တဲ့ အခြေအနေမှာ ရှိနေမယ်ဆိုရင် application မှာရှိတဲ့ route အားလုံးအတွက် ကြိုတင်ပြုလုပ်ထားနိုင်တဲ့ စိတ်ကြိုက် မြင်ကွင်း(view) ကိုပြပေးပါလိမ့်မယ်။ ပြုပြင်ထိန်းသိမ်းမှုပြုလုပ်နေရင်ပဲဖြစ်ဖြစ်၊ update လုပ်နေရင်ပဲဖြစ်ဖြစ် application ကို လွယ်လွယ်ကူကူပဲ disable လုပ်ထားနိုင်ပါတယ်။ `app/start/global.php` ဖိုင်ထဲမှာရှိပြီးသားဖြစ်တဲ့ `App::down` ဆိုတဲ့ method ကိုခေါ်သုံးလိုက်ရုံပဲ။ အဲ့ဒီ method ကနေပြန်လာတဲ့ response ကို users တွေဆီကိုပို့ပေးပါလိမ့်မယ်။

ပြုပြင်ထိန်းသိမ်းမှုပြုလုပ်နေပါတယ်ဆိုတဲ့ အခြေအနေကိုထားချင်တယ်ဆိုရင် `down` ဆိုတဲ့ Artisan command ကို အသုံးပြုနိုင်ပါတယ်။

	php artisan down

ထိန်းသိမ်းမှုပြုလုပ်ပြီးသွားလို့ application ကိုပြန်ပြီး အသက်သွင်းချင်ရင် `up` ဆိုတဲ့ Artisan command ကို အသုံးပြုနိုင်ပါတယ်။

	php artisan up

ထိန်းသိမ်းမှုပြုလုပ်နေတဲ့အခြေအနေအတွက် စိတ်ကြိုက် မြင်ကွင်း (view) သတ်မှတ်ချင်တယ်ဆိုရင်တော့ အောက်မှာပြထားသလိုပဲ `app/start/global.php` ဖိုင်ထဲမှာ နှစ်သက်သလို သွားရောက်ပြင်ဆင်နိုင်ပါတယ်။

	App::down(function()
	{
		return Response::view('maintenance', array(), 503);
	});

အကယ်၍ `down` method ထဲကို Closure တစ်ခု passing ပေးလိုက်ရင်တော့ `NULL` ပဲ return ပြန်လာပြီး အဲ့ဒီ request မှာပါတဲ့ maintenance mode ကို ignore လုပ်သွားပါလိမ့်မယ်။

### Maintenance Mode နှင့် Queues

Application ဟာ maintenance mode မှာ ရှိနေစဉ်အတွင်း [queue jobs](queues.md) တွေကို ကိုင်တွယ်ဖြေရှင်းမှာမဟုတ်ပါဘူး။ Application ဟာ ပုံမှန်အခြေအနေ ကိုပြန်ရောက်ပီဆိုတော့မှ ပြန်လည်ကိုင်တွယ်ဖြေရှင်းပေးမှာဖြစ်ပါတယ်။