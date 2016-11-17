# Cache

- [ပြင်ဆင်ခြင်း](#configuration)
- [Cache အသုံးပြုသည့်ပုံစံ](#cache-usage)
- [တန်ဖိုး ထပ်တိုးခြင်း နှင့် လျော့ချခြင်း](#increments-and-decrements)
- [Cache များအား အုပ်စုဖွဲ့ခြင်း](#cache-tags)
- [Database Cache](#database-cache)

<a name="configuration"></a>
## ပြင်ဆင်ခြင်း

Caching ပြုလုပ်နည်းပုံစံမျိုးစုံတွက် Laravel မှ API ထုတ်ပေးပြီးသားဖြစ်ပါတယ်။ Cache configuration အတွက် `app/config/cache.php` ဖိုင်ထဲမှာသွားပြင်ရမှာပါ။ Application တစ်ခုလုံးအတွက်အသုံးပြုမဲ့ cache driver ကို အဲ့ဒီဖိုင်ထဲမှာ သတ်မှတ်ပေးရမှာပါ။  [Memcached](http://memcached.org) နှင့် [Redis](http://redis.io) ကဲ့သိုသော လူသုံးများပြီး popular ဖြစ်တဲ့ caching methods တွေကို laravel မှာ အထောက်အပံ့ပေးထားပါတယ်။ 

အဲ့ဒီ cache configuration ဖိုင်ထဲမှာ ကျန်တဲ့ options တွေလဲ အများကြီးရှိပါသေးတယ်။ အဲ့ဒီအတွက်လဲ ဖိုင်ထဲမှာ တစ်ခါတည်း လမ်းညွှန်ချက်ရေးပေးထားပြီးသားပါ။ အကယ်၍ အဲ့ဒီ options တွေကိုအသုံးပြုမယ်ဆိုရင်တော့ option နဲ့ပတ်သက်တဲ့လမ်းညွှန်ချက်ကို သေသေချာချာဖတ်ပြီးမှ အသုံးပြုဖို့လိုအပ်ပါတယ်။ ပုံမှန်အတိုင်းဆိုရင်တော့ laravel ဟာ `file` cache driver အတွက် ပြင်ဆင်ပေးထားပါတယ်။ အဲ့ဒီ cache ဖိုင် objects တွေကို နံပါတ်စဉ်အတိုင်း filesystem ထဲမှာသွားသိမ်းထားပါတယ်။ Application အကြီးတွေအတွက်ဆိုရင်တော့ Memcached သို့မဟုတ် APC (Alternative PHP Cache) ကဲ့သို့သော in-memory cache တွေကိုအသုံးပြုသင့်ပါတယ်။ 

<a name="cache-usage"></a>
## Cache အသုံးပြုသည့်ပုံစံ

#### အချက်အလက်ကို Cache ထဲတွင်သိမ်းဆည်းခြင်း

	Cache::put('key', 'value', $minutes);

#### အချိန်ကန့်သတ်ဖို့အတွက် Carbon Objects အသုံးပြုခြင်း

	$expiresAt = Carbon::now()->addMinutes(10);

	Cache::put('key', 'value', $expiresAt);

#### အချက်အလက်သည် Cache ထဲတွင် ရှိမနေလျှင် ထပ်ထည့်ခြင်း

	Cache::add('key', 'value', $minutes);

အကယ်၍ အချက်အလက်ဟာ cache ထဲမှာ **ရှိနေလျှင်** `add` method ဟာ `true` return ပြန်မှာဖြစ်ပြီး၊ အဲ့လိုမဟုတ်ရင်တော့ `false` return ပြန်မှာဖြစ်ပါတယ်။

#### Cache ရှိမရှိ စစ်ဆေးခြင်း

	if (Cache::has('key'))
	{
		//
	}

#### Cache ထဲမှ အချက်အလက်ကို ရယူခြင်း

	$value = Cache::get('key');

#### အချက်အလက်ရယူခြင်း (သို့မဟုတ်) Default Value တစ်ခု return ပြန်ခြင်း

	$value = Cache::get('key', 'default');

	$value = Cache::get('key', function() { return 'default'; });

#### အချက်အလက်ကို Cache ထဲသို့ အကန့်မသတ်မရှိသိမ်းဆည်းခြင်း

	Cache::forever('key', 'value');

တစ်ခါတစ်ရံမှာ cache ထဲက အချက်အလက်ကိုလဲ ယူချင်တယ်၊ အကယ်၍ အဲ့ဒီအချက်အလက်ရှိမနေဘူးဆိုရင်လည်း cache ထဲကို default value တစ်ခု ထည့်ထားခဲ့ချင်တဲ့ အခြေအနေတွေရှိလာနိုင်ပါတယ်။ အဲ့ဒီလို အခြေအနေမျိုးအတွက် `Cache::remember` method ကိုအသုံးပြုနိုင်ပါတယ်။   

	$value = Cache::remember('users', $minutes, function()
	{
		return DB::table('users')->get();
	});

`remember` နဲ့ `forever` method နှစ်ခုလုံးကို ပေါင်းစပ်ပြီး အသုံးပြုနိုင်ပါသေးတယ်။

	$value = Cache::rememberForever('users', function()
	{
		return DB::table('users')->get();
	});

Cache ထဲမှာသိမ်းဆည်းလိုက်တဲ့ အချက်အလက်တွေဟာ နံပါတ်စဉ်အလိုက်သိမ်းဆည်းတာဖြစ်တဲ့အတွက် သင့်အနေနဲ့ ဘယ်လို အချက်အလက်အမျိုးအစားကိုမဆို လွတ်လပ်စွာ သိမ်းဆည်းနိုင်ကြောင်း သတိပြုပါလေ။

#### Cache ထဲရှိ အချက်အလက်ကို ဆွဲထုတ်ခြင်း

Cache ထဲမှ အချက်အလက်ကို ရယူအသုံးပြုပြီးတာနဲ့ ဖျက်ပြစ်လိုက်ချင်တယ်ဆိုရင်တော့၊ `pull` method ကိုအသုံးပြုနိုင်ပါတယ်။

	$value = Cache::pull('key');

#### Cache ထဲမှ အချက်အလက်ကို ပယ်ဖျက်ခြင်း

	Cache::forget('key');

<a name="increments-and-decrements"></a>
## တန်ဖိုး ထပ်တိုးခြင်း နှင့် လျော့ချခြင်း

`file` နဲ့ `database` driver မှလွဲ၍ ကျန်တဲ့ cache drivers တွေအားလုံးကို `increment` နဲ့ `decrement`လုပ်ဆောင်ချက်တွေအတွက် အထောက်အပံ့ပေးထားပါတယ်။

#### အချက်အလက်တန်ဖိုး ထပ်တိုးခြင်း

	Cache::increment('key');

	Cache::increment('key', $amount);

#### အချက်အလက်တန်ဖိုးလျော့ချခြင်း

	Cache::decrement('key');

	Cache::decrement('key', $amount);

<a name="cache-tags"></a>
## Cache များအား အုပ်စုဖွဲ့ခြင်း

> **သတိပြုရန်:** `file` သို့မဟုတ် `database` cache driver သုံးထားရင်တော့ Cache tags ကို အထောက်အပံ့ပေးမှာမဟုတ်ပါဘူး။ ၎င်းအပြင် cache ကို tags တွေနဲ့တွဲသုံးမယ်ဆိုရင် အဲ့ဒီ cache ကို အမြဲတမ်းသိမ်းဆည်းထားမှာဖြစ်တဲ့အတွက် `memcached` ကဲသို့သော driver ကိုအသုံးပြုမှသာ permormance အတွက်ပိုပြီးအဆင်ပြေစေမှာပါ။ အဲ့ဒီတော့မှ အသုံးမလိုတော့တဲ့ အချက်အလက်တွေကို အလိုအလျှောက် ပယ်ဖျက်ပေးမှာဖြစ်ပါတယ်။

#### Cache များအား အုပ်စုဖွဲ့ခြင်း

Cache ထဲမှာရှိတဲ့ ဆက်စပ်နေတဲ့ အချက်အလက်တွေကို အတူတကွအုပ်စုဖွဲ့ပေးခြင်းကို cache tags ကပြုလုပ်ပေးနိုင်ပါတယ်။ ပြီးရင်တော့ ပေးထားခဲ့တဲနာမည်အတိုင်းပြန်ပြီး လွယ်လွယ်ကူကူပဲ ပြန်လည်ပယ်ဖျက်နိုင်ပါတယ်။  Cache တွေကို တစ်ခုတစည်းထဲ အုပ်စုဖွဲ့ထားဖို့အတွက် `tags` method ကိုအသုံးပြုရပါမယ်။

Cache တွေကို တွဲစပ်ဖို့အတွက် `tags` method ထဲသို့ အမည်များကို `,` ခံ၍သော်လည်းကောင်း၊ array အနေနှင့် passing ပေး၍သော်လည်းကောင်း သိမ်းဆည်းနိုင်ပါတယ်။

	Cache::tags('people', 'authors')->put('John', $john, $minutes);

	Cache::tags(array('people', 'artists'))->put('Anne', $anne, $minutes);

Cache တွေကိုတစ်ခုတစည်းထဲ အုပ်စုဖွဲ့ထားဖို့အတွက် နှစ်သက်ရာ caching method ကိုအသုံးပြုနိုင်ပါတယ်။ `remember`, `forever` နှင့် `rememberForever` စတာတွေအပါအဝင်ပေါ့။ `increment` နဲ့ `decrement` method တွေကိုတော့ အသုံးပြုလို့ရမှာမဟုတ်ပါဘူး။

#### အုပ်စုဖွဲ့ထားသော Cache ထဲမှ အချက်အလက်ကို ရယူခြင်း

အုပ်စုဖွဲ့ထားသော cache ထဲမှ အချက်အလက်ကို ပြန်လည်ရယူဖို့အတွက် အုပ်စုဖွဲ့ခြင်းပြုလုပ်စဉ်က ပေးထားခဲ့သော အမည်များအတိုင်းအစဉ်လိုက်ပြန်လည် passing ပေးပြီး ရယူနိုင်ပါတယ်။ 

	$anne = Cache::tags('people', 'artists')->get('Anne');

	$john = Cache::tags(array('people', 'authors'))->get('John');

ပြန်လည်ပယ်ဖျက်ချင်တယ်ဆိုလျင်လဲ အုပ်စုဖွဲ့ခြင်းပြုလုပ်စဉ်ကပေးထားခဲ့သော နာမည်တစ်ခု သို့မဟုတ် တစ်ခုထက်ပိုသော အမည်များကို အသုံးပြုပြီးပယ်ဖျက်နိုင်ပါတယ်။ အောက်မှာပေးထားတဲ့ ဥပမာကို ကြည့်မယ်ဆိုရင် `people` အုပ်စုကော `author` အုပ်စုကိုကော ပယ်ဖျက်လိုက်တာဖြစ်ပါတယ်။ အဲ့ဒီအတွက် အဲ့ဒီအုပ်စုနှစ်ခုထဲမှာပါတဲ့ "Anne" နဲ့ "John" ကို cache ထဲကနေ ဖျက်သွားမှာဖြစ်ပါတယ်။ 

	Cache::tags('people', 'authors')->flush();

အောက်မှာပြထားတဲ့ ဥပမာအရဆိုရင် `authors` အုပ်စုကိုပဲပယ်ဖျက်လိုက်တာဖြစ်ပါတယ်။ အဲ့ဒါကြောင့် `authors` အုပ်စုထဲမှာပါတဲ့ "John" ကိုပဲဖျက်သွားမှာဖြစ်ပြီး "Anne" ကိုဖျက်သွားမှာမဟုတ်ပါဘူး။ အပေါ်ကဥပမာနဲ့ အောက်က ဥပမာကို ယှဉ်ကြည့်ပါ။ 

	Cache::tags('authors')->flush();

<a name="database-cache"></a>
## Database Cache

`database` cache driver ကိုအသုံးပြုမယ်ဆိုရင်တော့ cache အချက်အလက်တွေကိုသိမ်းဆည်းဖို့အတွက် table တစ်ခုပြုလုပ်ပေးဖို့ လိုပါတယ်။ အောက်မှာ `Schema` နဲ့ cache table ပြုလုပ်ထားပုံကို ဥပမာအနေနဲ့ပြပေးထားပါတယ်။ 

	Schema::create('cache', function($table)
	{
		$table->string('key')->unique();
		$table->text('value');
		$table->integer('expiration');
	});