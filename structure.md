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


The `resources` directory contains your views as well as your raw, un-compiled assets such as LESS, SASS, or JavaScript. This directory also houses all of your language files.

<a name="the-routes-directory"></a>
#### The Routes Directory

The `routes` directory contains all of the route definitions for your application. By default, two route files are included with Laravel: `web.php` and `api.php`. The `web.php` file contains routes that the `RouteServiceProvider` places in the `web` middleware group, which provides session state, CSRF protection, and cookie encryption. If your application does not offer a stateless, RESTful API, all of your routes will most likely be defined in the `web.php` file.

The `api.php` file contains routes that the `RouteServiceProvider` places in the `api` middleware group, which provides rate limiting. These routes are intended to be stateless, so requests entering the application through these routes are intended to be authenticated via tokens and will not have access to session state.

<a name="the-storage-directory"></a>
#### The Storage Directory

The `storage` directory contains your compiled Blade templates, file based sessions, file caches, and other files generated by the framework. This directory is segregated into `app`, `framework`, and `logs` directories. The `app` directory may be used to store any files generated by your application. The `framework` directory is used to store framework generated files and caches. Finally, the `logs` directory contains your application's log files.

The `storage/app/public` directory may be used to store user-generated files, such as profile avatars, that should be publicly accessible. You should create a symbolic link at `public/storage` which points to this directory. You may create the link using the `php artisan storage:link` command.

<a name="the-tests-directory"></a>
#### The Tests Directory

The `tests` directory contains your automated tests. An example [PHPUnit](https://phpunit.de/) is provided out of the box. Each test class should be suffixed with the word `Test`. You may run your tests using the `phpunit` or `php vendor/bin/phpunit` commands.

<a name="the-vendor-directory"></a>
#### The Vendor Directory

The `vendor` directory contains your [Composer](https://getcomposer.org) dependencies.

<a name="the-app-directory"></a>
## The App Directory

The majority of your application is housed in the `app` directory. By default, this directory is namespaced under `App` and is autoloaded by Composer using the [PSR-4 autoloading standard](http://www.php-fig.org/psr/psr-4/).

The `app` directory contains a variety of additional directories such as `Console`, `Http`, and `Providers`. Think of the `Console` and `Http` directories as providing an API into the core of your application. The HTTP protocol and CLI are both mechanisms to interact with your application, but do not actually contain application logic. In other words, they are simply two ways of issuing commands to your application. The `Console` directory contains all of your Artisan commands, while the `Http` directory contains your controllers, middleware, and requests.

A variety of other directories will be generated inside the `app` directory as you use the `make` Artisan commands to generate classes. So, for example, the `app/Jobs` directory will not exist until you execute the `make:job` Artisan command to generate a job class.

> {tip} Many of the classes in the `app` directory can be generated by Artisan via commands. To review the available commands, run the `php artisan list make` command in your terminal.

<a name="the-console-directory"></a>
#### The Console Directory

The `Console` directory contains all of the custom Artisan commands for your application. These commands may be generate using the `make:command` command. This directory also houses your console kernel, which is where your custom Artisan commands are registered and your [scheduled tasks](/docs/{{version}}/scheduling) are defined.

<a name="the-events-directory"></a>
#### The Events Directory

This directory does not exist by default, but will be created for you by the `event:generate` and `event:make` Artisan commands. The `Events` directory, as you might expect, houses [event classes](/docs/{{version}}/events). Events may be used to alert other parts of your application that a given action has occurred, providing a great deal of flexibility and decoupling.

<a name="the-exceptions-directory"></a>
#### The Exceptions Directory

The `Exceptions` directory contains your application's exception handler and is also a good place to place any exceptions thrown by your application. If you would like to customize how your exceptions are logged or rendered, you should modify the `Handler` class in this directory.

<a name="the-http-directory"></a>
#### The Http Directory

The `Http` directory contains your controllers, middleware, and form requests. Almost all of the logic to handle requests entering your application will be placed in this directory.

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
