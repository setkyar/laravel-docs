# Laravel Quickstart

- [Installation](#installation)
- [Routing](#routing)
- [Creating A View](#creating-a-view)
- [Creating A Migration](#creating-a-migration)
- [Eloquent ORM](#eloquent-orm)
- [Displaying Data](#displaying-data)

<a name="installation"></a>
## Installation

### Laravel Installer ကို အသုံးပြုခြင်း

ရှေးဦးစွာ [Laravel installer PHAR archive](http://laravel.com/laravel.phar) ကို ဒေါင်းပါ။  အဆင်ပြေစေရန်အတွက် ထို  file ကို `laravel` ဟု အမည်ပေးပြီး `/usr/local/bin` ထဲသို ့ပြောင်းရွေ  ့လိုက်ပါ။ ထိုနောက် `laravel new` command ဖြင့် သင်ထားရှိထားသော directory ပေါ်တွင် laravel installation အလိုအလျောက် ပြလုပ်သွားမည် ဖြစ်သည်။  ဥပမာ `laravel new blog` ဆိုသည့် command ကိုအသုံးပြပါက `blog` အမည်ရှိ folder တစ်ခုကို တည်ဆောက်ပေးပြီး လိုအပ်သည့် package များကိုပါ တခါတည်း ဒေါင်းလုပ်လုပ်ကာ စုစည်းပေးသွားမည် ဖြစ်သည်။ ၄င်းသို ့ install ပြုလုပ်ခြင်းသည် Composer မှ install လုပ်ခြင်းထက် ပို၍ လျင်မြန်ပါလိမ့်မည်။

### Composer ကို အသုံးပြုခြင်း

Laravel framework ကို [Composer](http://getcomposer.org) မှလည်း installation နှင့် လိုအပ်သည့် package များကို ထည့်သွင်းနိုင်သည်။ Composer မသွင်းရသေးပါက  [Composer ထည့်သွင်းခြင်းနည်းလမ်း](http://getcomposer.org/doc/00-intro.md) ကိုကြည့်၍ ထည့်သွင်းနိုင်ပါသည်။

ထိုနောက် သင့်အနေဖြင့် terminal မှ အောက်ပါ command ကို ရိုက်သွင်းခြင်းဖြင့် Laravel ကို install ပြုလုပ်နိုင်မည် ဖြစ်သည်။

	composer create-project laravel/laravel your-project-name --prefer-dist

၄င်း command မှ laravel အသစ်စက်စက် ကို သင့်`your-project-name` folder အတွင်းတွင် တည်ရှိနေမည်ကို တွေ ့ရပါမည်။

ထိုတင်မက သင့် အနေဖြင့် [Laravel repository from Github](https://github.com/laravel/laravel/archive/master.zip) မှ ဒေါင်းလော့ ပြုလုပ်ပြီး directory ထဲတွင် `composer install` run ၍လည်း install ပြုလုပ်နိုင်ပါသည်။ ထို command မှ framework တွင် လိုအပ်သော package များကို အလိုအလျောက် download ပြုလုပ်ပြီး install သွားမည် ဖြစ်သည်။

### Permissions

Laravel ကို install ပြုလုပ်ပြီးပါက သင့်အနေဖြင့် web server ၏ write permission ဖြင့်ပတ်သတ်၍ `app/storage` ထဲတွင် ပြင်ဆင်ရန် လိုအပ်ကောင်း လိုအပ်ပေမည်။ အသေးစိတ် အချက်အလက်ကို  [Installation](installation.md) တွင် ကြည့်ရှုနိုင်ပါသည်။

### Serving Laravel

အကြမ်းအားဖြင့် Apache သို ့မဟုတ် Nginx ပေါ်တွင် laravel application ကို တင်ထားနိုင်သည်။  သင့် အသုံးပြုသော PHP version မှာ 5.4 အထက်ဖြစ်ပြီး PHP တွင်ပါဝင်သော default server ကို အသုံးပြုလိုပါက သင့်အနေဖြင့် Artisan command ဖြစ်သည့် `serve` ကို အသုံးပြုနိုင်သည်။

	php artisan serve

<a name="directories"></a>
### Directory Structure

Framework ကို install ပြုလုပ်ပြီးနောက် သင့် အနေဖြင့် directory structure ဖြင့် ရင်းနှီးနေရန် လိုပေမည်။ `app` directory ထဲတွင် `views`, `controllers`, and `models` အစရှိသည့် folder များ တည်ရှိနေသည်ကို တွေ ့ ရမည် ဖြစ်သည်။ သင့် application ၏ code များကို ထိုထဲတွင် ရေးသားရမည် ဖြစ်သည်။ သင့်အနေဖြင့် လိုအပ်မည့် configuration နှင့် ပတ်သတ်၍ `app/config` အမည်ရှိ directory ထဲတွင်ကြည့်ရှုရမည် ဖြစ်သည်။

<a name="routing"></a>
## Routing

ရှေးဦးစွာ Route တစ်ခုကို တည်ဆောက်ကြပါစို ့။  Laravel တွင် အရိုးရှင်းဆုံး route မှာ route to Closure ဖြစ်သည်။ `app/routes.php` ကိုဖွင့်ပြီး အောက်ပါ code ကိုထည့်သွင်းကြည့်ပါ။ 

	Route::get('users', function()
	{
		return 'Users!';
	});

ထိုနောက် web browser ပေါ်တွင် `/users` ဟူသော route ဖြင့် စမ်းကြည့်ပါက သင့်အနေဖြင့် `Users!` တုံ ့ပြန်သည်ကို မြင်တွေ ့ရမည် ဖြစ်သည်။ 
ကောင်းလေစွ! သင့်အနေဖြင့် ပထမဦးစွာ route တစ်ခုကို ဖန်တီးလိုက်ပြီ ဖြစ်သည်။


Route များမှာ controller များနှင့်လည်း ချိတ်ဆက် အလုပ်လုပ်နိုင်သည်။ ဥပမာ

	Route::get('users', 'UserController@getIndex');

အဆိုပါ route တွင် 	`/users` ဟုခေါ်ယူလိုက်ပါက `UserController` class အတွင်းရှိ `getIndex` method  ကို အလုပ်လုပ်မည် ဖြစ်သည်။ Controller routing နှင့် ပတ်သတ်၍ အသေးစိတ်ကို [controller documentation](controllers.md) တွင်ကြည့်ရှုနိုင်သည်။

<a name="creating-a-view"></a>
## View တစ်ခု တည်ဆောက်ခြင်း

ထိုနောက် user data များကို ဖော်ပြရန် ရိုးရှင်းသည့် view တစ်ခုကို တည်ဆောက်ရန် လိုပေမည်။  view file များသည် `app/views` directory  ထဲတွင် တည်ရှိမည် ဖြစ်သည်။  View တွင် သင့် application တွင် ဖော်ပြလိုသည့်  HTML ဖြင့် ဖော်ပြသွားမည် ဖြစ်သည်။  `layout.blade.php` နှင့် `users.blade.php` ဟု၍ file နှစ်ခုကို တည်ဆောက်လိုက်ပါ။ `layout.blade.php` ဟုသည့် file တွင် အောက်ပါ အတိုင်း ရေးသားလိုက်ပါ။

	<html>
		<body>
			<h1>Laravel Quickstart</h1>

			@yield('content')
		</body>
	</html>

ထိုနောက် `users.blade.php` ဟုသော view တစ်ခုကို တည်ဆောက် လတ္တံ ့။ 

	@extends('layout')

	@section('content')
		Users!
	@stop

တချို  ့သော syntax များမှ သင့်အတွက် နည်းနည်း စိမ်းနေမည် ဖြစ်သည်။ အဘယ်ကြောင့်ဆိုသော် ယခု အသုံးပြုထားသည်မှာ Laravel ၏ templating system ဖြစ်သည့် Blade ကို အသုံးပြုထားခြင်း ကြောင့် ဖြစ်သည်။ Blade သည် အလွန်မြန်ဆန် လှပေသည်။ အကြောင်းမှာ ရိုးရှင်းလွယ်ကူ regular expression များကို အသုံးပြုကာ PHP အဖြစ်သို ့ compile ပြုလုပ်ထားခြင်းကြောင့်ဖြစ်သည်။ Blade အနေဖြင့် အလွန်တရာ စွမ်းအင်ကြီးမားလှသော template inheritance ကဲ့သို ့သော feature များကို support ပေးရုံသာမက  PHP တွင် ရေးသားနိုင်သည့် `if` နှင့် `for` သို ့သော Conditional statement များကိုပါ သေသပ်လှပစွာ ရေးသားနိုင်သောကြောင့်ဖြစ်သည်။ အသေးစိတ်ကို  [Blade documentation](templates.md) ကြည့်ရှုနိုင်ပေမည်။ 

ယခု ကျွန်တော်တို ့ views အပိုင်းကို ဖန်တီးပြီး ဖြစ်၍ `/users` ဟုသော route ဘက်ကို ပြန်လှည့်ကြပါစို ့။ Route မှ `Users!` ဟု return ပြန်ခြင်းထက် 
view ကို ပြန်ပေးဖို ့လိုပေမည်။ 

	Route::get('users', function()
	{
		return View::make('users');
	});

အံသြဖွယ်ကောင်းလေစွ။ သင့်အနေဖြင့် layout တစ်ခုကို extends ပြုလုပ်ထားသော view တစ်ခုကို တည်ဆောက်ပြီးပေသည်။ ဆက်၍ database layer တွင် ဆက်၍ လှုပ်ရှားကြပါစို ့။

<a name="creating-a-migration"></a>
## Migration တစ်ခုဖန်တီးခြင်း

Table တစ်ခုတည်ဆောက်ပြီး data တွေကို handle နိုင်ရန် Laravel migration system ကို အသုံးပြုရန်လိုပေမည်။ Migration အနေဖြင့် သင့် database ၏ modification ကို အလွယ်တကူ သတ်မှတ်နိုင်ပြီး သင့်အဖွဲ  ့သားများနှင့် မျှဝေနိုင်ပေမည်။

ရှေးဦးစွာ database နှင့် ချိတ်ဆက်ရန် လိုပေမည်။ database ဖြင့်ချိတ်ဆက်ရန် အတွက် `app/config/database.php` တွင် ပြင်ဆင်ရန်လိုပေမည်။ ပုံမှန်အားဖြင့် Laravel သည် MySQL ဖြင့် အသုံးပြုရန် သတ်မှတ်ထားသည်။ သင့်အနေဖြင့် လိုအပ်သော credential များကို config file တွင် ဖြည့်သွင်းရန်လိုပေမည်။ သင့်အနေဖြင့် အလိုရှိပါက စိတ်ကြိုက် `driver` option ကို `sqlite` ဖြစ်စေပြောင်းလဲနိုင်ပြီ။ ၄င်းအနေဖြင့် `app/database` directory အောက်တွင် တည်ရှိမည့် SQLite database ကို အလုပ်လုပ်မည် ဖြစ်သည်။

ထိုနောက် migration တစ်ခု ဖန်တီးရန် [Artisan CLI](artisan.md) ကို အသုံးပြုမည် ဖြစ်သည်။ project ၏ root တွင် အောက်ပါ အတိုင်း terminal မှ run ရန် လိုပေမည်။

	php artisan migrate:make create_users_table

ဆက်၍ `app/database/migrations` တည်ရှိသည့် migration file ကို ရှာရန် လိုပေမည်။ ထိုထဲတွင် `up` နှင့်`down`ဟူသော method နှစ်ခုပါဝင်မည် ဖြစ်သည်။


`up` method တွင် database တွင် ပြောင်းလဲချင်သည်များကို ထည့်သွင်းရေးသား၍  `down` method ပြောင်းပြန်ရေးသားရမည် ဖြစ်သည်။
အောက်ပါအတိုင်း migration ကို တည်ဆောက်လိုက်ပါ။

	public function up()
	{
		Schema::create('users', function($table)
		{
			$table->increments('id');
			$table->string('email')->unique();
			$table->string('name');
			$table->timestamps();
		});
	}

	public function down()
	{
		Schema::drop('users');
	}

ဆက်၍ migrate ပြုလုပ်လိုပါက terminal တွင်`migrate` ဟုရိုက်ရန်လိုပေမည်။ 

	php artisan migrate

migration တစ်ခုကို rollback (နောက်ပြန်လှည့်) လိုပါက သင့်အနေဖြင့် `migrate:rollback` ဟူ၍  ရိုက်ရုံသာ ဖြစ်သည်။ ယခု database table ရှိပြီ ဖြစ်၍ 
data လေးနည်းနည်းဖြင့် စလိုက်ကြပါစို ့။

<a name="eloquent-orm"></a>
## Eloquent ORM

Eloquent ORM သည် Laravel ၏ အလှတရား တစ်ရပ်ပင်ဖြစ်သည်။ သင့်အနေဖြင့် Ruby on Rails framework ကို အသုံးပြုဖူးပါက ၄င်းကဲ့သို ့ database interaction ပြုလုပ်ရာတွင် ActiveRecord ORM style သုံးထားသာ Eloquent နှင့်ရင်းနှီးနေမည် ဖြစ်သည်။  

ပထမဦးဆုံး model တစ်ခုကို သတ်မှတ်ကြပါစို ့။ Eloquent model တစ်ခုသည် ဆက်စပ်နေသော database table များ၏ query ကိုပါ အသုံးပြုနိုင်သည်။ သိပ်များ နားရှုပ်သွားသလား မသိ။ အခုလာမယ့် အပိုင်းမှာ တဖြည်းဖြည်း နားလည်လာမှာပါ။ Model တွေဟာ `app/models` ဆိုတဲ့ directory အတွင်းမှာ တည်ရှိပါတယ်။ အဆိုပါ directory ထဲမှာ အောက်ပါအတိုင်း `User.php` ဆိုတဲ့ model တစ်ခုကို တည်ဆောက်လိုက်ပါ။

	class User extends Eloquent {}

သတိပြုရမည်မှာ ကျွန်တော်တို ့အနေဖြင့် Eloquent ကို မည်သည့် table အသုံးပြုရန် မညွန်းဆိုရသေးချေ။ Eloquent တွင် အသုံးပြုနည်း များစွာ ရှိသည့် အနက်တစ်ခုမှာ Model အမည်၏ အများကိန်းမှာ database table အဖြစ် အလိုအလျောက် သိရှိနေမည် ဖြစ်သည်။ အဆင်ပြေလေစွ!

သင့်အနေဖြင့် ကြိုက်သည့် database administration tool ကို အသုံးပြုပြီး `users` table တွင် row အနည်းငယ် data သွင်းလိုက်ပါ။  ထိုနောက် Eloquent ကို အသုံးပြု၍  data များကို ထုတ်ယူပြီး view သို ့လွဲပြောင်းပေးလိုက်မည်။

ယခု `/users` route ကို အောက်ပါပုံစံပြောင်းလဲလိုက်ပါ။

	Route::get('users', function()
	{
		$users = User::all();

		return View::make('users')->with('users', $users);
	});

အထက်ပါ route ကိုကြည့်ပါ။ ရှေးဦးစွာ `User` model မှ `all` method မှာ `users` table မှ rows အားလုံးကို ထုတ်ပေးမည် ဖြစ်သည်။ ထိုနောက် ထို record များကို `with` method အသုံးပြု၍ view သို ့ passing ပေးလိုက်ခြင်း ဖြစ်သည်။ ထို `with` method  သည် key နှင့် value အနေဖြင့် data များကို လက်ခံမည် ဖြစ်သည်။ ထိုအခါ view သို ့ data များရောက်သွားမည် ဖြစ်သည်။

ကောင်းလေးစွ။ ယခု ကျွန်တော်တို ့ user ကို data များ ပြသနိုင်ရန် အဆင်သင့်ဖြစ်ချေပြီ။

<a name="displaying-data"></a>
## Data များ ပြသခြင်း

ယခုအခါ `users` ကို view တွင် မြင်သာစေရန် ပြုလုပ်ပြီးပြီဖြစ်သည်။ ကျွန်တော်တို ့ အောက်ပါ အတိုင်း ပြသနိုင်လေပြီ။

	@extends('layout')

	@section('content')
		@foreach($users as $user)
			<p>{{ $user->name }}</p>
		@endforeach
	@stop

သင့်အနေဖြင့် `echo` statements ကိုရှာနေလား မသိ။ Blade ကို အသုံးပြုရာတွင် data များကို တွန် ့ကွင်း နှစ်ခု အကြား ထည့်သွင်းခြင်းဖြင့် data များကို echo အစား ပြသပေးနိုင်သည်။ ဘယ်လောက်များ လွယ်ကူပေသလဲ။ ယခုအခါ သင့်အနေဖြင့် `/users` route ကို လှမ်းခေါ်လိုက်ခြင်းဖြင့် သင့် users များကို ပြသနိုင်လေပြီ။

အထက်ပါ ဥပမာဟာ အစသာရှိပါသေးသည်။ ထို tutorial တွင် သင့်အနေဖြင့် laravel ၏ အခြေခံကို တွေ ့မြင်နိုင်မည် ဖြစ်သည်။ သို ့သော်လည်း ပိုမို၍ စိတ်လှုပ်ရှားစရာ အချက်များစွာ စီတန်း၍ လေ့လာရန် ကျန်ရှိနေပါသေးသည်။ documentation ကို ဖတ်ရှုခြင်းဖြင့်  စွမ်းအားကြီးမားလှသည့်  [Eloquent](eloquent) နှင့် [Blade](/docs/templates) ကဲ့သို ့သော သို ့မဟုတ် သင့်ပိုစိတ်ဝင်စားနိုင်သည့်  [Queues](/docs/queues) နှင့် [Unit Testing](/docs/testing) ကဲ့သို ့သော အကြောင်းအရာများကို လေ့လာနိုင်သည်။ ထပ်၍ သင့် application ၏ architecture ကို သက်တောင့်သက်သာ ဖြစ်စေမည့်  [IoC Container](/docs/ioc.md) များလည်း ပါဝင်ပါသေးသည်။ ရွေးချယ်ပါလော့။
