# ဆက်ရှင်

- [ပြင်ဆင်ခြင်း](#configuration)
- [Session Usage](#session-usage)
- [Flash Data](#flash-data)
- [Database Sessions](#database-sessions)
- [Session Drivers](#session-drivers)

<a name="configuration"></a>
## ပြင်ဆင်ခြင်း

HTTP မှာ Stateless protocol ဖြစ်သောကြောင့် request တစ်ခုနှင့်တစ်ခု ကြားထဲတွင် Session ထဲတွင် အချက်အလက်များကို သိမ်းဆည်းကာ ပို ့ဆောင်ရပေသည်။ Laravel တွင် session ကို နည်းလမ်းမျိုးစုံဖြင့် အသုံးပြုနိုင်ရန် API တစ်ခုကို ဖန်တီးကာ စုစည်းထားသည်။ အခြားသော ကျော်ကြားသည့်  
[Memcached](http://memcached.org) နှင့် [Redis](http://redis.io), Session အဖြစ်အသုံးပြုနိုင်သည့် နည်းလမ်းများကို ပံ့ပိုးထားသည်။

Session နှင့်ပတ်သတ်သည့် အချက်အလက်များကို `app/config/session.php` တွင် လိုအပ်သလို ပြောင်းလဲ ရမည် ဖြစ်သည်။ ပုံမှန်အားဖြင့် application အတော်များများတွင် အဆင်ပြေမည့် `file` session driver ကို အသုံးပြုထားသည်။

#### Reserved Keys (သီးသန့် key)


`flash` ဆက်ရှင်ကီးကို Laravel Farmework အတွင်းပိုင်းတွင်သုံးထားပါသည်၊ ထို့ကြောင့်  သင့်အနေနဲ့အဲ့ဒီ့ `flash` ဆိုတဲ့အမည်နဲ့  session ထဲကို item တစ်ခုမှ မထည့်သင့်ပါ။

<a name="session-usage"></a>
## Session Usage

#### Storing An Item In The Session

	Session::put('key', 'value');

#### Push A Value Onto An Array Session Value

	Session::push('user.teams', 'developers');

#### Retrieving An Item From The Session

	$value = Session::get('key');

#### Retrieving An Item Or Returning A Default Value

	$value = Session::get('key', 'default');

	$value = Session::get('key', function() { return 'default'; });

#### Session မှ value တစ်ခု ထုတ်ယူကာ ဖယ်ထုတ်ခြင်း

	$value = Session::pull('key', 'default');

#### Session မှ value များအားလုံး ခေါ်ယူခြင်း

	$data = Session::all();

#### Session မှ item ရှိမရှိ စစ်ဆေးခြင်း

	if (Session::has('users'))
	{
		//
	}

#### Session မှ item တစ်ခုကို ထုတ်ပယ်ခြင်း

	Session::forget('key');

#### Session တစ်ခုလုံး ရှင်းပစ်ခြင်း

	Session::flush();

#### Session ID အသစ်ထုတ်ယူခြင်း

	Session::regenerate();

<a name="flash-data"></a>
## Flash Data

တခါတရံ  တစ်ချို  ့သော data များကို နောက်ထပ် request တစ်ခါစာသာ သိမ်းဆည်းလိုပေမည်။ ထိုသို ့ပြုလုပ်နိုင်ရန် `Session::flash` method ကို အသုံးပြုနိုင်သည်။

	Session::flash('key', 'value');

#### နောက်ထပ် request တစ်ခုစာ သက်တမ်းတိုးခြင်း

	Session::reflash();

#### နောက်ထပ် request တစ်ခုစာ သက်တမ်းတိုးခြင်း  (ရွေးချယ်ထားသော data များသာ) 

	Session::keep(array('username', 'email'));

<a name="database-sessions"></a>
## Database Sessions


`database` session driver ကို အသုံးပြုပါက Session item များကို သိမ်းဆည်းရန် table တစ်ခု တည်ဆောက်ရန်လိုပေမည်။ အောက်တွင်  table အတွက် `Schema` တည်ဆောက်ပုံကို ဖော်ပြထားပါသည်။

	Schema::create('sessions', function($table)
	{
		$table->string('id')->unique();
		$table->text('payload');
		$table->integer('last_activity');
	});

	
Table ကို အသုံးပြုထားသောကြောင့် `session:table` ဟူသည့် Artisan command ကို အသုံးပြုပြီး migration ပြုလုပ်နိုင်သည်။


	php artisan session:table

	composer dump-autoload

	php artisan migrate

<a name="session-drivers"></a>
## Session Drivers

session "driver" မှ session data များ မည်သည့်နေရာတွင်း သိမ်းဆည်းမည်ကို သတ်မှတ်ထားသည်။  Laravel အနေဖြင့် အတော်လေးကောင်းမွန်သော driver အမျိုးအစားများကို ပံပိုးထားသည်။

- `file` - sessions သည် `app/storage/sessions` တွင် သိမ်းဆည်းထားမည်။
- `cookie` - sessions သည် encrypted cookies အနေဖြင့် သိမ်းဆည်းထားမည် ဖြစ်သည်။
- `database` session သည့် application ၏ database ထဲတွင် သိမ်းဆည်းထားမည် ဖြစ်သည်။
- `memcached` / `redis` တို ့သည် မြန်ဆန်သွက်လက်သည့် cache based session engine များဖြစ်ကြသည်။
- `array` - sessions သည် PHP array အဖြစ် သိမ်းဆည်းမည်ဖြစ်ပြီး နောက်ထပ် request များအတွက် သိမ်းဆည်းထားနိုင်မည် မဟုတ်ပေ။


> **မှတ်ချက်:**  array driver သည် [unit tests](testing.md) အတွက် အသုံးပြုခြင်း ဖြစ်ပြီး တကယ့် session data အတွက် အသုံးပြုခြင်း မဟုတ်ပေ။

