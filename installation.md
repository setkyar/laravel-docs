# Laravel install လုပ်ခြင်း

- [Composer ကို Install လုပ်ခြင်း](#install-composer)
- [Laravel ကို Install လုပ်ခြင်း](#install-laravel)
- [Server လိုအပ်ချက်မျာ](#server-requirements)
- [Configuration လုပ်ခြင်း](#configuration)
- [URL လှလှလေးလိုချင်တယ်](#pretty-urls)

<a name="install-composer"></a>
## Composer ကို Install လုပ်ခြင်း

Laravel ရဲ့အသုံးဝင်တဲ့tool .... [Composer](http://getcomposer.org), သူ့ရဲ့ depenedencies တွေကို Manage လုပ်ဖို့။ ပထမဆုံး `composer.phar` copy ကိို download လုပ်လိုက်ပါ။ download လုပ်ပြီးသွားပြီဆိုရင် သင့်မှာ PHAR ဆိုတဲ့file လေးရှိသွားပါပြီ၊ အဲဒီ့ file ကိုသင့်ရဲ့local project မှာဒီတိုင်းထားချင်ရင်လည်းရပါတယ် တကယ်လို့သင်က `usr/local/bin` ထဲကိုရွှေ့ပြီးတော့သင့်ရဲ့  System အတွက် Global လုပ်မယ်ဆိုလည်းလုပ်နိုင်ပါတယ်။ Window မှာဆိုရင်တော့ [Windows installer](https://getcomposer.org/Composer-Setup.exe) ကိုသုံးပြီး install လုပ်နိုင်ပါတယ်။

<a name="install-laravel"></a>
## Laravel ကို Install လုပ်ခြင်း

### Laravel Installer မှတစ်ဆင့်

ပထမဆုံး[Laravel installer PHAR archive](http://laravel.com/laravel.phar) ကို  download လုပ်ပါ၊ install လုပ်ရာမှာလွယ်ကူအောင်လို့ file name ကို `laravel` လို့ပြောင်းလိုက်ပါ၊ ပြောင်းပြီးသွားရင်အဲ့ဒီ့ File ကို  `/usr/local/bin` ထဲကိုရွှေ့လိုက်ပါ။ Laravel ကို Install လုပ်မယ်ဆိုရင် `laravel new` ဆိုပြီး command line ကနေ run လိုက်ရင် Laravel Framework တစ်ခုကိုကိုယ်ကြိုက်တဲ့နေရာမှာ Install လုပ်နိုင်ပါပြီ။ `laravel new blog` ဆိုပြီး command line ကနေ run လိုက်ရင် blog ဆိုတဲ့အမည်နဲ့ command line ကနေကိုယ် create လုပ်ချင်တဲ့နေရာမှာ Laravel Framework အသစ်တစ်ခုကို install လုပ်ပေးမှာဖြစ်ပါတယ်။ ဒီနည်းက composer ကနေ download လုပ်တာထက်ပိုမြန်ပါတယ်။

သင့်အနေနဲ့ Laravel ကို Composer ကနေတစ်ဆင့် `create-project`  command သုံးပြီးတော့လည်း install လုပ်နိုင်ပါတယ်၊ terminal မှာ အောက်မှာရေးထားတဲ့ command ကို run ပြီးတော့လည်း install လုပ်နိုင်ပါတယ်

	composer create-project laravel/laravel --prefer-dist

### Download မှတစ်ဆင့်

Composer ကို install လုပ်ပြီးသွားပြီဆိုရင် Laravel Framework [latest version](https://github.com/laravel/laravel/archive/master.zip) ကို download လုပ်လိုက်ပါ၊  သင့်ရဲ့ web server ထဲမှာ  zip ကို extra လုပ်လိုက်ပါ၊ extra လုပ်ထားတဲ့  framework folder ထဲကို command line ကဝင်ပြီးတော့  `php composer.phar install` ဒါမှမဟုတ် (`composer install`) ဆိုပြီး run လိုက်ပါ။ ဒီ command က framework ရဲ့ dependencies တွေကို install လုပ်ခိုင်းလိုက်တာပါ။ ဒီ installation လုပ်တဲ့နေရာမှာ webserver မှာ git install လုပ်ထားမှ successfully complete ဖြစ်မှာပါ။

တကယ်လို့သင် Framework ကို update လုပ်ချင်တယ်ဆိုရင်`php composer.phar update` command ကို run ပေးရပါ့မယ်။

<a name="server-requirements"></a>
## Server လိုအပ်ချက်များ
Laravel Framework မှာ system requirements တစ်ချို့ရှိပါတယ်။ ဘာတွေလည်းဆိုရင်

- PHP >= 5.3.7
- MCrypt PHP Extension

တို့ဘဲဖြစ်ပါတယ်။

PHP 5.5 မှာ တစ်ချို့ OS တွေက PHP JSON extension ကို manullly install လုပ်ပေးရပါတယ်။ တကယ်လို့ Ubuntu သုံးတယ်ဆိုရင် `apt-get install php5-json` ဆိုပြီး terminal ကနေ run လိုက်တာနဲ့အဆင်ပြေပါတယ်။

<a name="configuration"></a>
## Configuration လုပ်ခြင်း

Laravel က configuration ဆိုတာမရှိသလောက်ပါဘဲ။ သင်စပြီး develop ဖို့ရာအဆင်သင့်ပါဘဲ။ဘယ်လိုဘဲပြောပြော သင့်အနေနဲ့ `app/config/app.php` file နဲ့သူ့ရဲ့ Documencation ကိုပြန်ကြည့်ချင်မှာပါဘဲ။ `app/config/app.php`မှာဘာတွေပါသလဲဆိုရင်တော့ `timezone` နောက် `locale`တို့ပါပါတယ်၊သင့်ရဲ့ application နဲ့အဆင်ပြေတာတွေကို configure လုပ်နိုင်ပါတယ်။

Laravel ကိုတစ်ခါ Install လုပ်တိုင်း [သင့်ရဲ့ local environmet](configuration#environment-configuration.md) ကို Configure ပြန်လုပ်သင့်ပါတယ်။ local machine မှာ     develop လုပ်တဲ့အခါ erros ကိုမြင်ရမယ်။ မူလကတော့ error reporting က သင့်ရဲ့ development production မှာ disable လုပ်ထားပါတယ်။

> **မှတ်ချက်:** `app.debug` ကို production မှာဘယ်တော့မှ true မပေးသင့်ပါဘူး။ဘယ်တော့မှ မလုပ်ပါနဲ့။

<a name="permissions"></a>
### Permissions များ

Laravel က   `app/storage` ကို web server အတွက် permission write ပေးရပါမယ်။

<a name="paths"></a>
### လမ်းကြောင်းများ

Framework ရဲ့ လမ်းကြောင်းတွေကပြောင်းလဲနိုင်ပါတယ်၊ ဒီ location တွေကိုပြောင်းချင်တယ်ဆိုရင် `bootstrap/paths.php` မှာကြည့်ရှူပြောင်းလည်းနိုင်ပါတယ်။

<a name="pretty-urls"></a>
## URL လှလှလေးလိုချင်တယ်

### Apache

Framework ထဲက `public/.htaccess` ကို URL မှာ `index.php` မပါအောင်ဖျောက်ထားပေးမှာဖြစ်ပါတယ်။ တကယ်လို့သင့် ရဲ့ Laravel application က Apache ကိုသုံးတယ်ဆိုရင်  `mod_rewrite` ကို enable လုပ်ဖို့မမေ့ပါနဲ့ဦး။

တကယ်လို့ `.htaccess`file က သင့် Application မှာအလုပ်မလုပ်ဘူးဆိုရင် အောက်ကတစ်ခုကိုစမ်းကြည့်လိုက်ပါ:

	Options +FollowSymLinks
	RewriteEngine On

	RewriteCond %{REQUEST_FILENAME} !-d
	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteRule ^ index.php [L]

### Nginx

Nginx မှာဆိုရင်အောက်ကညွှန်ကြားချက်ကို လိုက်လုပ်လိုက်တာနဲ့URL လှလှလေးတွေရပါတယ်

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
