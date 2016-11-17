# Redis

- [အစပျိုး](#introduction)
- [Configuration](#configuration)
- [Usage](#usage)
- [Pipelining](#pipelining)

<a name="introduction"></a>
## အစပျိုး

[Redis](http://redis.io) သည် open source advanced key-value store တစ်ခုဖြစ်သည်။  ၄င်းသည် keys များတွင် [strings](http://redis.io/topics/data-types#strings), [hashes](http://redis.io/topics/data-types#hashes), [lists](http://redis.io/topics/data-types#lists), [sets](http://redis.io/topics/data-types#sets), and [sorted sets](http://redis.io/topics/data-types#sorted-sets) ပါဝင်သောကြောင့်  ရံဖန်ရံခါ  data structure server ဟု သတ်မှတ်ခြင်း ခံရသည်။   

> **Note:** သင့်တွင် Redis PHP extension ကို PECL မှ တဆင့် သွင်းပြီးပါက Redis အတွက် အတိုကောက် အမည်ကို `app/config/app.php` ကြေညာပေးရမည်။

<a name="configuration"></a>
## Configuration

Application အတွက် Redis configuration မှာ **app/config/database.php**  အမည်ရှိ file ထဲတွင် တည်ရှိမည် ဖြစ်ပြီး ထို file ထဲတွင်  **redis** 
အမည်ရှိ array ကို application မှ အသုံးပြုမည် ဖြစ်သည်။


	'redis' => array(

		'cluster' => true,

		'default' => array('host' => '127.0.0.1', 'port' => 6379),

	),

default server configuration မှာ development အတွက် ဦးတည်ထားသော်လည်း မိမိတို ့စိတ်ကြိုက် ထို array ကိုပြောင်းလဲ သတ်မှတ်နိုင်သည်။ 
ထို Redis server ၏ name ၊ host နှင့် Server မှ အသုံးပြုသည့် port ကို ကြေညာပေးရန်လိုပေမည်။


 Laravel Redis client ကို `cluster` option မှ Redis nodes များ အကြား client-side sharding ပြုလုပ်ရန် ညွန်ကြားခြင်းဖြင့် Nodes များမှ data ဆွဲယူပြီး RAM အတွက် နေရာလွတ်များ ဖန်တီးနိုင်မည် ဖြစ်သည်။ သို ့သော် client-side sharding သည် failover ကို ကိုင်တွယ်နိုင်ခြင်း မရှိပေ။ ထိုကြောင့်
 Primary data store များ ရရှိနိုသည့် အခြေအနေတွင် cache data များ ထုတ်လွတ်ပေးသူ အဖြစ် အသုံးဝင်သည်။

Redis Server အနေဖြင့် စိစစ်ရန်လိုအပ်ပါက Redis Server Configuration array အတွင်း `password` key / value pair ကို ထည့်သွင်းနိုင်သည်။

<a name="usage"></a>
## အသုံးပြုပုံ


 `Redis::connection` method ကို ခေါ်ယူခြင်းဖြင့် Redis instance ကိုရယူနိုင်သည်။

	$redis = Redis::connection();

၄င်းသည် default Redis server ၏ instance ကို ပြန်ပေးမည် ဖြစ်သည်။ Server clustering ကို အသုံးပြုနေသည် မဟုတ်ပါက `connection` method 
တွင် မိမိတို ့ အသုံးပြုနေသည့် server ၏ အမည်ကို configuration တွင် ထည့်သွင်းပေးရန် လိုအပ်ပေမည်။

	$redis = Redis::connection('other');

Redis ၏ instance ကို ရရှိသည်နှင့် တပြိုင်နက် [Redis commands](http://redis.io/commands) ကို အသုံးပြုနိုင်ပြီ ဖြစ်သည်။ Laravel အနေဖြင့် magic methods ကို အသုံးပြုပြီး Redis server သို ့ command များကို ပို ့ဆောင်ပေးသည်။

	$redis->set('name', 'Taylor');

	$name = $redis->get('name');

	$values = $redis->lrange('names', 5, 10);


အထက်က ဖော်ပြထားသည့် အတိုင်း Magic method များမှ command များကို passing ပေးသွားသည်ကို တွေ ့ရမည် ဖြစ်သည်။ သို ့သော် သင့်အနေဖြင့် Magic method များကို မသုံးမဖြစ် သုံးရသည် မဟုတ်ပဲ ၊ အသုံးမပြုလိုပါက `command` method ကို အစားထိုး အသုံးပြုနိုင်သည်။

	$values = $redis->command('lrange', array(5, 10));

default connection  မှ ဆန် ့ကျင်ပြီး command များ အသုံးပြုလိုပါက `Redis` class မှ static magic method များကို အသုံးပြုနိုင်သည်။

	Redis::set('name', 'Taylor');

	$name = Redis::get('name');

	$values = Redis::lrange('names', 5, 10);

> **Note:** Laravel တွင် Redis [cache](cache) နှင့် [session](/docs/session.md) drivers များ ပါဝင်ပြီး ဖြစ်သည်။

<a name="pipelining"></a>
## Pipelining

Operation တစ်ခုအတွက် Command များစွာ ကို ပို ့လွတ်ရန် လိုအပ်ပါက Pipelining ကို အသုံးပြုရသည်။ ထို သို ့ ပြုလုပ်ရန် `pipeline`  ကို အသုံးပြုရမည်။

#### Server သို ့ Command များကို Piping ပြုလုပ်ခြင်း 

	Redis::pipeline(function($pipe)
	{
		for ($i = 0; $i < 1000; $i++)
		{
			$pipe->set("key:$i", $i);
		}
	});