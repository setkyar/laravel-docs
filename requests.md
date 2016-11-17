# Requests နှင့် Input များအကြောင်း

- [Basic Input](#basic-input)
- [Cookies](#cookies)
- [Old Input](#old-input)
- [Files](#files)
- [Request Information](#request-information)

<a name="basic-input"></a>
## Basic Input

You may access all user input with a few simple methods. You do not need to worry about the HTTP verb used for the request, as input is accessed in the same way for all verbs.

Http verb တွေအားလုံးက input ဆီကို ဝင်ရောက်လာတဲ့အချိန်မှာ Simple methods တွေနဲ့ users အားလုံးရဲ့ input တွေကို access လုပ်နိုင်ပါတယ်။ Request တွေအတွက် HTTP verb တွေကိုစိုးရိမ်စရာမလိုပါဘူး။

#### Input Value တစ်ခုကိုပြန်လည်ရချင်ရင်

	$name = Input::get('name');

#### Input မှာ Value မရှိသေးဘဲ Default Value ပြချင်ရင် -

	$name = Input::get('name', 'Sally');

#### Input Value ရှိတာကိုဆုံးဖြတ်ဖို့-

	if (Input::has('name'))
	{
		//
	}

#### Input အားလုံးရဲ့ Request ကိုရချင်ရင်-

	$input = Input::all();

#### Input တစ်ချို့ရဲ့ Request အားလုံးကိုရချင်ရင်-

	$input = Input::only('username', 'password');

	$input = Input::except('credit_card');

When working on forms with "array" inputs, you may use dot notation to access the arrays:
Form တွေကို arrays input တွေနဲ့အသုံးပြုတဲ့အခါမှာ arrays တွေကို access လုပ်ဖို့ "." သင်္ကေတကိုအသုံးပြုရပါမယ်။

	$input = Input::get('products.0.name');

> **Note:** Some JavaScript libraries such as Backbone may send input to the application as JSON. You may access this data via `Input::get` like normal.

<a name="cookies"></a>
## Cookies

Cookies အားလုံးကို Laravel Framework က authernication code နဲ့ encrypted လုပ်ထားပါတယ်၊ ဒါကဘာကိုဆိုလိုတာလဲဆိုရင် cookie တွေကို client ကပြောင်းလိုက်ပြီဆိုရင် သူတို့တရားမဝင်တာကိုနားလည်လိမ့်မယ်။

#### Cookie တစ်ခုရဲ့ Value ကိုရချင်ရင်

	$value = Cookie::get('name');

#### Response တစ်ခုဆီကို Cookie အသစ်တစ်ခု attach လုပ်ချင်ရင် -

	$response = Response::make('Hello World');

	$response->withCookie(Cookie::make('name', 'value', $minutes));

#### နောက် Response တစ်ခုအတွက် Cookie တစ်ခုကို Queue လုပ်ခြင်း
Response မလုပ်ခင်မှာ cookie တစ်ခုကို set ချင်တယ်ဆို့င်ရင် `Cookie::queue()` method ကိုသုံးပါ။ သင့် application မှ နောက်ဆုံး response ကို cookie က အလိုလို attach လုပ်သွားပါလိမ့်မယ်။

	Cookie::queue($name, $value, $minutes);

#### Creating A Cookie That Lasts Forever

	$cookie = Cookie::forever('name', 'value');

<a name="old-input"></a>
## Old Input

သင့်အနေနဲ့ request တစ်ခုကနေ တစ်ခု အကူးအပြောင်းအထိ input တွေကိုထိမ်းသိမ်းထားချင်ပါလိမ့်မယ်... ဥပမာ သင့်အနေနဲ့ form input တွေကို validation လုပ်ပြီး errors message နဲ့အတူ input တွေကိုပြန်ပြတဲ့ အချိန်မျိုးပေါ့။

#### Flashing Input To The Session

	Input::flash();

#### Flashing Only Some Input To The Session

	Input::flashOnly('username', 'email');

	Input::flashExcept('password');

Since you often will want to flash input in association with a redirect to the previous page, you may easily chain input flashing onto a redirect.

	return Redirect::to('form')->withInput();

	return Redirect::to('form')->withInput(Input::except('password'));

> **Note:** You may flash other data across requests using the [Session](session.md) class.

#### Input Data အဟောင်းတွေကိုပြန်ကြည့်ချင်ရင် -

	Input::old('username');

<a name="files"></a>
## Files

#### File Upload တစ်ခုကိုပြန်ကြည့်ချင်ရင် -

	$file = Input::file('photo');

#### File upload လုပ်သွားလား မသွားလား ဆုံးဖြတ်ခြင်ရင်

	if (Input::hasFile('photo'))
	{
		//
	}

The object returned by the `file` method is an instance of the `Symfony\Component\HttpFoundation\File\UploadedFile` class, which extends the PHP `SplFileInfo` class and provides a variety of methods for interacting with the file.

#### File Upload လုပ်တာမှားလားစစ်ချင်ရင် -

	if (Input::file('photo')->isValid())
	{
		//
	}

#### Upload File ကို Move လုပ်ချင်ရင်

	Input::file('photo')->move($destinationPath);

	Input::file('photo')->move($destinationPath, $fileName);

#### File Upload လုပ်သွားတဲ့ လမ်းကြောင်းရချင်ရင် -

	$path = Input::file('photo')->getRealPath();

#### Upload File ရဲ့ မူလအမည်ကိုရချင်ရင် -

	$name = Input::file('photo')->getClientOriginalName();

#### Upload File ရဲ့ extension ကိုသိချင်ရင်

	$extension = Input::file('photo')->getClientOriginalExtension();

#### Upload လုပ်လိုက်တဲ့ File Size ကိုသိချင်ရင်

	$size = Input::file('photo')->getSize();

#### Upload File ရဲ့ MIME Type ကိုသိချင်ရင်

	$mime = Input::file('photo')->getMimeType();

<a name="request-information"></a>
## Request Information

The `Request` class provides many methods for examining the HTTP request for your application and extends the `Symfony\Component\HttpFoundation\Request` class. Here are some of the highlights.

#### Request URI ရဲ့ လမ်းကြောင်းကိုသိချင်ရင်

	$uri = Request::path();

#### Request Method ကို retrieving လုပ်ချင်ရင်

	$method = Request::method();

	if (Request::isMethod('post'))
	{
		//
	}

#### Request လမ်းကြောင်းက pattern တစ်ခုနဲ့ mathces ဖြစ်လားဆိုတာကိုဆုံးဖြတ်ချင်ရင် -

	if (Request::is('admin/*'))
	{
		//
	}

#### Request URL ကိုရယူခြင်ရင်

	$url = Request::url();

#### Request URI segment ကို retrieve လုပ်ချင်ရင်

	$segment = Request::segment(1);

#### Request Header ကိုရချင်ရင် -

	$value = Request::header('Content-Type');

#### Retrieving Values From $_SERVER

	$value = Request::server('PATH_INFO');

#### Request က HTTPS ကလားဆိုတာကိုစစ်ချင်ရင် -

	if (Request::secure())
	{
		//
	}

#### Request က AJAX သုံးထားလားဆိုတာကိုစစ်ချင်ရင်

	if (Request::ajax())
	{
		//
	}

#### Request မှာ JSON Content Type ရှိလားဆိုတာကိုစစ်ချင်ရင်

	if (Request::isJson())
	{
		//
	}

#### Request က JSON ကို တောင်းလားဆိုတာကိုစစ်ချင်ရင်

	if (Request::wantsJson())
	{
		//
	}

#### Request ရဲ့ Response ကို Check လုပ်ချင်ရင်

The `Request::format` method will return the requested response format based on the HTTP Accept header:

	if (Request::format() == 'json')
	{
		//
	}
