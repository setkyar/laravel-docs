# Controllers များအကြောင်း

- [Basic Controllers](#basic-controllers)
- [Controller Filters](#controller-filters)
- [RESTful Controllers](#restful-controllers)
- [Resource Controllers](#resource-controllers)
- [Handling Missing Methods](#handling-missing-methods)

<a name="basic-controllers"></a>
## Basic Controllers

Application ရဲ့  Route ပိုင်းဆိုင်ရာအာလုံးကို `routes.php` တဖိုင်တည်းမှာ အားလုံး သတ်မှတ်ထားသလို ၊ အဲဒီ route တွေရဲ့  Action အားလုံးကို Controller က Class တွေနဲ့ ထိန်းချုပ်နိုင်မှာပါ။ Controllers က routes မှာသတ်မှတ်တဲ့ Action တွေကို ထိန်းချုပ်ပေးယုံ သာမက Framework တခုအတွက် အားသာချက်တခုဖြစ်တဲ့ [Automatic dependency injection][1] တွေပါအသုံးပြုနိုင်မှာပါ။

Controllers ဖိုင်တွေကို ပုံမှန်အားဖြင့် `app/controllers` အောက်မှာ သိမ်းဆည်းထားပါတယ်။ အဲဒီ Controller ဖိုင်တွေကို  `Composer.json` မှာ `Classmap စနစ်ဖြင့်အသုံးပြုထားပါတယ်။ဘယ်လိုပဲဖြစ်ဖြစ် Controllers တွေက Application ရဲ့ ဘယ်နေရာမှာ မဆို အလုပ်လုပ်နိုင်ပါတယ်။ Route မှာ Controllers ကို သိမ်းဆန်းထားတဲ့နေရာအတွက် သတ်မှတ်ချက်တွေက မရှိပါ။ဘာလို့လဲဆိုတော့ Composer က Classmap autoload သုံးပြုထားတဲ့ controller class ကို အလိုလို သိမှတ်ပြုပြီးသားဖြစ်နေလို့ပါ။Controllers ဖိုင်တွေကို နှစ်သက်ရာ နေရာမှာ သိမ်းဆည်းပြီး အလုပ်လုပ်နိုင်ပါတယ်။

အခြေခံအားဖြင့် Controller Class တခုရဲ့ ပုံစံက အောက်ပါအတိုင်းရေးသားပါတယ်။

	class UserController extends BaseController {

		/**
		 * Show the profile for the given user.
		 */
		public function showProfile($id)
		{
			$user = User::find($id);

			return View::make('user.profile', array('user' => $user));
		}

	}


Controllers တွေအားလုံးက `BaseController` Class ကို ထပ်ကွန့် အသုံးပြုရမှာပါ။ `BaseController` ကိုလဲ `app/controllers` မှာ သိမ်းဆည်းထားပြီး ပြန်လည်အသုံးပြုနိုင်တဲ့ ဖိုင်အနေနဲ့သိမ်းဆည်းထားပါတယ်။`BaseContoller` ဆိုတာက Laravel framework ရဲ့  `Controller` Class ကို ထပ်ကွန့် အသုံးပြုထားပါတယ်။အခုဆိုရင်တော့ Route ဖိုင်မှ တဆင့် အသုံးပြုလိုတဲ့ Controller action ကို အောက်ပါအတိုင်းသတ်မှတ်နိုင်ပါပြီ။

	Route::get('user/{id}', 'UserController@showProfile');


PHP ရဲ့  magic function namespaces ကို အသုံးပြုပြီး Controller ကို သတ်မှတ်မယ်ဆိုရင်တော့ အောက်ကပုံစံအတိုင်း Route ပေးတဲ့အချိန်မှာ သတ်မှတ်နိုင်ပါတယ်။

	Route::get('foo', 'Namespace\FooController@method');

> **Note::** အခုချိန်မှာ ကျွန်တော်တို့က PHP classes တွေ ကို autoload အသုံးပြုနိုင်ရန်အတွက် [Composer](http://getcomposer.org) ကို အသုံးပြုထားပါတယ်။ ဒါကြောင့် Controller file ကို System ရဲ့ ဘယ်နေရာမှာပဲထားထား ၊ Composer က အဲဒီ ဖိုင်ကို သိနေသ၍ အသုံးပြုနိုင်မှာပါ။ Controllers ဖိုင်ရဲ့ တည်နေရာကို အတည်တကျ အသေ သတ်မှတ်ထားသလိုမျိုးမရှိပါဘူး။Routing to controllers is entirely de-coupled from the file system.


Controller ကို သတ်မှတ်ထားတဲ့ Route ကို အောက်ကပုံစံအတိုင်း သတ်မှတ်နိုင်ပါတယ်။

	Route::get('foo', array('uses' => 'FooController@method','as' => 'name'));

Controller Action ပါတဲ့ URL ( Link ) တခု ဖန်တီးဖို့အတွက် `URL::action` ဆိုပြီးသုံးပြုနိုင်သလို `action` helper function ကိုလဲ အောက်ပါအတိုင်းသုံးပြုနိုင်ပါတယ်။

	$url = URL::action('FooController@method');

	$url = action('FooController@method');

Controller action တစ်ခုရဲ့ နာမည်ကို သိရန်အတွက် 'currentRouteAction' method ကို အောက်က ပုံစံအတိုင်း အသုံးပြုနိုင်ပါတယ်။

	$action = Route::currentRouteAction();

<a name="controller-filters"></a>
## Controller Filters

[Filters](routing#route-filters.md) က Controller ပါတဲ့ route တခုသတ်မှတ်ကတည်းက အောက်ပါပုံစံအတိုင်း သတ်မှတ်ခဲ့ပါတယ်။

	Route::get('profile', array('before' => 'auth',
				'uses' => 'UserController@showProfile'));

နောက်တစ်နည်းက Controller ဖိုင်ထဲရောက်မှ အောက်ကပုံစံအတိုင်းလဲ အသုံးပြုနိုင်ပါတယ်။

	class UserController extends BaseController {

		/**
		 * Instantiate a new UserController instance.
		 */
		public function __construct()
		{
			$this->beforeFilter('auth', array('except' => 'getLogin'));

			$this->beforeFilter('csrf', array('on' => 'post'));

			$this->afterFilter('log', array('only' =>
								array('fooAction', 'barAction')));
		}

	}


You may also specify controller filters inline using a Closure:

	class UserController extends BaseController {

		/**
		 * Instantiate a new UserController instance.
		 */
		public function __construct()
		{
			$this->beforeFilter(function()
			{
				//
			});
		}

	}

Controller တစ်ခုအတွက် Filter ကို သီးခြားအသုံးပြုချင်ရင်တော့ `@` ဆိုတဲ့ syntax ကို အသုံးပြုပြီး အောက်ပါအတိုင်း သတ်မှတ်ပေးရပါတယ်။

	class UserController extends BaseController {

		/**
		 * Instantiate a new UserController instance.
		 */
		public function __construct()
		{
			$this->beforeFilter('@filterRequests');
		}

		/**
		 * Filter the incoming requests.
		 */
		public function filterRequests($route, $request)
		{
			//
		}

	}

<a name="restful-controllers"></a>

## RESTful Controllers

Laravel တွင် Controller တွေရဲ့  Action အားလုံးအတွက် REST ဖြင့် Route ကနေ အလွယ်တကူ သတ်မှတ်နိုင်ပါတယ်။ ပထမဆုံး method အတွက် Route  ကို 'Route::controller' ဖြင့် သတ်မှတ်ပါ။

	Route::controller('users', 'UserController');

`controller` method တွင် arguments နှစ်ခု လက်ခံပါတယ်။ ပထမတခုက Base URL controller handles( ဥပမာ. create, index ) ဖြစ်ပြီး ၊ ဒုတိယတခုကတော့ Controller ရဲ့ နာမည်ဖြစ်ပါတယ်။ နောက်တစ်ခုက Controller တွင် Method(getindex/postprofile)တွေကို HTTP verb အလိုက် ထည့်ပေးရပါမယ်။

	class UserController extends BaseController {

		public function getIndex()
		{
			//
		}

		public function postProfile()
		{
			//
		}

	}

`index` method က route မှာပေးထားတဲ့ index ကို အလုပ်လုပ်ပါတယ်။ ကျွန်တော်တို့ အပေါ်မှာဆို route name ကို users ဆိုပြီးပေးခဲ့ပါတယ်။ အဲဒါဆိုရင် users လို့ခေါ်ရင် Controller ထဲက index က အလုပ်လုပ်သွားမှာပါ။

Controller action မှာ စကားစုအများကြီးပါလာသုံးခဲ့ရင် `dash` syntax သုံးနည်းနဲ့ Mathod ကို သတ်မှတ်ပေးရပါတယ်။ ဥပမာ "OurController" ထဲက method တစ်ခု ကို getAdminProfile လို့ပေးထားရင် url ကို `users/admin-profile` လို့ခေါ်လို့ရသွားမှာပါ။

	public function getAdminProfile() {}

<a name="resource-controllers"></a>
## Resource Controllers

Resource controllers က sources တွေ အသုံးပြုပြီး Restful controller တွေ ဖန်တီးတဲ့အခါ အလွယ်တကူအသုံးပြုနိုင်အောင်လုပ်ဆောင်ပေးပါတယ်။ ဥပမာ ဓါတ်ပုံ စီမံတဲ့ controller တစ်တခု ပြုလုပ်မယ်ဆိုပါစို ့။ Terminal(CMD) ကနေ တစ်ဆင့် Artisan command ဖြင့် `controller:make` ကိုအသုံးပြုပြီးတော့ `Route::resource` ဆိုပြီး route သတ်မှတ်ပေးလိုက်ရင် resoure controller တစ်ခု အလွယ်တကူရရှိမှာဖြစ်ပါတယ်။

Controller တစ်ခုကို Terminal(cmd) ကနေ ဖန်တီးဖို့အတွက် အောက်က ကွန်မန်းဖြင့် စမ်းကြည့်ပါ။

	php artisan controller:make PhotoController

ပြီးရင်တော့ အပေါ်မှာ လုပ်ထားတဲ့ Controller ကို အသုံးပြုနိုင်ရန်အတွက်အောက်ကအတိုင်း Route မှာ သတ်မှတ်ပေးပါ။

	Route::resource('photo', 'PhotoController');


အခုဆိုရင် သာမန် Route လေးတစ်ကြောင်းနဲ့ Photo အတွက် RESTful action တစ်ခုရရှိသွားပါပြီ။အခုလိုမျိုးသတ်မှတ်ရုံဖြင့် အောက်ကအတိုင်း Action တွေ အကုန်အသုံးပြုနိုင်သွားပါပြီ။

#### Actions Handled By Resource Controller

Verb      | Path                        | Action       | Route Name
----------|-----------------------------|--------------|---------------------
GET       | /resource                   | index        | resource.index
GET       | /resource/create            | create       | resource.create
POST      | /resource                   | store        | resource.store
GET       | /resource/{resource}        | show         | resource.show
GET       | /resource/{resource}/edit   | edit         | resource.edit
PUT/PATCH | /resource/{resource}        | update       | resource.update
DELETE    | /resource/{resource}        | destroy      | resource.destroy

တခါတလေကျတော့ အပေါ်ကအတိုင်း Controller မှာ Action တွေ အသုံးမလိုတဲ့အခါမျိုးတွေရှိလာရင် အောက်ကအတိုင်း --only ဆိုပြီး သုံးပြုလိုတဲ့ Action ကိုပဲ ရွေးချယ်သတ်မှတ်နိုင်ပါတယ်။

	php artisan controller:make PhotoController --only=index,show

	php artisan controller:make PhotoController --except=index


ပြီးတော့ Route မှ တဆင့်လဲ လိုအပ်တဲ့ Action တွေ ကို only ကို အသုံးပြုပြီး သတ်မှတ်ပေးသွားနိုင်ပါတယ်။

	Route::resource('photo', 'PhotoController',
					array('only' => array('index', 'show')));

	Route::resource('photo', 'PhotoController',
					array('except' => array('create', 'store', 'update', 'destroy')));

ပုံမှန်အားဖြင့် Resource controller action တွေမှာ route name တွေက အလိုလျောက်သတ်မှတ်ပေးထားပြီးသားပါ။တကယ်လို ့ အဲဒီ route name ကို ကိုယ့်ဘာသာ အောက်က ပုံစံအတိုင်း သတ်မှတ်ပေးလို့ရနိုင်ပါသေးတယ်။

	Route::resource('photo', 'PhotoController',
					array('names' => array('create' => 'photo.build')));

#### Adding Additional Routes To Resource Controllers

တကယ်လို့ resource controller မှာ ပုံမှန် routes တွေနဲ့ မလုံလောက်လို့ အသစ်ထပ်ထည့်လို့ရနိုင်ပါသေးတယ်။ အဲဒီ route name ကိုတော့ `Route::resource` ကို မရေးခင်မှာအောက်က ပုံစံအတိုင်း ရေးပေးရပါမယ်။

	Route::get('photos/popular');
	Route::resource('photos', 'PhotoController');

<a name="handling-missing-methods"></a>
## Handling Missing Methods

"catch-all method" ဆိုတာက Controller ပေးထားပြီး Mehod မရှိတဲ့အခါမှာ အလုပ်လုပ်မယ့် အခြေအနေဖြစ်ပါတယ်။ Method နာမည်ကိုတော့ "MissingMethod" ဆိုပြီးအောက်ကပုံစံအတိုင်း အသုံးပြုရမှာပါ။

#### Defining A Catch-All Method

	public function missingMethod($parameters = array())
	{
		//
	}


  [1]: /docs/ioc