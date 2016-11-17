# Laravel ရဲ့ Official Development Homestead အကြောင်း

- [Homestead မိတ်ဆက်](#introduction)
- [Homestead မှာပါဝင်သော Software များ](#included-software)
- [Installation & Setup](#installation-and-setup)
- [နေ့စဉ်အသုံးပြုမှူ](#general-usage)
- [Ports](#ports)

<a name="introduction"></a>
## Homestead မိတ်ဆက်

သင့်ရဲ့ PHP Development environment  ကို local development environment မှာပါ ကြည်နူးသာယာဖွယ်ကောင်းအောင်Laravel က အကောင်းဆုံးကြိုးစားအားထုတ်မှူတစ်ခုလုပ်ခဲ့ပါတယ်။ [Vagrant](http://vagrantup.com) ကသင့်ရဲ့ Virtual Machine တွေကို လွယ်လွယ်ကူကူ ထိန်းသိမ်း နိုင်အောင် သင့်ကိုထောက်ပံ့ ပေးထားပါတယ်။


Laravel Homestead က official ပါ၊ Vagrant  "box" မှာ ကြိုု ပြီး package လုပ်ထားတာပါ... နောက် အဲဒါကသင့်ကို development environment တစ်ခု တည်ဆောက်တဲ့နေရာမှာ PHP, a web server, နဲ့ အခြားအသုံးဝင်တဲ့  tools တွေကို သင့်ရဲ့ local machine မှာ install လုပ်စရာမလိုပါဘူး။ ဘယ် Opearting System ကိုသုံးတယ်ဆိုတာကိုလည်း worry များစရာမလိုတော့ပါဘူး။ Vagrant boxes တွေနဲ့ဘဲ အသုံးပြုလို့ရပါတယ်။ တကယ်လို့တစ်ခုခုမှားသွားတယ်ဆိုရင် vagrant boxes တွေကိုမိနစ်အနည်းငယ်အတွင်း destory လုပ်ပြီးတော့ ပြန်ပြီး create လုပ်နိုင်ပါတယ်။

Homestead က မည်သည့် Window, Mac, Linux မှာမဆို run ပါတယ်။ Homesead မှာ Nginx web server, PHP 5.5, MySQL, Postgres, Redis, Memcached နဲ့ အခြား Laravel application အတွက် အသုံးဝင်တာတွေပါဝင်ပါတယ်။

<a name="included-software"></a>
## Homestead မှာပါဝင်သော Software များ

- Ubuntu 14.04
- PHP 5.5
- Nginx
- MySQL
- Postgres
- Node (With Bower, Grunt, and Gulp)
- Redis
- Memcached
- Beanstalkd
- [Laravel Envoy](ssh#envoy-task-runner.md)
- Fabric + HipChat Extension

<a name="installation-and-setup"></a>
## Installation & Setup

### VirtualBox နဲ့ Vagrant Installing

သင်အနေနဲ့ Homestead environment ကိုမဖွင့်ခင် [VirtualBox](https://www.virtualbox.org/wiki/Downloads) နဲ့ [Vagrant](http://www.vagrantup.com/downloads.html) ကို install လုပ်ထားရပါ့မယ်။ ဒီ software နှစ်ခုပေါင်းပြီး popular operating systems များကိုလွယ်ကူစွာ virtual install လုပ်လို့ရပါမည်။ 

### Vagrant Box များထည့်ခြင်း

VirtualBox နဲ့ Vagrant ကို install လုပ်ပြီးပြီဆိုရင် ပထမဆုံး သင့်ရဲ့ Vagrant installation မှာ  `laravel/homestead` လို့ terminal ကနေ run ပြီး Laravel ရဲ့ Homestead ကို Virtual Box မှာ add လိုက်ပါ။ Laravel ရဲ့ Homestead box ကို download လုပ်ဖို့အတွက် သင့်အင်တာနက် conn  ပေါ်မူတည်ပြီး အချိကြာပါ့မယ်

	vagrant box add laravel/homestead

### Clone The Homestead Repository

သင်ရဲ့ Vagrant Installation မှာ box ထည့်ပြီးသွားပြီဆိုရင် သင့်အနေနဲ့ဒီ repository ကို download ဒါမှမဟုတ် clone လုပ်ပေးပါ။ နားလည်ထားရမှာက ဒီ repositiry က `Homestead` ပါ၊ ဒီ Folder ထဲမှာ သင့်ရဲ့ Laravel Projects တွေကို ထားရမှာပါ၊ Homestead box တွေကသင့်ရဲ့ Laravel (နဲ့ PHP Projects) တွေကို host အဖြစ် run မှာဖြစ်ပါတယ်။

	git clone https://github.com/laravel/homestead.git Homestead

### Set Your SSH Key

ပြီးရင်တော့သင် download လုပ်ထားတဲ့ repository ထဲမှာပါတဲ့ `Homestead.yaml` file ကို edit လုပ်သင့်ပါတယ်။ ဒီ file ထဲမှာဆိုရင် သင်ရဲ့ public SSH key တို့ နောက် သင့်ရဲ့ main machine နဲ့ Homestead virtual machine တို့ကို share တဲ့ Folder တို့ကို configure လုပ်နိုင်ပါတယ်။

သင့်မှာ SSH key မရှိဘူးလား၊ သင်က Linux ဒါမှမဟုတ် Mac မှာဆိုရင်  အောက်မှာဖော်ပြထားတဲ့ command ကို run လိုက်တာနဲ့  ssh key တစ်စုံကိုသင့်အတွက်ဖန်တီးပေးပါလိမ့်မယ်

	ssh-keygen -t rsa -C "your@email.com"

Windows မှာဆိုရင် သင်အနေနဲ့ [Git](http://git-scm.com/) ကို install လုပ်ပြီးတော့ `Git Bash`မှာ အထက်က command ကို run ပြီးတော့ အဆင်ပြေပါတယ်။ အဲလိုမှမဟုတ်ဘူးဆိုရင်လည်း[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) and [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). တို့ကိုအသုံးပြုနိုင်ပါတယ်။ 

သင် SSH Key ကို create လုပ်ပြီးပြီဆိုရင်  `Homestead.yaml` file ထဲက `authorize` ဆိုတဲ့ လမ်းကြောင်းထဲမှာ သင့်ရဲ့ SSH Key ရဲ့ path ကိုသတ်မှတ်လိုက်ပါ။

### Configure Your Shared Folders

သင့်ရဲ့ Homestead environment နဲ့ သင့်ရဲ့ local machine နှစ်ခုကြားမှာ Share တဲ့ Folder တွေအားလုံးက `Homestead.yaml` File ထဲမှာရှိမှာပါ။ တကယ်လို့ အဲ့ဒီ့ Files တွေ change သွားရင် သင့်ရဲ့ local machine နဲ့ Homestead environment ကို auto sync လုပ်ပေးသွားမှာပါ။ Share Folders တွေအများကြီးကိုလည်းသင်လိုအပ်ရင် configure လုပ်ရမှာပါ။

### Configure Your Nginx Sites

Nginx နဲ့သိပ်မရင်းနှီးဘူးမဟုတ်လား ပြသနာမရှိပါဘူး။ `sites` တွေကသင့်ရဲ့ Homestead environment က Folders တွေကို "domain" ဆီကိုလွယ်ကူစွာ map ပေးပါလိမ့်မယ်။ Site configuration တစ်ခုကို `Homestead.yaml` မှာတွေ့နိုင်ပါတယ်။ သင့်အနေနဲ့ sites အများကြီးကိုသင့်ရဲ့ Homestead မှာထည့်ချင်ပါလိမ့်မယ်၊ Homestad က သင့်virtualized Laravel Projects တွေရဲ့ environment တွေကို အဆင်ပြေစေပါလိမ့်မယ်။

### Bash Aliases

To add Bash aliases to your Homestead box, simply add to the `aliases` file in the root of the Homestead directory.

### VagrantBox ကိုစတင်ခြင်း

`Homestead.yaml` file မှာသင့်ရဲ့ link တွေကို edit လုပ်ပြီးပြီဆိုရင် သင့်ရဲ့ `Homestead` directory ထဲမှာ `vagrant up` ဆိုပြီး terminal ကနေ run လိုက်ပါ။ Vagrant က Virtual Machine ကို boot လုပ်ပါ့လိမ့်မယ် ပြီးရင်တော့ သင့်ရဲ့ share folders နဲ့ Nginx sites တွေကို auto configure လုပ်သွားပါလိမ့်မယ်။

သင့်ရဲ့ Nginx sites တွေအတွက် "domain" တွေကို သင်ရဲ့ local machine က hosts မှာထက်ပေါင်းထည့်ဖို့မမေ့ပါနဲ့ဦး။ hosts file ကသင့် local machine က requests တွေကို Homestead ဆီကို redirect လုပ်ပေးပါလိမ့်မယ်။ Mac နဲ့ linux မှာ ဆိုရင် hosts file က `/etc/hosts` ထဲမှာပြင်လို့ရပါတယ်။ Window မှာဆိုရင်တော့ `C:\Windows\System32\drivers\etc\hosts` မှာရှိပါတယ်။ သင်ထက်ပေါင်းထည့်ရမယ့် line က အောက်ကလိုဖြစ်ပါလိမ့်မယ်၊

	127.0.0.1  homestead.app

သင့်ရဲ့ domain ကိုသင့်ရဲ့ `hosts` file ထဲကိုပေါင်းထည့်ပြီးပြီဆိုရင် သင်ရဲ့ browser ကနေသင့် domain နောက်က port နံပါတ်နဲ့ဆိုရင်သင့်ရဲ့ဆိုက်ကို access လုပ်လို့ရပါပြီ။

	http://homestead.app:8000

သင်ရဲ့ database တွေကိုဘယ်လို connect လုပ်မလဲဆိုတာကို လေ့လာဖို့ ဆက်ဖတ်ပါဦ။

<a name="daily-usage"></a>
## နေ့စဉ်အသုံးပြုမှူ

### SSH ကို connect လုပ်ခြင်း

သင့်ရဲ့ Homestead environment ကို SSH ကနေ ချိတ်ဆက်ဝင်ဖို့ သင့်အနေနဲ့ `127.0.0.1` port ကတော့ 2222 ဖြစ်ပြီး SSH key ကတော့ သင်ရဲ့`Homestead.yaml`မှာ သင်သတ်မှတ်ခဲ့တဲ့ key ဘဲဖြစ်ပါတယ်။ `vagrant ssh` ဆိုပြီးသင့်ရဲ့ Homestead Folder ကနေလည်း ဝင်လို့ရပါတယ်။

သင်အနေနဲ့ ဒါ့ထက်အဆင်ပြေမှူ လိုချင်သေးတယ်ဆိုရင်တော့ အောက်မှာဖော်ပြထားတဲ့ alias ကို သင့်ရဲ့ `~/.bash_aliases` ဒါမှမဟုတ် `~/.bash_profile` မှာပေါင်းထည့်လိုက်တာက ပိုပြီးအသုံးဝင်ပါမယ်၊ 

	alias vm='ssh vagrant@127.0.0.1 -p 2222'

### သင့်ရဲ့ Databases များကို connect လုပ်ခြင်း

homestead` ရဲ့ databases တွေဖြစ်တဲ့ MySQL နဲ့ Postgres နှစ်ခုလုံးကို box တွေရဲ့အပြင်မှာ configuration လုပ်ထားပါတယ်။ ဒါထက်ပိုပြီးအဆင်ပြေဖို့ Laravel ရဲ့ `local` database ကို default configure လုပ်ထားပါတယ်။

သင့်ရဲ့ database MySQL ဒါမှမဟုတ် Postgres ကို Navicat (သို့) Sequel Pro ကနေသင့်ရဲ့ main machine နဲ့ connect လုပ်ချင်တယ်ဆိုရင် သင့်အနေနဲ့  MySQL အတွက် `127.0.0.1` နဲ့ port 33060 နဲ့Postgres အတွက် port 54320 ဖြစ်ပါတယ်။ Database နှစ်ခုလုံးအတွက် username နဲ့ password က  `homestead`/ `secreat` ဖြစ်ပါတယ်။

> **Note:** You should only use these non-standard ports when connecting to the databases from your main machine. You will use the default 3306 and 5432 ports in your Laravel database configuration file since Laravel is running _within_ the Virtual Machine.

### နောက်ထက်ဆိုက်တစ်ခု ထပ်ထည့်ခြင်း

သင့်ရဲ့ Homestead environment ကသင်ထည့်ချင်တာတွေထည့်ပြီးသွားပြီ run လည်း run နေပြီဆိုရင် သင့်အနေနဲ့ Laravel applications တွေကို သင့်ရဲ့ Nginx sites မှာထပ်ထည့်ချင်မှာပေါ့။ Homestead environment တစ်ခုမှာ သင်ကြိုက်သလောက် Laravel installation လုပ်နိုင်ပါတယ်။ Laravel application ထက်ပေါင်းထည့် တဲ့နေရာမှာ နည်းနှစ်ခုရှိပါတယ်။ ပထမတစ်ခုကသင့်ရဲ့ `Homestead.yaml` files မှာထက်ပေါင်းထည့်ပါ  ပြီးရင် `vagrant destory` နဲ့ box တွေကို ဖျက်ပါ၊ ပြီးရင် `vagrant up` ပြန်လုပ်ပါ။

နောက်ထက်နည်းတစ်ခုကတော့ သင့်ရဲ့ Homestead environment မှာ `serve` script ကိုသုံးပြီး Laravel application တွေကိုထက်ထည့်နိုင်ပါတယ်။ `serve` script ကိုအသုံးပြုချင်တယ်ဆိုရင်တော့ သင့်ရဲ့ Homestead environment ထဲကိုဝင်ပြီးတော့ အောက်က command ကို run လိုက်ပါ

	serve domain.app /home/vagrant/Code/path/to/public/directory

> **မှတ်ချက်:** `serve` command ကို run ပြီးပြီဆိုရင်  `hosts` file ထဲမှာ သင်ထပ်ပေါင်းထည့်လိုက်တဲ့ နောက်ထက် site ကို သင့်ရဲ့ စက်မှာ ထက်ပေါင်းထည့်ဖို့ မမေ့ပါနဲ့။

<a name="ports"></a>
## Ports

အောက်မှာဖော်ပြထားတဲ့ ports တွေက  သင့် Homestead ရဲ့ ports တွေဖြစ်ပါတယ်

- **SSH:** 2222 -> Forwards To 22
- **HTTP:** 8000 -> Forwards To 80
- **MySQL:** 33060 -> Forwards To 3306
- **Postgres:** 54320 -> Forwards To 5432
