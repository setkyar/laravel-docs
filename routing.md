# Route လုပ်ခြင်း

- [လမ်းကြောင်းပေးခြင်း အခြေခံ](#basic-routing)
- [လမ်းကြောင်းထိန်းကွပ် ကိန်းများ](#route-parameters)
- [Route Filterများ](#route-filters)
- [အမည်ရှိ လမ်းကြောင်းများ](#named-routes)
- [လမ်းကြောင်းအုပ်စုများ](#route-groups)
- [Sub-Domain များ အသုံးပြု ၍ လမ်းကြောင်းပေးခြင်း](#sub-domain-routing)
- [လမ်းကြောင်းရှေ့ ဆွယ်ပေးခြင်း](#route-prefixing)
- [လမ်းကြောင်း နှင့် Model ချိတ်တွယ်ခြင်း](#route-model-binding)
- [404 error များ ထုတ်လွှတ်ခြင်း](#throwing-404-errors)
- [Controller များအား လမ်းကြောင်းပေးခြင်း](#routing-to-controllers)

<a name="basic-routing"></a>

###လမ်းကြောင်းပေးခြင်း(Routing) အခြေခံ

သင့် application ၏ လမ်းကြောင်း အများစု ကို `app/routes.php` ဖိုင် တွင် သတ်မှတ်ရပါမည်။ `Laravel` တွင် အရိုးရှင်းဆုံး လမ်းကြောင်းတစ်ခုသည် `URI` တစ်ခု နှင့် `closure` ပြန်ခေါ်ချိတ် method (callback method) တစ်ခု ပါ ၀င် ပါသည်။
#### အခြေခံ GET လမ်းကြောင်း

	Route::get('/', function()
	{
		return 'Hello World';
	});

#### အခြေခံ POST လမ်းကြောင်း

	Route::post('foo/bar', function()
	{
		return 'Hello World';
	});

#### လမ်းကြောင်းတစ်ခုအား HTTP ကြိယာ အများ ဖြင့် မှတ်ပုံတင်ခြင်း

	Route::match(array('GET', 'POST'), '/', function()
	{
		return 'Hello World';
	});

#### လမ်းကြောင်းတစ်ခုအား မည်သည့် HTTP ကြိယာဖြင့် ဖြစ်စေ သက်ဆိုင်စေရန် မှတ်ပုံတင်ခြင်း

	Route::any('foo', function()
	{
		return 'Hello World';
	});

#### လမ်းကြောင်းတစ်ခုအား HTTPS ဖြင့် မဖြစ်မနေ အသုံးပြ ုစေချင်း

	Route::get('foo', array('https', function()
	{
		return 'Must be over HTTPS';
	}));

မကြာခဏ သင့် လမ်းကြောင်းများအတွက် `URL` များ ထုတ်ရန် လိုအပ်ပါလိမ့်မည်။ ထို့အတွက် `URL::to` method ဖြင့် အသုံးပြုနိုင်ပါသည်။

	$url = URL::to('foo');

<a name="route-parameters"></a>
## လမ်းကြောင်းထိန်းကွပ် ကိန်းရှင်များ

	Route::get('user/{id}', function($id)
	{
		return 'User '.$id;
	});

#### မထည့်လည်းရသော လမ်းကြောင်းထိန်းကွပ်ကိန်းရှင်များ

	Route::get('user/{name?}', function($name = null)
	{
		return $name;
	});

#### ပေးထားသော မူလတန်ဖိုးများဖြင့် လမ်းကြောင်းထိန်းကွပ်ကိန်းရှင်များ

	Route::get('user/{name?}', function($name = 'John')
	{
		return $name;
	});

#### Regular Expression များဖြင့် လမ်းကြောင်းထိန်းကွပ်ကိန်းများအား ကန့်သတ်ခြင်း

	Route::get('user/{name}', function($name)
	{
		//
	})
	->where('name', '[A-Za-z]+');

	Route::get('user/{id}', function($id)
	{
		//
	})
	->where('id', '[0-9]+');

#### Where အကန့်အသတ်များအား Array အဖြစ်ဖြင့် ပေးပို့ခြင်း

အကယ်၍ လို အပ်ပါက ကန့်သတ်ချက်များအား `Array` အဖြစ်တွဲ၍လည်း သုံးနိုင်ပါသည်။

	Route::get('user/{id}/{name}', function($id, $name)
	{
		//
	})
	->where(array('id' => '[0-9]+', 'name' => '[a-z]+'))

#### Global Pattern များ သတ်မှတ်ခြင်း

အကယ်၍ လမ်းကြောင်းထိန်းကွပ်တစ်မျိ  ုးအား ပေးထားသော regular expression တစ်ခုဖြင့် ကန့်သတ်လိုပါက `pattern` method ကို အသုံးပြ ုနိုင်ပါသည်။

	Route::pattern('id', '[0-9]+');

	Route::get('user/{id}', function($id)
	{
		// Only called if {id} is numeric.
	});

#### လမ်းကြောင်းထိန်းကွပ်ကိန်းတစ်ခု၏ တန်ဖိုး ကို အသုံးပြ ုခြင်း

အကယ်၍ လမ်းကြောင်းထိန်းကွပ် ကိန်းတစ်ခု ၏ တန်ဖိုးအား လမ်းကြောင်း၏ အပြင်ဘက်တွင် အသုံးပြု လိုပါက `Route::input` method ကို အသုံးပြု နိုင်ပါသည်။

	Route::filter('foo', function()
	{
		if (Route::input('id') == 1)
		{
			//
		}
	});

<a name="route-filters"></a>
## Route filter များ

route filter များ သည်  ပေးထားသော လမ်းကြောင်းတစ်ခုကို အသုံးပြ ုနိုင်စွမ်း ကန့်သတ်ရာ၌ လွယ်ကူသက်သာအောင် ဖန်တီးပေးထားသော နည်းလမ်းတစ်မျိ  ုးဖြစ်ပါသည်။ ၎င်းတို့ သည် သင့် site တွင်  အသိအမှတ်ပြု  စစ်ဆေးချက်များ (Authentications) လို အပ်ပါက အသုံးဝင်နိုင်ပါသည်။ Laravel framework အတွင်း၌ပင် `auth filter`, `auth.basic filter`, `guest filter`, `csrffilter` အစရှိသဖြင့် များစွာသော route filter များ ပါ၀င်ပါသည်။၎င်းတို့ အားလုံး သည် `app/filters.php` ဖိုင်တွင် တည်ရှိပါသည်။

#### Route filter တစ်ခု သတ်မှတ်ခြင်း

	Route::filter('old', function()
	{
		if (Input::get('age') < 200)
		{
			return Redirect::to('home');
		}
	});

အကယ်၍ ပေးထားသော Web Server ၏ တုန့်ပြန်ချက် သည် route filter တစ်ခုဆီမှ ပြန်လာခြင်းဖြစ်ပါက ထို တုန့်ပြန်ချက်အား မူလတောင်းဆိုချက်၏ တုန့်ပြန်ချက်အဖြစ် စဉ်းစားမည်ဖြစ်ပြီး လမ်းကြောင်းကို execute လုပ်မည် မဟုတ်ပါ။ထို့ပြင် သတ်မှတ်ထားပြီးသော နောက်ဆွယ် route filters(after filters)ကို လည်း ပျက်ပြယ်စေမည် ဖြစ်ပါသည်။

#### လမး်ကြောင်းတစ်ခုပေါ်သို့ Route filter တစ်ခု ချိတ်ဆက်ခြင်း

	Route::get('user', array('before' => 'old', function()
	{
		return 'You are over 200 years old!';
	}));

####  Controller Action တစ်ခု သို့ Route filter တစ်ခု ချိတ်ဆက်ခြင်း

	Route::get('user', array('before' => 'old', 'uses' => 'UserController@showProfile'));

#### လမ်းကြောင်းတစ်ခုပေါ်သို့ Route filter အများ ချိတ်ဆက်ခြင်း

	Route::get('user', array('before' => 'auth|old', function()
	{
		return 'You are authenticated and over 200 years old!';
	}));

#### လမ်းကြောင်းတစ်ခုပေါ်သို့ Route filter အများ အား Array အဖြစ်ဖြင့် ချိတ်ဆက်ခြင်း

	Route::get('user', array('before' => array('auth', 'old'), function()
	{
		return 'You are authenticated and over 200 years old!';
	}));

#### Route filter ထိန်းကွပ်ကိန်းများ သတ်မှတ်ခြင်း

	Route::filter('age', function($route, $request, $value)
	{
		//
	});

	Route::get('user', array('before' => 'age:200', function()
	{
		return 'Hello World';
	}));

နောက်ဆွယ် Route filter များ သည် `$response` အား တတိယမြောက် argument အဖြစ် လက်ခံရရှိပါသည်။

	Route::filter('log', function($route, $request, $response)
	{
		//
	});

#### Pattern အခြေခံ Filter များ

Route filter တစ်ခုအား လမ်းကြောင်းတို့၏ URI ပေါ် အခြေခံ ၍ သတ်မှတ်ထားသော လမ်းကြောင်း အုပ်စုတစ်ခု လုံး ပေါ်သို့ သက်ရောက်စေရန်လည်း သတ်မှတ်နိုင်ပါသည်။ 

	Route::filter('admin', function()
	{
		//
	});

	Route::when('admin/*', 'admin');

ပေးထားသော ဥပမာတွင် `admin` route filter သည် `admin/` ဖြင့် စသော လမ်းကြောင်းအားလုံး ပေါ်သို့ သက်ရောက်မည် ဖြစ်ပါသည်။ ခရေပွင့် စာလုံး `*` ကို မည်သည့် စာလုံးနှင့်မဆို ကိုက်ညီစေမည့် သံခိတ် စာလုံး အဖြစ် အသုံးပြု နိုင်ပါသည်။

ထို့ အပြင် HTTP ကြိယာများဖြင့်လည်း pattern အခြေခံ filter များ အား ကန့်သတ်နိုင်ပါသည်။
You may also constrain pattern filters by HTTP verbs:

	Route::when('admin/*', 'admin', array('post'));

#### Filter class များ

အဆင့်မြင့် route filter များ တွင် Closure တစ်ခု ထက် class တစ်ခုကို အသုံးပြု ချင် ကောင်း အသုံးပြု ပါလိမ့်မည်။စင်စစ် filter class များသည် application [IoC Container](ioc.md) မှ တစ်ဆင့် ပြန်ဖြည်ချင်းဖြစ်ရာ dependency injection ကို အသုံး ပြု နိုင်စေ၍ test လုပ်ခြင်းကို အထောက်အပံ့ကောင်းကောင်းပေးနိုင်ပါသည်။

#### Class အခြေခံ filter တစ်ခု အား မှတ်ပုံတင်ခြင်း

	Route::filter('foo', 'FooFilter');

ပုံမှန်အားဖြင့် `FooFilter` class ၏ `filter` method ကို ခေါ်ပါလိမ့်မည်။

	class FooFilter {

		public function filter()
		{
			// Filter logic...
		}

	}

အကယ်၍ `filter` method ကို မသုံးလိုပါက အခြား method တစ်ခုကို သတ်မှတ်လိုက်ရုံပင်။

	Route::filter('foo', 'FooFilter@foo');

<a name="named-routes"></a>
## အမည်ရှိ လမ်းကြောင်းများ

အမည်ရှိလမ်းကြောင်းများသည် လမ်းကြောင်းလွှဲများ ပြု လုပ်သောအခါ သို့မဟုတ် URL များ ရေးသားသောအခါ လမ်းကြောင်းများကို ညွှန်းဆိုရာ ၌ ပိုမိုလွယ်ကူစေပါသည်။

	Route::get('user/profile', array('as' => 'profile', function()
	{
		//
	}));

Controller action အတွဲများ အတွက် လည်း လမ်းကြောင်းအမည်များ သတ်မှတ်နိုင်ပါသည်။

	Route::get('user/profile', array('as' => 'profile', 'uses' => 'UserController@showProfile'));

အထက်ပါအတိုင်းသတ်မှတ်ပြီးပါက ပေးထားသော လမ်းကြောင်းနာမည်ဖြင့် URL များ ထုတ်ရာ၌ ဖြစ်စေ လမ်းကြောင်းလွှဲများ အသုံးပြု ရာ ၌ ဖြစ်စေ သုံးနိုင်ပါပြီ။

	$url = URL::route('profile');

	$redirect = Redirect::route('profile');

လက်ရှိ ရောက်ရှိနေသော လမ်းကြောင်း၏ အမည်ကို `currentRouteName` method ဖြင့် သိရှိအသုံးပြု နိုင်ပါသည်။

	$name = Route::currentRouteName();

<a name="route-groups"></a>
## လမ်းကြောင်း အုပ်စုများ

တစ်ခါတစ်ရံ  လမ်းကြောင်း အုပ်စု တစ်ခု ပေါ်သို့ filter များ သက်ရောက်ဖို့ လိုအပ်ကောင်းလိုအပ်နိုင်ပါသည်။ ထိုအခါမျိ  ုးတွင် လမ်းကြောင်းတစ်ခုစီအတွက် filter များသတ်မှတ်မည့်အစား လမ်းကြောင်းအုပ်စု တစ်ခုကို အသုံးပြု နိုင်ပါသည်။

	Route::group(array('before' => 'auth'), function()
	{
		Route::get('/', function()
		{
			// Has Auth Filter
		});

		Route::get('user/profile', function()
		{
			// Has Auth Filter
		});
	});

`group` array အတွင်းတွင်`namespace` ထိန်းကွပ်ကိန်းထည့်၍ လည်း ပေးထားသော အုပ်စုအတွင်းရှိ controller များအား namespace တစ်ခုအတွင်း ကျရောက်နေစေရန် စီမံနိုင်ပါသည်။

	Route::group(array('namespace' => 'Admin'), function()
	{
		//
	});

<a name="sub-domain-routing"></a>
## Sub-Domain များ အသုံးပြု ၍ လမ်းကြောင်းပေးခြင်း

Laravel လမ်းကြောင်းများတွင် သံခိတ်သုံး sub-domain များကို ကောင်းမွန်စွာ စီမံအသုံးချနိုင်ပြီး domain မှ သံခိတ် ထိန်းကွပ်ကိန်းများ ကို ပေးပို့နိုင်ပါသည်။

#### Sub-domain လမ်းကြောင်းများ မှတ်ပုံတင်ခြင်း

	Route::group(array('domain' => '{account}.myapp.com'), function()
	{

		Route::get('user/{id}', function($account, $id)
		{
			//
		});

	});

<a name="route-prefixing"></a>
## လမ်းကြောင်းရှေ့ ဆွယ်ပေးခြင်း

လမ်းကြောင်း အုပ်စု တစ်ခု အား `prefix` ထိန်းကွပ်ကိန်းအား `group` array တွင် ထည့်သွင်း၍ ရှေ့ ဆွယ် လမ်းကြောင်းတစ်ခုပေးနိုင်ပါသည်။

	Route::group(array('prefix' => 'admin'), function()
	{

		Route::get('user', function()
		{
			//
		});

	});

<a name="route-model-binding"></a>
## လမ်းကြောင်း နှင့် Model ချိတ်တွယ်ခြင်း

Model ချိတ်တွယ်ခြင်း သည် model instance တစ်ခုအား လမ်းကြောင်းများ အတွင်းသို့ အလွယ်တကူ ထိုးသွင်းနိုင်စေပါသည်။ ဥပမာ user တစ်ယောက်၏ id ကို လမ်းကြောင်းအတွင်း ထည့်သွင်းမည့်အစား ပေးထားသော id နှင့် ကိုက်ညီသည့် user model instance တစ်ခုကို တိုက်ရိုက်ထည့်သွင်းနိုင်ပါသည်။ ပထမဦးစွာ`Route::model` method ကို အသုံးပြု ပြီး ပေးထားသော ထိန်းကွပ်ကိန်းအတွင်း အသုံးပြု မည့် model အမည်ကို သတ်မှတ်ပေးရပါမည်။

#### ထိန်းကွပ်ကိန်းတစ်ခုအား model တစ်ခုဖြင့် ချိတ်တွယ်ခြင်း

	Route::model('user', 'User');

ပြီးနောက် `{user}` ထိန်းကွပ်ကိန်းပါ၀င်သည့် လမ်းကြောင်းတစ်ခု သတ်မှတ်ပေးရပါမည်။

	Route::get('profile/{user}', function(User $user)
	{
		//
	});

`{user}` ထိန်းကွပ်ကိန်းကို `User` model ဖြင့် ချိတ်တွယ်ခဲ့သဖြင့် `User` instance တစ်ခုကို လမ်းကြောင်းအတွင်းသို့ ထိုးသွင်းပါလိမ့်မည်။ ဥပမာအားဖြင့် `profile/1` သို့ လာသော တောင်းဆိုချက်တစ်ခုသည် ID 1 ရှိသော `User` instance တစ်ခုကို ထိုးသွင်းပါလိမ့်မည်။

>**မှတ်ချက်** အကယ်၍ ကိုက်ညီသည့် model instance တစ်ခုကို database တွင် ရှာမတွေ့ ပါက 404 error ဖြစ်ပေါ်ပါလိမ့်မည်။

အကယ်၍ မိမိဘာသာ "not found" တုန့်ပြန်ချက်တစ်ခု သတ်မှတ်လိုပါက `model` method တွင် Closure တစ်ခုအား တတိယ arugment အဖြစ် ပေးပို့နိုင်ပါသည်။

	Route::model('user', 'User', function()
	{
		throw new NotFoundHttpException;
	});

တစ်ခါတစ်ရံ ကိုယ်တိုင် လမ်းကြောင်းထိန်းကွပ်ကိန်းများ မိမိ ဘာသာ ဖြည်လိုခြင်း မျိ  ုးရှိနိုင်ပါသည်။ ထို့ အတွက် `Route::bind` method ကို သုံးလိုက်ရုံပင်။

	Route::bind('user', function($value, $route)
	{
		return User::where('name', $value)->first();
	});

<a name="throwing-404-errors"></a>
## 404 error များ ထုတ်လွှတ်ခြင်း

လမ်းကြောင်းတစ်ခု ဆီမှ 404 error တစ်ခု ဖြစ်ပေါ်အောင် ကိုယ်တိုင် ပြု လုပ်နည်း နှစ်မျ  ိုး ရှိပါသည်။ ပထမတစ်နည်း မှာ `App::abort` method ကို အသုံးပြု ခြင်းဖြစ်သည်။

	App::abort(404);

ဒုတိယတည်နည်းမှာ `Symfony\Component\HttpKernel\Exception\NotFoundHttpException` ကို ကိုယ်တိုင် ထုတ်လွှတ်ခြင်းဖြစ်သည်။

404 exception များ ကိုင်တွယ်ခြင်း နှင့် ၎င်းတို့ အတွက် ကိုယ်ပိုင်တုန့်ပြန်ချက်များ ပြု လုပ်ခြင်းတို့ နှင့် ပတ်သက်၍ [errors](errors#handling-404-errors.md) အပိုင်းတွင် ပိုမို ဖတ်ရှုနိုင်ပါသည်။

<a name="routing-to-controllers"></a>
## Controller များ အား လမ်းကြောင်းပေးခြင်း

Laravel တွင် လမ်းကြောင်းပေးရာ၌ Closure များ ကိုသာ မဟုတ် controller class များကို လည်း အသုံးပြု နိုင်သည့် အပြင်  [resource controllers](/docs/controllers#resource-controllers လမ်းကြောင်းများ ပါ ခွင့်ပြုထားပါသည်။

[Controllers](controllers.md) လမ်းညွှန် တွင်အသေးစိတ် ဖတ်ရှု နိုင်ပါသည်။
