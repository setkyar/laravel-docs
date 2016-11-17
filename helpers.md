# အကူ Function မ်ား

- [Arrays](#arrays)
- [Paths](#paths)
- [Strings](#strings)
- [URLs](#urls)
- [Miscellaneous](#miscellaneous)

<a name="arrays"></a>
## Arrays

### array_add

`array_add` function သည္ အကယ္၍ ပထမ argument array တြင္ထပ္မံေပး လုိက္ေသာ key နွင့္ တန္ဖုိး အတြဲ ရွိျပီးသား မဟုတ္ခဲ့ပါက ထုိ key နွင့္ တန္ဖုိးအတြဲအား ထပ္မံေပါင္းထည့္ေပးပါသည္။

	$array = array('foo' => 'bar');

	$array = array_add($array, 'key', 'value');

### array_divide

`array_divide` function သည္ မူလ array အား key မ်ားပါ၀င္ေသာ array နွင့္ တန္ဖုိးမ်ား ပါ ၀င္ေသာ array နွစ္ခု ပါ၀င္ေသာ array အျဖစ္ return ျပန္ေပးပါသည္။

	$array = array('foo' => 'bar');

	list($keys, $values) = array_divide($array);

### array_dot

`array_dot` function သည္ dimension တစ္ခုထက္ပုိ၍ ပါ၀င္ေသာ array တစ္ခုကုိ "." သေကၤတသုံးေရးနည္းျဖင့္ အဆင့္ဆင့္ေခါ္ယူနုိင္ေသာ dimension တစ္ခုတည္း ရွိ array တစ္ခု အျဖစ္ေျပာင္းလဲေပးပါသည္။

	$array = array('foo' => array('bar' => 'baz'));
ၡ
	$array = array_dot($array);

	// array('foo.bar' => 'baz');

### array_except

`array_except` method သည္ ေပးလုိက္ေသာ key နွင့္ တန္ဖုိး အတြဲကုိ ပထမ argument array ထံမွ ဖယ္ရွားေပးပါသည္။

	$array = array_except($array, array('keys', 'to', 'remove'));

### array_fetch

`array_fetch` method သည္ ပထမ argument array အတြင္းတြင္ အဆင့္ဆင့္ ညွပ္သုံးထားေသာ ေပးထားသည့္ ဒုတိယ argument နွင့္ ကုိက္ညီသည့္ nested array element မ်ားအား တစ္ဆင့္တည္း flattened လုပ္ထားေသာ array အျဖစ္ျဖင့္ return ျပန္ေပးပါသည္။ 

	$array = array(
		array('developer' => array('name' => 'Taylor')),
		array('developer' => array('name' => 'Dayle')),
	);

	$array = array_fetch($array, 'developer.name');

	// array('Taylor', 'Dayle');

### array_first

`array_first` method သည္ ေပးထားေသာ array အတြင္း မွ ေပးထားေသာ truth test ကုိ ေျပလည္ ေစ မည့္  ပထမဆုံး element အား return ျပန္ေပးပါသည္။

	$array = array(100, 200, 300);

	$value = array_first($array, function($key, $value)
	{
		return $value >= 150;
	});

default တန္ဖုိးတစ္ခုကုိ လည္း တတိယ parameter အျဖစ္ ထည့္သြင္းေပးနုိင္ပါသည္။

	$value = array_first($array, $callback, $default);

### array_last

`array_last` method သည္ ေပးထားေသာ array အတြင္းမွ ေပးထားေသာ truth test ကုိ ေျပလည္ေစမည့္ ေနာက္ဆုံး element အား return ျပန္ေပးပါသည္။

	$array = array(350, 400, 500, 300, 200, 100);

	$value = array_last($array, function($key, $value)
	{
		return $value > 350;
	});

	// 500

default တန္ဖုိးတစ္ခုကုိ လည္း တတိယ parameter အျဖစ္ ထည့္သြင္းေပးနုိင္ပါသည္။

	$value = array_last($array, $callback, $default);

### array_flatten

`array_flatten` method သည္ ေပးထားေသာ dimension တစ္ခုထက္ပုိသည့္ array တစ္ခုကို တစ္ဆင့္တည္း ရွိေသာ array တစ္ခု အျဖစ္ return ျပန္ေပးပါသည္။

	$array = array('name' => 'Joe', 'languages' => array('PHP', 'Ruby'));

	$array = array_flatten($array);

	// array('Joe', 'PHP', 'Ruby');

### array_forget

`array_forget` method သည္ အဆင့္ဆင့္နက္နဲစြာ ညွပ္ထားေသာ deeply nested array တစ္ခုမွ  "." သေကၤတသုံးေရးနည္းကုိ အသုံးျပု၍ ေရးထားေသာ ေပးရင္း key နွင့္ တန္ဖုိး အတြဲကုိ ဖယ္ရွားေပးပါသည္။

	$array = array('names' => array('joe' => array('programmer')));

	array_forget($array, 'names.joe');

### array_get

`array_get` method သည္ အဆင့္ဆင့္နက္နဲစြာ ညွပ္ထားေသာ deeply nested array တစ္ခုမွ  "." သေကၤတသုံးေရးနည္းကုိ အသုံးျပု၍ ေရးထားေသာ ေပးရင္း key နွင့္ တန္ဖုိး အတြဲကုိ ထုတ္ယူေပးပါသည္။

	$array = array('names' => array('joe' => array('programmer')));

	$value = array_get($array, 'names.joe');

>**မွတ္ခ်က္** အကယ္၍ `array_get` ၏ အလုပ္လုပ္ပုံမ်  ိုး ကုိ object မ်ားတြင္ သုံးလုိပါက `object_get` အားသုံးနုိင္ပါသည္။

### array_only

`array_only` method သည္ ပထမ argument array အတြင္းမွ ေပးထားေသာ key သုိ့မဟုတ္ တန္ဖုိးမ်ား ပါ၀င္သည့္ အတြဲမ်ားကုိသာ return ျပန္ေပးပါသည္။

	$array = array('name' => 'Joe', 'age' => 27, 'votes' => 1);

	$array = array_only($array, array('name', 'votes'));

### array_pluck

`array_pluck` method သည္ ပထမ argument array ထံမွ ေပးထားေသာ key သို့ မဟုတ္ တန္ဖုိး ပါ၀င္သည့္ အတြဲကုိသာ return ျပန္ေပးပါသည္။

	$array = array(array('name' => 'Taylor'), array('name' => 'Dayle'));

	$array = array_pluck($array, 'name');

	// array('Taylor', 'Dayle');

### array_pull

`array_pull` method သည္ ပထမ argument array ထံမွ ေပးထားေသာ key သုိ့ မဟုတ္ တန္ဖုိးပါ၀င္သည့္ အတြဲ ကုိ return ျပန္ျပီး ထုိ အတြဲအား မူလ array ထံမွလည္း ဖယ္ရွားေပးပါသည္။

	$array = array('name' => 'Taylor', 'age' => 27);

	$name = array_pull($array, 'name');

### array_set

`array_set` method သည္ အဆင့္ဆင့္နက္နဲစြာ ညွပ္ထားေသာ array တစ္ခုအတြင္းမွ  "." သေကၤတသုံးေရးနည္းကုိ အသုံးျပု၍ ေရးထားေသာ ဒုတိယ argument နွင့္ကုိက္ညီသည့္ တန္ဖုိးအား ျပု ျပင္ထည့္သြင္းေပးပါသည္။

	$array = array('names' => array('programmer' => 'Joe'));

	array_set($array, 'names.editor', 'Taylor');

### array_sort

`array_sort` method သည္ ေပးထားေသာ array အား ေပးထားေသာ Closure function ၏ ရလဒ္ကုိ အသုံးျပု ၍ စီေပးပါသည္။

	$array = array(
		array('name' => 'Jill'),
		array('name' => 'Barry'),
	);

	$array = array_values(array_sort($array, function($value)
	{
		return $value['name'];
	}));

### array_where

`array_where` သည္ ေပးထားေသာ array အား ေပးထားေသာ Closure function တစ္ခုျဖင့္ filter လုပ္ေပးပါသည္။

	$array = array(100, '200', 300, '400', 500);

	$array = array_where($array, function($key, $value)
	{
		return is_string($value);
	});

	// Array ( [1] => 200 [3] => 400 )

### head

array တစ္ခု၏ ပထမဆုံး element ကုိ return ျပန္ေပးပါသည္။ PHP 5.3.x တြင္ အသုံးျပုနုိင္ေသာ method chaining အတြက္ အသုံး၀င္ပါသည္။

	$first = head($this->returnsArray('foo'));

### last

array တစ္ခု၏ ေနာက္ဆုံး element ကို return ျပန္ေပးပါသည္။ method chaining အတြက္ အသုံး၀င္ပါသည္။

	$last = last($this->returnsArray('foo'));

<a name="paths"></a>
## Paths

### app_path

`app` directory၏ path အျပည့္အစုံကုိ ေပးပါသည္။

	$path = app_path();

### base_path

application ကုိ install လုပ္ထားေသာ root directory ၏ path အျပည့္အစုံကုိ ေပးပါသည္။

### public_path

`public` directory ၏ path အျပည့္အစုံကုိ ေပးပါသည္။

### storage_path

`app/storage` directory ၏ path အျပည့္အစုံကုိ ေပးပါသည္။

<a name="strings"></a>
## Strings

### camel_case

ေပးထားေသာ string အား `camelCase` ေရးဟန္သုိ့ ေျပာင္းလဲေပးပါသည္။

	$camel = camel_case('foo_bar');

	// fooBar

### class_basename

ေပးထားေသာ class ၏  အမည္ကုိ namespace အမည္မ်ား မပါ၀င္ပဲ ေပးပါသည္။

	$class = class_basename('Foo\Bar\Baz');

	// Baz

### e

`htmlentities` function အား ေပးထားေသာ string ျဖင့္ run ပါသည္။ UTF-8 support ပါ၀င္ပါသည္။

	$entities = e('<html>foo</html>');

### ends_with

ပထမ argument တြင္ ေပးထားေသာ string တစ္ခုသည္ ေပးထားေသာ string နွင့္ အဆုံးသတ္ျခင္းရွိမရွိ ဆုံးျဖတ္ေပးပါသည္။

	$value = ends_with('This is my name', 'name');

### snake_case

ေပးထားေသာ string အား `snake_case` ေရးဟန္ သုိ့ ေျပာင္းလဲေပးပါသည္။

	$snake = snake_case('fooBar');

	// foo_bar

### str_limit

string တစ္ခု အတြင္းရွိ အကၡရာအေရအတြက္ကုိ ကန့္သတ္ေပးပါသည္။

	str_limit($value, $limit = 100, $end = '...')

ဥပမာ

	$value = str_limit('The PHP framework for web artisans.', 7);

	// The PHP...

### starts_with

ပထမ argument တြင္ ေပးထားေသာ string တစ္ခုသည္ ေပးထားေသာ string နွင့္ စတင္ျခင္းရွိမရွိ ဆုံးျဖတ္ေပးပါသည္။

	$value = starts_with('This is my name', 'This');

### str_contains

ပထမ argument တြင္ ေပးထားေသာ string တြင္ ေပးထားေသာ string ပါ၀င္ျခင္းရွိမရွိ ဆုံးျဖတ္ေပးပါသည္။

	$value = str_contains('This is my name', 'my');

### str_finish

ပထမ argument တြင္ေပးထားေသာ string ၏ အဆုံးတြင္ ေပးထားေသာ string အားေပါင္းထည့္ေပးပါသည္။

	$string = str_finish('this/string', '/');

	// this/string/

### str_is

ေပးထားေသာ pattern တစ္ခုသည္ ေပးထားေသာ string နွင့္ ကုိက္ညီ မကုိက္ညီ ဆုံးျဖတ္ေပးပါသည္။ ခေရပြင့္အကၡရာ "*" မ်ားအား wildcard အကၡရာအျဖစ္သုံးနုိင္ပါသည္။

	$value = str_is('foo*', 'foobar');

### str_plural

ေပးထားေသာ string တစ္ခုအား English ဘာသာ ျဖင့္ အမ်ားကိန္း (plural) ပုံစံသုိ့ ေျပာင္းလဲေပးပါသည္။

	$plural = str_plural('car');

### str_random

ေပးထားေသာ အေရအတြက္အတုိင္း အတိအက် ရွိသည့္ က်ပန္း string တစ္ခုကုိ ထုတ္ေပးပါသည္။

	$string = str_random(40);

### str_singular

ေပးထားေသာ string တစ္ခုအား English ဘာသာ ျဖင့္ အနည္းကိန္း (singular) ပုံစံသုိ့ ေျပာင္းလဲေပးပါသည္။

	$singular = str_singular('cars');

### studly_case

ေပးထားေသာ string အား `StudlyCase` ေရးဟန္သုိ့ ေျပာင္းလဲေပးပါသည္။

	$value = studly_case('foo_bar');

	// FooBar

### trans

ေပးထားေသာ string တစ္ေျကာင္းအား ဘာသာျပန္ေပးပါသည္။ `Lang::get` method ၏ အမည္ေျပာင္း method တစ္ခုျဖစ္ပါသည္။

	$value = trans('validation.required'):

### trans_choice

ေပးထားေသာ string တစ္ေျကာင္းအား ေပးထားေသာ နံပါတ္ အလုိက္ ဘာသာျပန္ message ကုိ အသုံးျပု၍ ဘာသာျပန္ေပးပါသည္။ `Lang::choice` method ၏ အမည္ေျပာင္း method တစ္ခုျဖစ္ပါသည္။

	$value = trans_choice('foo.bar', $count);

<a name="urls"></a>
## URLs

### action

ေပးထားေသာ controller action တစ္စုံအတြက္ URL တစ္ခု ထုတ္ေပးပါသည္။

	$url = action('HomeController@getIndex', $params);

### route

ေပးထားေသာ အမည္ရွိ လမ္းေျကာင္း တစ္ခုအတြက္ URL တစ္ခု ထုတ္ေပးပါသည္။

	$url = route('routeName', $params);

### asset

ေပးထားေသာ asset အတြက္ URL တစ္ခု ထုတ္ေပးပါသည္။

	$url = asset('img/photo.jpg');

### link_to

ေပးထားေသာ URL အတြက္ HTML link တစ္ခု ထုတ္ေပးပါသည္။

	echo link_to('foo/bar', $title, $attributes = array(), $secure = null);

### link_to_asset

ေပးထားေသာ asset အတြက္ HTML link တစ္ခု ထုတ္ေပးပါသည္။

	echo link_to_asset('foo/bar.zip', $title, $attributes = array(), $secure = null);

### link_to_route

ေပးထားေသာ လမ္းေျကာင္း တစ္ခု အတြက္ HTML link တစ္ခု ထုတ္ေပးပါသည္။

	echo link_to_route('route.name', $title, $parameters = array(), $attributes = array());

### link_to_action

ေပးထားေသာ controller action တစ္စုံအတြက္ HTML link တစ္ခု ထုတ္ေပးပါသည္။

	echo link_to_action('HomeController@getIndex', $title, $parameters = array(), $attributes = array());

### secure_asset

ေပးထားေသာ asset အတြက္ HTTPS သုံးထားေသာ HTML link တစ္ခု ထုတ္ေပးပါသည္။

	echo secure_asset('foo/bar.zip', $title, $attributes = array());

### secure_url

ေပးထားေသာ path တစ္ခု အတြက္ URL အျပည့္အစုံ တစ္ခု ထုတ္ေပးပါသည္။

	echo secure_url('foo/bar', $parameters = array());

### url

ေပးထားေသာ path တစ္ခု အတြက္ URL အျပည့္အစုံ ထုတ္ေပးပါသည္။

	echo url('foo/bar', $parameters = array(), $secure = null);

<a name="miscellaneous"></a>
## Miscellaneous

### csrf_token

လက္ရွိ CSRF token တန္ဖုိးကုိ ေပးပါသည္။

	$token = csrf_token();

### dd

ေပးထားေသာ variable ကုိ dump လုပ္၍ script execution ကုိ ရပ္ေစပါသည္။

	dd($value);

### value

ေပးထားေသာ တန္ဖုိးသည္ `Closure` တစ္ခု ျဖစ္ပါက `Closure` မွတစ္ဆင့္ return ျပန္လာေသာ value ကုိ return ျပန္ေပး၍ `Closure` မဟုတ္ပါက တန္ဖုိးအတုိင္း return ျပန္ေပးပါသည္။

	$value = value(function() { return 'bar'; });

### with

ေပးထားေသာ object ကုိ return ျပန္ေပးပါသည္။ PHP 5.3.x တြင္ method chaining constructor မ်ား အတြက္ အသုံး၀င္ပါသည္။

	$value = with(new Foo)->doWork();