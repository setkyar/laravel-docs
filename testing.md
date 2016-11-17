# Unit Testing

- [Introduction](#introduction)
- [Defining & Running Tests](#defining-and-running-tests)
- [Test Environment](#test-environment)
- [Calling Routes From Tests](#calling-routes-from-tests)
- [Mocking Facades](#mocking-facades)
- [Framework Assertions](#framework-assertions)
- [Helper Methods](#helper-methods)
- [Refreshing The Application](#refreshing-the-application)

<a name="introduction"></a>
## Introduction

Laravel ဟာ unit testing ကို အဓိကအခြေခံထားပြီး တည်ဆောက်ထားတာ ဖြစ်ပါတယ်။ ဒါ့အပြင် testing framework ဖြစ်တဲ့ PHPUnit support လည်း ပါဝင်ပါတဲ့အတွက် application ကို စ setup လုပ်ကတည်းက `phpunit.xml` ဖိုင်ကို တစ်ခါတည်း setup လုပ်ပေးထားမှာ ဖြစ်ပါတယ်။ PHPUnit အပြင် Laravel မှာ Symfony ရဲ့ HttpKernel, DomCrawler နှင့် BrowserKit တို့ ပါဝင်တဲ့အတွက် testing လုပ်ရာမှာ application ရဲ့ views တွေကို web browser တစ်ခုကဲ့သို့ simulate လုပ်နိုင်ပြီး စစ်ဆေးပြုပြင်နိုင်မှာဖြစ်ပါတယ်။

ဥပမာ အနေဖြင့် test ဖိုင်တစ်ခုလည်း `app/tests` folder ထဲမှာပါဝင်ပါတယ်။ Laravel appilcation တစ်ခုကို install လုပ်ပြီးပါက `phpunit` command ကို run ယုံဖြင့် application ရဲ့ tests များကို run နိုင်မှာဖြစ်ပါတယ်။


<a name="defining-and-running-tests"></a>
## Defining & Running Tests (Tests များ သတ်မှတ်ခြင်းနှင့် Run ခြင်း)

Test case ကိုဖန်တီးဖို့ `app/tests` folder ထဲမှာ file အသစ်တစ်ခု ပြုလုပ်ပါ။ class ကတော့ `TestCase` class ကို extend ရမှာဖြစ်ပါတယ်။ ထို့နောက်မှာတော့ သင်နှစ်သက်သလို test methods များကို PHPUnit ကိုအသုံးပြုပြီး ဖန်တီးနိုင်ပြီ ဖြစ်ပါတယ်။


#### Test Class ဥပမာ

	class FooTest extends TestCase {

		public function testSomethingIsTrue()
		{
			$this->assertTrue(true);
		}

	}

သင့် application မှ tests များကို terminal မှ `phpunit` command ရိုက်ပြီး run နိုင်ပါတယ်။


> **သတိ:** ကိုယ့်ဟာကို `setUp` method ရေးထားပါက `parent::setUp` ကို ခေါ်ဖို့ သတိရပါ။

<a name="test-environment"></a>
## Test Environment

unit tests များကို run နေစဉ် Laravel က configuration environment ကို `testing` သို့ အလိုအလျောက် ပြောင်းထားမှာဖြစ်ပါတယ်။ ထို့အပြင် Laravel ရဲ့ test environment ထဲမှာ  `session` နှင့် `cache` တို့ရဲ့ configuration files များပါ ပါဝင်မှာဖြစ်ပါတယ်။ ဒီ drivers နှစ်ခုစလုံးကို test environment ထဲမှာ `array` အဖြစ် set ထားမှာဖြစ်ပါတဲ့အတွက် testing လုပ်ပြီးရင်တော့ testing နဲ့ပတ်သက်တဲ့ session သို့မဟုတ် cache data တွေတော့ ပျက်သွားမှာဖြစ်ပါတယ်။ လိုအပ်ရင်လိုအပ်သလို တခြား testing environments တွေကို ဆက်လက်ဖန်တီးလို့လည်း ရပါတယ်။

<a name="calling-routes-from-tests"></a>

## Calling Routes From Tests (Tests များမှ Routes ကိုခေါ်ခြင်း)

#### Test တစ်ခုမှ Route ကိုခေါ်ခြင်း
`call` method ကိုအသုံးပြု၍ route တစ်ခုခုကို test ကနေ အလွယ်တကူ ခေါ်နိုင်ပါတယ်၊

	$response = $this->call('GET', 'user/profile');

	$response = $this->call($method, $uri, $parameters, $files, $server, $content);
	
ထို့နောက် `Illuminate\Http\Response` object ကို စစ်ဆေးနိုင်ပါတယ်။

	$this->assertEquals('Hello World', $response->getContent());

#### Test တစ်ခုမှ Controller ကိုခေါ်ခြင်း

test ကနေ controller ကိုလည်းခေါ်နိုင်ပါတယ်။

	$response = $this->action('GET', 'HomeController@index');

	$response = $this->action('GET', 'UserController@profile', array('user' => 1));
	
ဒီ `getContent` method ဟာ response ကနေ evaluated string contents တွေကို ပြန်ပေးမှာဖြစ်ပါတယ်။ သင့်၏ route မှ `View` return ရင်တော့ `original` property ကို အသုံးပြု၍ access လုပ်နိုင်ပါတယ်၊

	$view = $response->original;

	$this->assertEquals('John', $view['name']);

HTTPS route တစ်ခုကိုခေါ်လိုပါက `callSecure` method ကို အသုံးပြုနိုင်ပါတယ်။

	$response = $this->callSecure('GET', 'foo/bar');

> **သတိ:** testing environment တွေထဲမှာ route filters တွေကို disable ထားပါတယ်။. ပြန်လည် enable ချင်ရင်တော့, test ထဲမှာ `Route::enableFilters()` ထည့်လိုက်ပါ။

### DOM Crawler

Route ကိုခေါ်၍ DOM Crawler ကိုလက်ခံပြီး ရလာတဲ့ content ကိုစစ်ဆေးနိုင်ပါတယ်။ 

	$crawler = $this->client->request('GET', '/');

	$this->assertTrue($this->client->getResponse()->isOk());

	$this->assertCount(1, $crawler->filter('h1:contains("Hello World!")'));

Crawler အသုံးပြုပုံနှင့်ပတ်သက်ပြီး ပိုသိလိုပါက ၎င်းရဲ့[official documentation](http://symfony.com/doc/master/components/dom_crawler.html) ကို ကိုးကားပါ၊

<a name="mocking-facades"></a>
## Mocking Facades (Facades များ အတုပြုလုပ်ခြင်း)

Testing လုပ်နေစဉ် ရံဖန်ရံခါမှ Laravel ၏ static facade call တွေကို အတုပြုလုပ် (mock) လိုတတ်ပါတယ်။ ဥပမာအနေဖြင့် အောက်ပါ controller action ကိုကြည့်ပါ။ 

	public function getIndex()
	{
		Event::fire('foo', array('name' => 'Dayle'));

		return 'All done!';
	}
	
`Event` class သို့ ခေါ်ထားသော call အား  facade မှာရှိတဲ့ `shouldReceive` method ဖြင့် အတုပြုလုပ်နိုင်ပါတယ်။ [Mockery](https://github.com/padraic/mockery) mock instance တစ်ခု ပြန်လည် return မှာ ဖြစ်ပါတယ်။

#### Facade တစ်ခု အတုပြုလုပ်ခြင်း

	public function testGetIndex()
	{
		Event::shouldReceive('fire')->once()->with('foo', array('name' => 'Dayle'));

		$this->call('GET', '/');
	}

> **သတိ:** `Request` facade ကိုတော့ မ mock သင့်ပါဘူး။ အဲဒီအစား pass ချင်တဲ့ input  အား `call` method သို့ pass ပြီး test ကို run ပါ။

<a name="framework-assertions"></a>
## Framework Assertions (Framework စစ်ဆေးခြင်းများ)

Laravel တွင် testing လုပ်ဖို့ အနည်းငယ် ပိုမိုလွယ်ကူသက်သာစေရန် `assert` methods များပါဝင်ပါတယ်။

#### Respones များ HTTP status OK ဖြစ်ကြောင်း စစ်ဆေးခြင်း

	public function testMethod()
	{
		$this->call('GET', '/');

		$this->assertResponseOk();
	}

#### အခြား response statuses များအား စစ်ဆေးခြင်း

	$this->assertResponseStatus(403);

#### responses များ HTTP Redirects များ ဖြစ်ကြောင်း စစ်ဆေးခြင်း

	$this->assertRedirectedTo('foo');

	$this->assertRedirectedToRoute('route.name');

	$this->assertRedirectedToAction('Controller@method');

#### View တွင် data ရှိကြောင်း စစ်ဆေးခြင်း

	public function testMethod()
	{
		$this->call('GET', '/');

		$this->assertViewHas('name');
		$this->assertViewHas('age', $value);
	}

#### Session တွင် data ရှိကြောင်း စစ်ဆေးခြင်း

	public function testMethod()
	{
		$this->call('GET', '/');

		$this->assertSessionHas('name');
		$this->assertSessionHas('age', $value);
	}

#### Session တွင် Errors များ စစ်ဆေးခြင်း

    public function testMethod()
    {
        $this->call('GET', '/');

        $this->assertSessionHasErrors();

        // Asserting the session has errors for a given key...
        $this->assertSessionHasErrors('name');

        // Asserting the session has errors for several keys...
        $this->assertSessionHasErrors(array('name', 'age'));
    }

#### Input အဟောင်းများ Data စစ်ဆေးခြင်း

	public function testMethod()
	{
		$this->call('GET', '/');

		$this->assertHasOldInput();
	}

<a name="helper-methods"></a>
## Helper Methods (အထောက်အကူ Methods များ)

Application test လုပ်ရာတွင် ပိုမိုလွယ်ကူစေရန် `TestCase` class တွင် helper methods များပါဝင်ပါတယ်။

#### Tests မှ Sessisons data များ ဖန်တီ ခြင်း flush ခြင်း

	$this->session(['foo' => 'bar']);

	$this->flushSession();

#### လက်ရှိ authenticated ဖြစ်ပြီးသော User တစ်ယောက်ဖန်တီးခြင်း

`be` method အား အသုံးပြု၍ လက်ရှိ authenticated ဖြစ်ပြီးသော user တစ်ယောက်ဖန်တီးနိုင်ပါတယ်။

	$user = new User(array('name' => 'John'));

	$this->be($user);

Database အား `seed` method အသုံးပြု၍ re-seed ပြုလုပ်နိုင်ပါတယ်။

#### Test မှ Database အား Re-seed ပြုလုပ်ခြင်း

	$this->seed();

	$this->seed($connection);

Database seeds များပြုလုပ်ခြင်းနှင့် ပတ်သက်၍ documentation ရဲ့ [migrations and seeding](migrations#database-seeding.md) အခန်းမှာ သွားကြည့်နိုင်ပါတယ်။


<a name="refreshing-the-application"></a>
## Application အား refresh ပြုလုပ်ခြင်း

သင်၏ Laravel `Application/IoC Container` အား `$this->app` မှတစ်ဆင့် မည်သည့် test method မှမဆို access နိုင်ပါတယ်။ ဒီ Application instance ဟာ test case တစ်ခုစီ အတွက် ပြန်လည် refresh သွားမှာဖြစ်ပါတယ်။ Application အား သင် သတ်မှတ်ထားသော method တစ်ခုအတွက်သာ refresh ပြုလုပ်ချင်ပါက test method မှ `refreshApplication` method ကို အသုံးပြုနိုင်ပါတယ်။ ဒါဟာ test cases များ စ run ကတည်းက IoC container ထဲမှာရှိတေသာ အပို bindings များ၊ အတုပြုလုပ်ခြင်း (mocks) များအား reset ပြုလုပ်သွားမှာ ဖြစ်ပါတယ်။