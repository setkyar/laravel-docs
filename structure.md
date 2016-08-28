# Directory Structure

-[Introduction](#introduction)
- [The Root Directory](#the-root-directory)
    - [The `app` Directory](#the-root-app-directory)
    - [The `bootstrap` Directory](#the-bootstrap-directory)
    - [The `config` Directory](#the-config-directory)
    - [The `database` Directory](#the-database-directory)
    - [The `public` Directory](#the-public-directory)
    - [The `resources` Directory](#the-resources-directory)
    - [The `routes` Directory](#the-routes-directory)
    - [The `storage` Directory](#the-storage-directory)
    - [The `tests` Directory](#the-tests-directory)
    - [The `vendor` Directory](#the-vendor-directory)
- [The App Directory](#the-app-directory)
    - [The `Console` Directory](#the-console-directory)
    - [The `Events` Directory](#the-events-directory)
    - [The `Exceptions` Directory](#the-exceptions-directory)
    - [The `Http` Directory](#the-http-directory)
    - [The `Jobs` Directory](#the-jobs-directory)
    - [The `Listeners` Directory](#the-listeners-directory)
    - [The `Mail` Directory](#the-mail-directory)
    - [The `Notifications` Directory](#the-notifications-directory)
    - [The `Policies` Directory](#the-policies-directory)
    - [The `Providers` Directory](#the-providers-directory)

<a name="introduction"></a>
## Introduction

Laravel ရဲ့ ဖိုဒါခွဲထားတဲ့ပုံစံဟာ application အကြီးအသေးအားလုံးကို လွယ်လွယ်ကူကူ အစပျိုးလို့ရအောင် ရည်ရွယ်ပါတယ်။ မူလ ပုံစံကိုမကြိုက်ဘူးဆိုရင်လဲ စိတ်ကြိုက်ပြုပြင်ပြောင်းလဲနိုင် ပါတယ်။ Composer က autoload လုပ်ပေးနိုင်တဲ့အတွက် Laravel ရဲ့ class အားလုံးနီးပါးကို ဘယ်နေရာမှာ ရှိမှရမယ်ဆိုတာမျိုး ကန့်သတ်မထားပါဘူး။ 

#### Where Is The Models Directory?

Laravel နဲ့အခုမှစရေးမဲ့ Developer အများစုဟာ `models` directory မရှိတာကိုဇဝေဇဝါဖြစ်နိုင်ပါတယ်။ ဘာကြောင့်မထည့်ထားတာလဲဆိုတော့ အဲ့ဒီ "models" ဆိုတဲ့အသုံးအနှုန်းဟာ developer များစုအတွက် မတူညီတဲ့ အဓိပ္ပါယ်တွေ ရှိနေလို့ဖြစ်ပါတယ်။ အချို့ developser တွေအတွက် application ရဲ့ "models" ဆိုတာ အဲ့ဒီ application ရဲ့ business logic ကြီးတစ်ခုလုံးကို ရည်ရွယ်ကြသလို အချို့တွေအတွက်တော့ "models" ဆိုတဲ့အသုံးအနှု
န်းဟာ  relational database table တွေကိုချိတ်ဆက်ဆောင်ရွက်ပေးမဲ့ class တွေအဖြစ် သတ်မှတ်ကြလို့ပဲဖြစ်ပါတယ်။      
 
ဒါကြောင့်မို့လို့ Eloquent model တွေကို default အနေနဲ့ `app` အောက်မှာထားပြီး developer ကို စိတ်ကြိုက်နေရာပြောင်းရွေ့ခွင့်ပေးထားပါတယ်။ 

<a name="the-root-directory"></a>
## The Root Directory

<a name="the-root-app-directory"></a>
#### The App Directory

`app` directory ထဲမှာတော့ သင်ပြုလုပ်မဲ့ application အတွက်လိုအပ်မဲ့ အဓိက code တွေပါဝင်ပါတယ်။ သင့် application အတွက်အသုံးပြုမဲ့ class တွေအားလုံးနီးပါးဟာ ဒီ directory အောက်မှာရေးရပါလိမ့်မယ်။ ဒီ directory နဲ့ပတ်သက်ပြီး နောက်ပိုင်းမှာပိုပြီး အသေးစိတ်ရှင်းပြထားပါတယ်။ 

<a name="the-bootstrap-directory"></a>
#### The Bootstrap Directory

`bootstrap` directory ထဲမှာတော့ framework စတင်ချိန်မှာပြုလုပ်မဲ့ autoloading တွေ bootstraping ပြုလုပ်တဲ့ကိစ္စတွေပါဝင်ပါတယ်။ application performance အတွက် framework က အလိုအလျောက်ပြုလုပ်တဲ့ route တို့ services တို့ရဲ့ cache file တွေကိုလဲ ဒီ directory အောက်မှာပဲ `cache` directory တစ်ခုပြုလုပ်ပီး သိမ်းဆည်းထားပါတယ်။    
 
<a name="the-config-directory"></a>
#### The Config Directory

`config` directory ဆိုတဲ့အတိုင်း application ရဲ့ configuration တွေပါဝင်တဲ့ directory ပါ။ ဒီ directory ထဲမှာပါတဲ့ ဖိုင်တွေကို ဖတ်ပြီး application အတွက် အသုံးပြုနိုင်တဲ့ options တွေနဲ့ လွယ်လွယ်ကူကူပဲ ရင်းနှီးသွားစေနိုင်ပါတယ်။ 

<a name="the-database-directory"></a>
#### The Database Directory

`database` directory ဟာ database migration ဖိုင်တွေ၊ seeder ဖိုင်တွေအတွက်နေရာပါ။ အကယ်၍ SQLite ကိုအသုံးပြုမယ်ဆိုရင်လဲ ဒီ directory အောက်မှာပဲ SQLite database ဖိုင်တွေကို သိမ်းဆည်းနိုင်ပါတယ်။ 

<a name="the-public-directory"></a>
#### The Public Directory

`public` directory အောက်မှာ `index.php` ဖိုင်ရှိပါတယ်။ အဲ့ဒီဖိုင်းဟာ သင့် application ကို ဝင်ရောက်လာမဲ့ request အားလုံးရဲ့ ဝင်ပေါက်ပါပဲ။ ဒါ့အပြင် ပုံတွေ၊ JavaScript ဖိုင်တွေနဲ့ CSS ဖိုင်တွေကိုလည်း ဒီ directory အောက်မှာပဲ သိမ်းဆည်းပါတယ်။ 

<a name="the-resources-directory"></a>
#### The Resources Directory

`resources` directory ထဲမှာတော့ view ဖိုင်တွေရယ်၊ compiled မလုပ်ရသေးတဲ့ LESS, SASS, Javascript ဖိုင်တွေပါဝင်ပါတယ်။ ဒါ့အပြင် language ဖိုင်တွေအားလုံးကိုလဲ ဒီထဲမှာပဲသိမ်းဆည်းပါတယ်။ 

<a name="the-routes-directory"></a>
#### The Routes Directory

`routes` directory ထဲမှာ application ရဲ့ route definitions အားလုံးရှိပါမယ်။ ပုံမှန်အနေနဲ့တော့ `web.php`၊ `api.php` ရယ် `console.php` ရယ် သုံးဖိုင်ရှိပါတယ်။

`web.php` ဖိုင်ထဲမှာပါတဲ့ routes တွေကို RouteServiceProvider ကနေပီးတော့ session state, CSRF protection နဲ့ cookie encryption တွေလုပ်ဆောင်ပေးမဲ့  `web` middleware group ထဲမှာနေရချပေးပါတယ်။ အကယ်၍ သင့် application ဟာ stateless၊ RESTful API ဖြစ်ဖို့မရည်ရွယ်ဘူးဆိုရင် သင့် routes တွေအားလုံးကို `web.php` ဖိုင်ထဲမှာရေးရပါလိမ့်မယ်။ 

`api.php` ဖိုင်ထဲမှာပါတဲ့ routes တွေကိုတော့ RouteServiceProvider ကနေ ကန်သတ်ချက်တွေရှိတဲ့ `api` middleware group ထဲမှာနေရာချပါတယ်။ ဘာကန့်သတ်ချက်တွေရှိလဲဆိုတော့ အဲ့ဒီ middleware အောက်မှာရှိတဲ့ routes တွေဟာ stateless ဖြစ်ပြီး session သုံးလို့မရပါဘူး။  ဒါကြောင့်မို့ ဒီ routes တွေကနေ  လုပ်တဲ့ requests တွေဟာ tokens ကနေတဆင့် authentication လုပ်ဖို့လိုအပ်ပါတယ်။ 

The console.php file is where you may define all of your Closure based console commands. Each Closure is bound to a command instance allowing a simple approach to interacting with each command's IO methods. Even though this file does not define HTTP routes, it defines console based entry points (routes) into your application.

<a name="the-storage-directory"></a>
#### The Storage Directory

`storate` directory ထဲမှာတော့ compiled လုပ်ပီးသား Blade templates ဖိုင်တွေ၊ file based sessions တွေ၊ file caches တွေနဲ့ framework ကနေပြုလုပ်တဲ့ files တွေအားလုံးရှိပါတယ်။ သူအောက်မှာမှ `app`၊ `framework` နဲ့ `logs` ဆိုပြီး နောက်ထပ် directory သုံးခုထပ်ခွဲပါတယ်။ `app` directory ကတော့ သင့် application ကနေ generate လုပ်လိုက်တဲ့ ဖိုင်တွေကို ထားပါတယ်။ `framework` directory ထဲမှာတော့ framework ကနေ generate လုပ်တဲ့ဖိုင်တွေရယ် cache ဖိုင်တွေရယ်ကို သိမ်းပါတယ်။ နောက်ဆုံးတစ်ခုဖြစ်တဲ့ `logs` directory ထဲမှာတော့ သင့် application ရဲ့ log ဖိုင်တွေရှိပါမယ်။ 

`storage/app/public` directory အောက်မှာတော့ profile avatars လိုမျိုး user-generated ဖိုင်တွေထားပါတယ်။ အဲ့ဒီဖိုင်တွေဟာ public ကနေယူသုံးလို့ရတဲ့ ဖိုင်မျိုးဖြစ်သင့်ပါတယ်။ ပြီးတော့ `public/storage`  directory နေ ဒီ `storage/app/public` directory ကို symbolic link ပြုလုပ်ထားသင့်ပါတယ်။ လွယ်လွယ်ကူကူပဲ `php artisan storage:link` command နဲ့ အဲ့ဒီ link ကို ပြုလုပ်နိုင်ပါတယ်။ 

<a name="the-tests-directory"></a>
#### The Tests Directory

The `tests` directory contains your automated tests. An example [PHPUnit](https://phpunit.de/) is provided out of the box. Each test class should be suffixed with the word `Test`. You may run your tests using the `phpunit` or `php vendor/bin/phpunit` commands.

<a name="the-vendor-directory"></a>
#### The Vendor Directory

[Composer](https://getcomposer.org) dependencies တွေအားလုံး `vendors` directory ထဲမှာရှိပါတယ်။ 

<a name="the-app-directory"></a>
## The App Directory

သင့် application အတွက် အဓိကအရေးကြီးတဲ့ အရာတွေက `app` directory အောက်မှာရှိပါတယ်။ ပုံမှန်အနေနဲ့ ဒီ directory ဟာ `App` ဆိုတဲ့ namespace အောက်မှာရှိပြီး Composer ကနေ [PSR-4 autoloading standard](http://www.php-fig.org/psr/psr-4/) ကိုသုံးပြီး autoload လုပ်ပေးထားပါတယ်။ 

`app` directory အောက်မှာ `'Console`၊ `Http` နဲ့ `Providers` တို့လို နောက်ထပ် sub-directory တွေအမျိုးမျိုးရှိပါသေးတယ်။ Think of the `Console` and `Http` directories as providing an API into the core of your application. HTTP protocol နဲ့ CLI နှစ်ခုစလုံးကို သင့် application အတွက် အလုပ်လုပ်ပေးမဲ့ ယန္တယားအဖြစ်သတ်မှတ်နိုင်ပါတယ်။ ဒါပေမဲ့ တကယ် application logic ကမပါဝင်ပါဘူး။ နောက်ပုံစံတစ်မျိုးနဲ့ ပြောရရင် သူတို့ဟာ application ကို ခိုင်းစေမဲ့ နည်းလမ်းနှစ်ခုအဖြစ် သတ်မှတ်နိုင်ပါတယ်။ `Console` directory ထဲမှာတော့ သင့်ရဲ့ Artisan commands တွေပါဝင်မှာဖြစ်ပြီး၊ `Http` directory ထဲမှာတော့ သင့်ရဲ့ controllers တွေ middleware တွေ နဲ့ requests တွေပါဝင်မှာဖြစ်ပါတယ်။

ကျန်တဲ့ directory တွေကတော့ `make` Artisan command တွေကို သုံးပြီး classes တွေ generate လုပ်တော့မှ အလိုအလျောက် ပြုလုပ်သွားမှာပါ။ ဥပမာ `app/Jobs` directory `make:job` Artisan command နဲ့ job class create မလုပ်မချင်း ရှိအုံးမှာမဟုတ်ပါဘူး။ 

> {tip} `app` directory အောက်မှာရှိတဲ့ classes တော်တော်များများဟာ Aritsan commands တွေအသုံးပြုပြီး generate ပြုလုပ်နိုင်ပါတယ်။ အသုံးပြုနိုင်တဲ့ commands တွေကိုကြည့်ဖို့အတွက် terminal ကနေ `php artisan list make` command ကိုအသုံးပြုပြီး ကြည့်ရှုနိုင်ပါတယ်။ 

<a name="the-console-directory"></a>
#### The Console Directory

သင့် application အတွက်လိုပြုလုပ်မဲ့ custom Artisan commands တွေဟာ `Console` directory ထဲမှာရှိပါမယ်။ အဲ့ဒီ custom commands တွေကို `make:command` ကိုအသုံးပြုပြီး generate ပြုလုပ်နိုင်ပါတယ်။ သင့်ရဲ့ custom Artisan commands တွေကို register လုပ်ဖို့ရယ်၊ သင့်ရဲ့ [scheduled tasks](/docs/{{version}}/scheduling) တွေသတ်မှတ်ဖို့အတွက် console kernal ဟာလဲ ဒီ directory အောက်မှာရှိပါမယ်။ 

<a name="the-events-directory"></a>
#### The Events Directory

`Events` directory ဟာ `event:generate` နဲ့ `event:make` Artisan command တွေအသုံးပြုပြီးမှပေါ်လာမှာပါ။ ထုံးစံအတိုင်းပဲ [event classes](/docs/{{version}}/events) တွေသိမ်းထားတဲ့ directory ပဲဖြစ်ပါတယ် ။ Events ဆိုတာ သူ့ကို အသုံးပြုပြီး သတ်မှတ်ထားတဲ့ action တွေဖြစ်ပေါ်လာတဲ့အချိန်မှာ  သင့် application ရဲ့ကျန်တဲ့အစိတ်အပိုင်းတွေကို လိုသလို ခိုင်းစေနိုင်ဖို့ အသုံးပြုပါတယ်။ flexiblility နဲ့ decoupling တို့အတွက် အရမ်းအကျိုးရှိနိုင်ပါတယ်။ 

<a name="the-exceptions-directory"></a>
#### The Exceptions Directory

The `Exceptions` directory contains your application's exception handler and is also a good place to place any exceptions thrown by your application. If you would like to customize how your exceptions are logged or rendered, you should modify the `Handler` class in this directory.

<a name="the-http-directory"></a>
#### The Http Directory

`Http` directory ထဲမှာတော့ သင့်ရဲ့ contollers တွေ၊ middleware တွေနဲ့ form request လုပ်တဲ့ အရာတွေပါဝင်ပါတယ်။ သင့် application ကို ဝင်ရောက်လာမဲ့ requests တွေကို ထိန်းချုပ်မဲ့ logic အားလုံးနီးပါးဟာ ဒီ direcoty ထဲမှာပဲလုပ်ဆောင်ပါတယ်။ 

<a name="the-jobs-directory"></a>
#### The Jobs Directory

This directory does not exist by default, but will be created for you if you execute the `make:job` Artisan command. The `Jobs` directory houses the [queueable jobs](/docs/{{version}}/queues) for your application. Jobs may be queued by your application or run synchronously within the current request lifecycle. Jobs that run synchronously during the current request are sometimes referred to as "commands" since they are an implementation of the [command pattern](https://en.wikipedia.org/wiki/Command_pattern).

<a name="the-listeners-directory"></a>
#### The Listeners Directory

This directory does not exist by default, but will be created for you if you execute the `event:generate` or `make:listener` Artisan commands. The `Listeners` directory contains the classes that handle your [events](/docs/{{version}}/events). Event listeners receive an event instance and perform logic in response to the event being fired. For example, a `UserRegistered` event might be handled by a `SendWelcomeEmail` listener.

<a name="the-mail-directory"></a>
#### The Mail Directory

This directory does not exist by default, but will be created for you if you execute the `make:mail` Artisan command. The `Mail` directory contains all of your classes that represent emails sent by your application. Mail objects allow you to encapsulate all of the logic of building an email in a single, simple class that may be sent using the `Mail::send` method.

<a name="the-notifications-directory"></a>
#### The Notifications Directory

This directory does not exist by default, but will be created for you if you execute the `make:notification` Artisan command. The `Notifications` directory contains all of the "transactional" notifications that are sent by your application, such as simple notifications about events that happen within your application. Laravel's notification features abstracts sending notifications over a variety of drivers such as email, Slack, SMS, or stored in a database.

<a name="the-policies-directory"></a>
#### The Policies Directory

This directory does not exist by default, but will be created for you if you execute the `make:policy` Artisan command. The `Policies` directory contains the authorization policy classes for your application. Policies are used to determine if a user can perform a given action against a resource. For more information, check out the [authorization documentation](/docs/{{version}}/authorization).

<a name="the-providers-directory"></a>
#### The Providers Directory

The `Providers` directory contains all of the [service providers](/docs/{{version}}/providers) for your application. Service providers bootstrap your application by binding services in the service container, registering events, or performing any other tasks to prepare your application for incoming requests.

In a fresh Laravel application, this directory will already contain several providers. You are free to add your own providers to this directory as needed.
