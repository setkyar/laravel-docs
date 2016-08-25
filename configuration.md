# Configuration

- [Introduction](#introduction)
- [Accessing Configuration Values](#accessing-configuration-values)
- [Environment Configuration](#environment-configuration)
    - [Determining The Current Environment](#determining-the-current-environment)
- [Configuration Caching](#configuration-caching)
- [Maintenance Mode](#maintenance-mode)

<a name="introduction"></a>
## Introduction

Laravel Framework ရဲ့ configuration files တွေအကုန်လုံးကို `config` directory ထဲမှာတွေ့နိုင်ပါတယ်။ option တစ်ခုတိုင်းဆီက document လုပ်ပြီးသားပါ၊ ဒါကြောင့်မလို့ files တွေကိုဖွင့်ကြည့်ပြီးတော့ သင့်အတွက် available ဖြစ်တဲ့ options တွေနဲ့ familiar ဖြစ်အောင်လုပ်နိုင်ပါတယ်။

<a name="accessing-configuration-values"></a>
## Accessing Configuration Values

Global `config` helper function ကိုသုံးပြီးတော့ configuration values တွေကို application ရဲ့မည်သည့်နေရာကမဆို access လုပ်နိုင်ပါတယ်။ configuration values တွေကို "dot" syntax ကိုအသုံးပြုပြီးတော့ file name နဲ့ ကိုယ်အသုံးပြုချင်တဲ့ access ကိုအသုံးပြုပြီးတော့ access လုပ်နိုင်ပါတယ်။ Default value သတ်မှတ်ပေးထားရင် configuration option မရှိရင် default value ကို return ပြန်ပါလိမ့်မယ်။

    $value = config('app.timezone');

Runtime မှာ configuration values တွေကို set လုပ်ချင်တယ်ဆိုရင် `config` helper မှာ array တစ်ခု pass ပေးရပါမယ်။

    config(['app.timezone' => 'America/Chicago']);

<a name="environment-configuration"></a>
## Environment Configuration

မတူညီတဲ့ configuration values တွေက မတူညီတဲ့ environment application တွေ run နေတဲ့အခါ အဲ့ဒါတွေကတော်တော်အသုံးဝင်ပါလိမ့်မယ်။ ဉပမာ သင့်အနေနဲ့ cache driver ကို သင့်စက်မှာတစ်ခုသုံးပေမယ့် production မှာတစ်ခုသုံးချင်ရင်သုံးချင်မှာပေါ့။ အဲ့လိုလုပ်ချင်ရင် environment based configuration ကလွယ်ကူစေပါလိမ့်မယ်။

ဒါကိုလွယ်ကူစေရန်အတွက် Laravel က Vance Lucas ရဲ့ [DotEnv](https://github.com/vlucas/phpdotenv) PHP library ကိုအသုံးပြုထားပါတယ်။ Fresh install လုပ်ထားတဲ့ Laravel မှာဆိုရင် သင့် application ရဲ့ root directory မှာ `.env.example` ဆိုတဲ့ file တစ်ခုပါဝင်ပါလိမ့်မယ်။ Laravel ကို Composer ကနေ install လုပ်ထားတယ်ဆိုရင် အဲ့ဒီ့ file ကို `.env` ဆိုပြီးအလိုအလျောက်အမည်ပြောင်းပေးပါလိမ့်မယ်။ အဲ့လိုမှမဖြစ်ဘူးဆိုရင် သင်ကိုယ်တိုင်ပြောင်းပေးသင့်ပါတယ်။

#### Retrieving Environment Configuration

သင့် application က request တစ်ခုလက်ခံရှိတဲ့အခါမှာ ဒီ file မှာ listed လုပ်ထားတဲ့ variables တွေအကုန်လုံးက `$_ENV` PHP super-global မှာ loaded ဖြစ်သွားမှာပါ။ ဒါပေမယ့် ဒီ variables တွေထဲက values တွေကိုပြန်လည်ရယူဖို့ `env` helper ကိုအသုံးပြုနိုင်ပါတယ်။ တကယ်တော့ Laravel configuration ကို review လုပ်လိုက်ရင်ဒီ helper ကို options တော်တော်များများမှာအသုံးပြုပြီးတာတွေ့ရပါလိမ့်မယ်။

    'debug' => env('APP_DEBUG', false),

`env` function ကို pass ပေးတဲ့ဒုတိယ value က "default value" ဖြစ်ပါတယ်။ ပေးထားတဲ့ key အတွက် environment variable မရှိလို့ရှိရင် အဲ့ဒီ့ value ကိုသုံးပါလိမ့်တယ်။ 

သင့်ရဲ့ `.env` file ကိုသင့် application ရဲ့ source control မှာ commit မလုပ်သင့်ပါဘူး။ ဘာကြောင့်လည်းဆိုရင် developer/ server တိုင်းမှာမတူညီတဲ့ configuration တွေကိုလိုအပ်နိုင်လို့ပါ။

သင်က team နဲ့ developing လုပ်နေတဲ့သူဆိုရင် `.env.example` ကိုတော့သင့် application နဲ့တစ်ပါးတည်းပါစေချင်မှာပါ။ configuration file values တွေမှာ place-holder values တွေကိုထည့်ပြီးတော့ပေါ့၊ သင့် team ကအခြား developer တွေက application ကို run ဖို့ရာအတွက်ဘယ် environment variables ကိုလိုအပ်တယ်ဆိုတာရှင်းရှင်းလင်းလင်းသိဖို့ရာအတွက်ဖြစ်ပါတယ်။

<a name="determining-the-current-environment"></a>
### Determining The Current Environment

လက်ရှိ application environment ကို `.env` file ရဲ့ `APP_ENV` variable ကနေသတ်မှတ်ပေးပါတယ်။ `App` [facade](/docs/{{version}}/facades) `environment` method ကနေပြီးတော့အဲ့ဒီ့ value တွေကို access လုပ်နိုင်ပါတယ်။

    $environment = App::environment();

`environment` method ကို arguments pass လုပ်ပြီးတော့ environment ကပေးထားတဲ့ value နဲ့ညီလားဆိုတာကိုစစ်နိုင်ပါတယ်။ environment ကပေးထားတဲ့ value နဲ့ညီတယ်ဆိုရင် method က `true` ပြန်ပါလိမ့်မယ်။

    if (App::environment('local')) {
        // The environment is local
    }

    if (App::environment('local', 'staging')) {
        // The environment is either local OR staging...
    }

<a name="configuration-caching"></a>
## Configuration Caching

To give your application a speed boost, you should cache all of your configuration files into a single file using the `config:cache` Artisan command. This will combine all of the configuration options for your application into a single file which will be loaded quickly by the framework.

You should typically run the `php artisan config:cache` command as part of your production deployment routine. The command should not be run during local development as configuration options will frequently need to be changed during the course of your application's development.

<a name="maintenance-mode"></a>
## Maintenance Mode

When your application is in maintenance mode, a custom view will be displayed for all requests into your application. This makes it easy to "disable" your application while it is updating or when you are performing maintenance. A maintenance mode check is included in the default middleware stack for your application. If the application is in maintenance mode, a `MaintenanceModeException` will be thrown with a status code of 503.

To enable maintenance mode, simply execute the `down` Artisan command:

    php artisan down

You may also provide `message` and `retry` options to the `down` command. The `message` value may be used to display or log a custom message, while the `retry` value will be set as the `Retry-After` HTTP header's value:

    php artisan down --message='Upgrading Database' --retry=60

To disable maintenance mode, use the `up` command:

    php artisan up

#### Maintenance Mode Response Template

The default template for maintenance mode responses is located in `resources/views/errors/503.blade.php`. You are free to modify this view as needed for your application.

#### Maintenance Mode & Queues

While your application is in maintenance mode, no [queued jobs](/docs/{{version}}/queues) will be handled. The jobs will continue to be handled as normal once the application is out of maintenance mode.

#### Alternatives To Maintenance Mode

Since maintenance mode requires your application to have several seconds of downtime, consider alternatives like [Envoyer](https://envoyer.io) to accomplish zero-downtime deployment with Laravel.

