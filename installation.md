# Installation

- [Installation](#installation)
    - [Server Requirements](#server-requirements)
    - [Installing Laravel](#installing-laravel)
    - [Configuration](#configuration)

<a name="installation"></a>
## Installation

<a name="server-requirements"></a>
### Server Requirements

Laravel framework က system requirements တစ်ချို့လိုအပ်ပါတယ်။ အဲ့ဒီ့လိုအပ်တဲ့ requirements တွေကို [Laravel Homestead](/docs/{{version}}/homestead) virtual machine က စိတ်ကျေနပ်မှုပေးမှာပါ၊ ဒါကြောင့်မလို့ Homestead ကိုသင့်ရဲ့ local development environment အတွက်အသုံးပြုဖို့ရာ highly recommended လုပ်ပါတယ်။

တကယ်လို့သင့်က Homestead ကိုအသုံးမပြုဘူးဆိုရင် သင့်ရဲ့ server ကအောက်ဖော်ပြပါ requirements တွေလိုအပ်မှာဖြစ်ပါတယ် -

<div class="content-list" markdown="1">
- PHP >= 5.6.4
- OpenSSL PHP Extension
- PDO PHP Extension
- Mbstring PHP Extension
- Tokenizer PHP Extension
</div>

<a name="installing-laravel"></a>
### Installing Laravel

Laravel utilizes [Composer](http://getcomposer.org) to manage its dependencies. So, before using Laravel, make sure you have Composer installed on your machine.

#### Laravel Installer မှတစ်ဆင့်

ပထမဆုံး Composer ကိုအသုံးပြုပြီး Laravel installer ကို download လုပ်ပါ -

    composer global require "laravel/installer"

`~/.composer/vendor/bin directory` ကို PATCH directory ထဲမှာထည့်ထားရပါ့မယ် ဒါမှ `laravel` ဆိုပြီးခေါ်နိုင်ဖို့ရာအတွက် system ကသိမှာဖြစ်ပါတယ်။

Install လုပ်ပြီးသွားပြီဆိုရင် laravel new command ကိုသုံးပြီးတော့ သင်ကြိုက်တဲ့ directory မှာ Laravel ကို install လုပ်လို့ရပါပြီ။ ဥပမာအားဖြင့် laravel new blog က blog လို့အမည်ပေးထားတဲ့ folder ထဲမှာ Laravel ရဲ့ dependencies တွေကို install လုပ်ထားမှာဖြစ်ပါတယ်။

    laravel new blog

#### Via Composer Create-Project

နောက်တစ်နည်းအနေးနဲ့ Laravel ကို terminal ကနေ Composer `create-project` command run ပြီးတော့ install လုပ်နိုင်ပါတယ်။

    composer create-project --prefer-dist laravel/laravel blog

<a name="configuration"></a>
### Configuration

#### Public Directory

Laravel ကို install လုပ်ပြီးတဲ့အခါမှာတော့သင့် web server ရဲ့ document / web root က `public` directory ဖြစ်ပါတယ်။ အဲ့ဒီ့ directory က `index.php` ကသင့် application ဆီကိုဝင်လာတဲ့ HTTP requests တွေအကုန်လုံးရဲ့ front controller အဖြစ် servers လုပ်ပါတယ်။

#### Configuration Files

Laravel framework ရဲ့ configuration အကုန်လုံးကို `config` directory တစ်ခုလုံးမှာ store လုပ်ထားပါတယ်။ option တစ်ခုတိုင်းဆီက document လုပ်ပြီးသားပါ၊ ဒါကြောင့်မလို့ files တွေကိုဖွင့်ကြည့်ပြီးတော့ သင့်အတွက် available ဖြစ်တဲ့ options တွေနဲ့ familiar ဖြစ်အောင်လုပ်ပါ။

#### Directory Permissions

Laravel ကို install လုပ်ပြီးတဲ့အခါမှာ permission တစ်ချို့ configure လုပ်ပေးဖို့လိုပါတယ်။ `storage` နဲ့ `bootstrap/cache` directories တွေကတော့ web server ကနေ writable ဖြစ်ဖို့လိုပါတယ်... အဲ့လိုမဟုတ်ဘူးဆိုရင် Laravel run မှာမဟုတ်ပါဘူး။ [Homestead](/docs/{{version}}/homestead) virtual machine ကိုသုံးမယ်ဆိုရင်တော့ဒီ permissions တွေက set လုပ်ပြီးသားဖြစ်ပါလိမ့်မယ်။

#### Application Key

Laravel install လုပ်ပြီးရင်နောက်တစ်ခုလုပ်သင့်တာကတော့ application key ပါဘဲ။ သင်က Laravel ကို Composer ဒါမှမဟုတ် Laravel installer ကနေတစ်ဆင့် install လုပ်ထားတယ်ဆိုရင်တော့ ဒီ key တွေက `php artisan key:generate` ကိုသုံးပြီးတစ်ခါတည်း set လုပ်ပြီးပါပြီ။

ပုံမှန်အားဖြင့် အဲ့ဒီ့ string က ၃၂ နှစ်လုံးရှိတဲ့စကားလုံးတွေပါ။ key ကို ` .env` environment file မှာလည်း set လုပ်နိုင်ပါတယ်။ `.env.example` ကို `.env` လို့အမည်မပြောင်းရသေးရင် သင့်အနေနဲ့အခုဘဲပြောင်းသင့်ပါတယ်။ **တကယ်လို့ application key က set လုပ်မထားဘူးဆိုရင် sessions နဲ့ အခြား encrypted data တွေကလုံခြုံမှာရှိမှာမဟုတ်ပါဘူး**

#### Additional Configuration

Laravel ကအခြား configuration တွေမလိုသလောက်ပါဘဲ။ သင့် developly လုပ်ချင်ရင်လုပ်နိုင်ပါပြီ။ သို့ရာတွင်လည်း သင့်အနေနဲ့ `config/app.php` file နဲ့ သူ့ documencation ကို review လုပ်ချင်ပါလိမ့်မယ်။ အဲဒီ့မှာ `timezone` နဲ့ `locale` options တွေများစွာပါဝင်ပါတယ်၊ သင့် application လိုအပ်ချက်အရအဲ့ဒါတွေကပြောင်းလဲရနိုင်ပါတယ်။

သင့်အနေနဲ့အခြား Laravel ရဲ့ components တွေကို configure လုပ်ချင်ပါလိမ့်မယ်။ ဉပမာအားဖြင့်

<div class="content-list" markdown="1">
- [Cache](/docs/{{version}}/cache#configuration)
- [Database](/docs/{{version}}/database#configuration)
- [Session](/docs/{{version}}/session#configuration)
</div>

Laravel ကို install လုပ်ပြီးသွားတဲ့အခါမှာ သင့်ရဲ့ [local environment](/docs/{{version}}/configuration#environment-configuration) ကိုရော configure လုပ်သင့်ပါတယ်
