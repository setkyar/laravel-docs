# Views နှင့် Responses များအကြောင်း

- [Basic Responses](#basic-responses)
- [Redirects](#redirects)
- [Views](#views)
- [View Composers](#view-composers)
- [Special Responses](#special-responses)
- [Response Macros](#response-macros)

<a name="basic-responses"></a>
## Basic Responses

#### String တစ်ခုကို Routes ကနေ return ပြန်ချင်ရင် -

	Route::get('/', function()
	{
		return 'Hello World';
	});

#### Creating Custom Responses

Symfony\Component\HttpFoundation\Response` class ကနေ Response` တစ်ခုကဖြစ်လာတယ်၊  HTTPS responses တွေကို တည်ဆောက်ဖို့ရာအတွက် များစွာသော methods တွေကနေ စီစဉ်ပေးပါတယ်။

	$response = Response::make($contents, $statusCode);

	$response->header('Content-Type', $value);

	return $response;

သင်က `Response` class တစ်ခုရဲ့ method ကိုလည်းလိုချင်တယ်... ဒါပေမယ့် response content အဖြစ် return ပြန်ချင်တယ် ဆိုရင်တော့`Response::view` method ကအဆင်ပြေပါလိမ့်မယ်-

	return Response::view('hello')->header('Content-Type', $type);

#### Cookies တွေကို Responses တွေဆီပြန်ချင်တယ်ဆိုရင်

	$cookie = Cookie::make('name', 'value');

	return Response::make($content)->withCookie($cookie);

<a name="redirects"></a>
## ပြန်လည်လမ်းကြောင်းညွှန်ကြားမှူ့

#### Redirect လုပ်ချင်တယ်ဆိုရင် -

	return Redirect::to('user/login');

#### Flash Data နဲ့ Redirect လုပ်ရင် -

	return Redirect::to('user/login')->with('message', 'Login Failed');

> **Note:** Since the `with` method flashes data to the session, you may retrieve the data using the typical `Session::get` method.

#### Nmaed Route နှင့် Redirect လုပ်ရင်-

	return Redirect::route('login');

#### Route Parameters တစ်ခုနဲ့ Redirect လုပ်ရင် -


	return Redirect::route('profile', array(1));

#### Route ထဲမှာ name parameters ပါတာကို Redirect လုပ်ရင်

	return Redirect::route('profile', array('user' => 1));

#### Controller ရဲ့ Action တစ်ခုကနေ Redirect တစ်ခု return လုပ်ချင်ရင်

	return Redirect::action('HomeController@index');

#### Paramater ပါတဲ့ Controller တစ်ခုကို Redirect တစ်ခု return လုပ်ခြင်း

	return Redirect::action('UserController@profile', array(1));

#### Name Parameters တစ်ခုပါတဲ့ Controller Action တစ်ခုကနေ Redirect  တစ်ခု return လုပ်ခြင်း

	return Redirect::action('UserController@profile', array('user' => 1));

<a name="views"></a>
## Views 

သင့်ရဲ့ presentation logic ကနေ controller နဲ့ domain logic တွေ ခွဲခြားဖို့ရာအတွက် Views က အဆင်ပြေဆုံးဖြစ်အောင်စီစဉ်ပေးပါတယ်။
Views Files တွေက `app/views` directory ထဲမှာ ရှိပါတယ်။ Views မှာ ထုံးစံအတိုင်း သင့် application ရဲ့ HTML တွေပါဝင်ပါတယ် ။

အောက်မှာဖော်ပြထားတာကတော့ Views နမူနာတစ်ခုပါ:

	<!-- View stored in app/views/greeting.php -->

	<html>
		<body>
			<h1>Hello, <?php echo $name; ?></h1>
		</body>
	</html>

အဲ့ဒီ့အထက်က View ကို browser ကိုအောက်ကလို retun ပြန်ခဲ့ပါတယ်

	Route::get('/', function()
	{
		return View::make('greeting', array('name' => 'Taylor'));
	});

The second argument passed to `View::make` is an array of data that should be made available to the view.

#### Data တွေကို View ဆီကို pass လုပ်ခြင်း

	// Using conventional approach
	$view = View::make('greeting')->with('name', 'Steve');

	// Using Magic Methods
	$view = View::make('greeting')->withName('steve');

အထက်ကဥပမာမှာ `$name` variable ကို view ကနေပြီးတော့ access လုပ်နိုင်ပါတယ်၊ နောက် `Steve` ကောပေါ့။

သင့်အနေနဲ့ data ထဲက array တွေကို `make` method ရဲ့ second partameter မှာ array ဖြစ်တဲ့ data ကို pass လုပ်နိုင်ပါတယ်။ သင်လုပ်ချင်ရင်ပေါ့

	$view = View::make('greetings', $data);

သင့်အနေနဲ့ data နည်းနည်း လေးကို views အားလုံးကို share နိုင်ပါတယ်၊

	View::share('name', 'Steve');

#### View တစ်ခုမှ Sub-View တစ်ခုကို pass လုပ်ခြင်း

တစ်ခါတစ်လေသင့်အနေနဲ့ veiw တစ်ခုကနေ တစ်ခုပြောင်းချင်ပါလိမ့်မယ်။ ဥပမာ၊ ဒုတိယ view တစ်ခုကို `app/views/child/view.php` မှာ stored လုပ်ထားတယ်၊ ကျွန်တော်တို့ နောက်ထက် View တစ်ခုကို Pass လုပ်ချင်တယ်ဆိုရင်... like so:

	$view = View::make('greeting')->nest('child', 'child.view');

	$view = View::make('greeting')->nest('child', 'child.view', $data);

paraent view က sub-view ဆီကနေ render လုပ်နိုင်ပါပြီ-

	<html>
		<body>
			<h1>Hello!</h1>
			<?php echo $child; ?>
		</body>
	</html>

<a name="view-composers"></a>
## View Composers

View က rendered ဖြစ်တဲ့အချိန်မှာ View composers တွေက callbacks ဒါမှမဟုတ်ရင် class methods တွေကို ခေါ်ခဲ့တယ် ။ သင့် application မှ render လုပ်ပြီးတော့ သင့်ရဲ့ view ကိုအချိန်တိုင်း သေချာပေါက်ပေးရမယ့် data ရှိတဲ့အခါမျိုးဆိုရင်  ... အဲ့ဒီ့ code ကို location တစ်ခုထဲကနေ View Composer တစ်ခုက organize လုပ်နိုင်တယ် ။

#### View Composer တစ်ခု သတ်မှတ်ခြင်း

	View::composer('profile', function($view)
	{
		$view->with('count', User::count());
	});

အခု `profile` view က rendered ဖြစ်တဲ့အချိန်တိုင်းမှာ  `count` data က view ဆီကို bound ပါလိမ့်မယ်

View composer တစ်ခုကနေ Multiple Views ကိုတစ်ကြိမ်တည်းသင့်အနေနဲ့ attach လုပ်နိုင်ပါတယ်

    View::composer(array('profile','dashboard'), function($view)
    {
        $view->with('count', User::count());
    });

If you would rather use a class based composer, which will provide the benefits of being resolved through the application [IoC Container](ioc.md), you may do so: 

	View::composer('profile', 'ProfileComposer');

View Composer Class တစ်ခုကို အောက်ကလို define လုပ်နိုင်ပါတယ် :

	class ProfileComposer {

		public function compose($view)
		{
			$view->with('count', User::count());
		}

	}

#### Composer နှစ်ခုသတ်မှတ်ခြင်း

တစ်ချိန်တည်းမှာဘဲ Composers Group တွေကို Register လုပ်ဖို့သင့်အနေနဲ့ `composers` method ကိုသုံးနိုင်ပါတယ်။


	View::composers(array(
		'AdminComposer' => array('admin.index', 'admin.profile'),
		'UserComposer' => 'user',
	));

> **Note:** There is no convention on where composer classes may be stored. You are free to store them anywhere as long as they can be autoloaded using the directives in your `composer.json` file.

### View Creators ( View ဖန်တီးသူများ)

View **creators** တွေက view composers တွေလုပ်သလိုမျိုးတစ်ပုံစံတည်းလုပ်တာပါ။ သို့ပေမယ့်လည်း...view တွေ instantiated ဖြစ်ပြီးပြီဆိုမှ သူတို့က ချက်ချင်း fired လုပ်တာပါ။ View creator တစ်ခုလုပ်ဖို့ Register လုပ်ချင်တယ်ဆိုရင် `creator` method ကိုသုံးပါ။

	View::creator('profile', function($view)
	{
		$view->with('count', User::count());
	});

<a name="special-responses"></a>
## Special Responses 

#### JSON Response တစ်ခုပြုလုပ်ခြင်း

	return Response::json(array('name' => 'Steve', 'state' => 'CA'));

#### JSON Response တစ်ခုပြုလုပ်ခြင်း

	return Response::json(array('name' => 'Steve', 'state' => 'CA'))->setCallback(Input::get('callback'));

#### File Download Response တစ်ခုပြုလုပ်ခြင်း

	return Response::download($pathToFile);

	return Response::download($pathToFile, $name, $headers);

> **Note:** Symfony HttpFoundation, which manages file downloads, requires the file being downloaded to have an ASCII file name.

<a name="response-macros"></a>
## Response Macros

သင့်အနေနဲ့ကိုယ်ပိုင် response တစ်ခုပြုလုပ်ပြီးတော့ routes နဲ့ controllers တွေကနေပြန်ပြီးတော့အသုံးပြုချင်တယ်ဆိုရင်... သင့်အနေနဲ့ `Response::macro` method ကိုသုံးနိုင်ပါတယ်

	Response::macro('caps', function($value)
	{
		return Response::make(strtoupper($value));
	});

`micro` function ကသူ့ရဲ့  name တစ်ခုကို first argument အဖြစ်လက်ခံထားတယ်၊ နောက် Closure ကတော့ သူ့ရဲ့ဒုတိယတစ်ခုပါ။ micro name က `Response` class ကို ခေါ်တဲ့အချိန်မှာ macro closure က execute ဖြစ်သွားပါတယ် :

	return Response::caps('foo');

micros တွေကို သင့်ရဲ့ `app/start`  files ထဲမှာ define လုပ်ထားရပါမယ်။  တစ်နည်းအားဖြင့် သင့် separate လုပ်ထားတဲ့ macros တွေကို `start` files မှာသင်ပြန် organize လုပ်ရပါမယ်။
