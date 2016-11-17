# စိစစ်ခြင်း

- [အခြေခံအသုံးပြုပုံ](#basic-usage)
- [Working With Error Messages](#working-with-error-messages)
- [Error Messages & Views](#error-messages-and-views)
- [Available Validation Rules](#available-validation-rules)
- [Conditionally Adding Rules](#conditionally-adding-rules)
- [Custom Error Messages](#custom-error-messages)
- [Custom Validation Rules](#custom-validation-rules)

<a name="basic-usage"></a>
## အခြေခံအသုံးပြုပုံ

Laravel အနေဖြင့် data များကို စိစစ်ရာတွင် ရိုးရှင်း အဆင်ပြေသော နည်းလမ်းများကို အသုံးပြုထားသည်။ error message များကို `Validation` class မှ တဆင့် ထုတ်ယူနိုင်သည်။

#### အခြေခံအသုံးပြုပုံ ဥပမာ

	$validator = Validator::make(
		array('name' => 'Dayle'),
		array('name' => 'required|min:5')
	);

Validation ပြုလုပ်ရာတွင်  `make` method ဟုသည့် method ကို အသုံးပြုပြီး array တွင်းပါရှိမည့် ပထမ argument မှာ data ဖြစ်ပြီး ဒုတိယ argument မှာ ထို data များကို စိစစ်မည့် rule များကို ထည့်သွင်းရမည်။


#### Array ကို အသုံးပြု၍ Rule များ သတ်မှတ်ခြင်း

တခုထက်ပိုသော rule များကို သတ်မှတ်လိုပါက "pipe" character ကိုဖြစ်စေ array အတွင်း ခြား၍ဖြစ်စေ ထည့်သွင်းနိုင်သည်။

	$validator = Validator::make(
		array('name' => 'Dayle'),
		array('name' => array('required', 'min:5'))
	);

#### Fields များစွာကို စိစစ်ခြင်း

    $validator = Validator::make(
        array(
            'name' => 'Dayle',
            'password' => 'lamepassword',
            'email' => 'email@example.com'
        ),
        array(
            'name' => 'required',
            'password' => 'required|min:8',
            'email' => 'required|email|unique:users'
        )
    );

`Validator` instance ကို ပြုလုပ်ပြီးပါက `fails` သို ့မဟုတ် `passes` method ကို အသုံးပြု၍ အချက်အလက်များ စိစစ်နိုင်သည်။


	if ($validator->fails())
	{
		// The given data did not pass validation
	}

စိစစ်ခြင်း မအောင်မြင်ပါက validator မှ error message ကို ရယူနိုင်ပေသည်။

	$messages = $validator->messages();

်fail ဖြစ်သည့် rule များကိုသာ ရယူလိုပြီး message များ မပါဝင်စေလိုပါက `failed` method ကို အသုံးပြုနိုင်သည်။

	$failed = $validator->failed();

#### Files များစိစစ်ခြင်း

`Validator` class အနေဖြင့် `size` နှင့် `mimes` အပါအဝင် များမြောင်လှသော validation method များကို အထောက်အပံ့ပေးထားပြီး file များ validate ပြုလုပ်လိုပါက ထိုထဲ့သို ့ ထည့်သွင်းရန်သာ လိုပေမည်။


<a name="working-with-error-messages"></a>
## Error Messages များနှင့် လှုပ်ရှားခြင်း


After calling the  on a 
`Validator` instance မှ `messages` method ကို ခေါ်ပြီးပါက Error message များဖြင့် အလုပ်လုပ်ရာတွင် လွယ်ကူစေမည့် method များစွာပါဝင်မည့် `MessageBag` ပါဝင်မည် ဖြစ်သည်။

#### Field တစ်ခုမှ ပထမဆုံး Error Message ကို ထုတ်ယူခြင်း

	echo $messages->first('email');

#### Field တစ်ခုမှ Error Message များထုတ်ယူခြင်း

	foreach ($messages->get('email') as $message)
	{
		//
	}

#### Field အားလုံးမှ Error Message များထုတ်ယူခြင်း

	foreach ($messages->all() as $message)
	{
		//
	}

#### Field တစ်ခုမှ message ရှိမရှိ စစ်ဆေးခြင်း

	if ($messages->has('email'))
	{
		//
	}

#### Error Message များအား Format ပြောင်း၍ ထုတ်ယူခြင်း

	echo $messages->first('email', '<p>:message</p>');
	
> **မှတ်ချက်:**  ပုံမှန်အားဖြင့် messages များကို Bootstrap ဖြင့် အဆင်ပြေမည့် ပုံစံများအနေဖြင့် သတ်မှတ်ထားပါသည်။

#### Error Messages များအား Format တစ်ခု သတ်မှတ်၍ ထုတ်ယူခြင်း

	foreach ($messages->all('<li>:message</li>') as $message)
	{
		//
	}

<a name="error-messages-and-views"></a>
## Error Message များနှင့် View များ

Validation ကို ဆောင်ရွက်ပြီးသည်နှင့် Error message များကို လွယ်လင့်တကူ ပြန်လည်ပြသနိုင်ရန် လိုအပ်ပေသည်။ ထိုလိုအပ်ချက်များကို Laravel မှ အဆင်ပြေလွယ်ကူစွာ ဖြည့်စွမ်းထားသည်။ အောက်ပါ route များကို ဥပမာ အနေဖြင့်ကြည့်ပါ။

	Route::get('register', function()
	{
		return View::make('user.register');
	});

	Route::post('register', function()
	{
		$rules = array(...);

		$validator = Validator::make(Input::all(), $rules);

		if ($validator->fails())
		{
			return Redirect::to('register')->withErrors($validator);
		}
	});

	
Note that when စိစစ်ခြင်း မအောင်မြင်ပါက `Validator` instance ကို `withErrors` method ဖြင့် Error များကို passing ပေးလိုက်ပြီး Redirect ပြုလုပ်လိုက်သည် ကို တွေ ့ရပေမည်။ အဆိုပါ method ကို အသုံးပြုခြင်းဖြင့် error message များကို လွယ်လင့်တကူ ဖြတ်ကနဲ  ပြသရာတွင် သုံးနိုင်ရင် next request ၏ Session ထဲတွင် ထည့်သွင်းထားပါသည်။

သို ့ပင်သော်ညား GET route နဲ ့ Error Message များကို အသေချည်နှောင်ထားရန် မလိုသည်ကို သတိပြုရမည်။ အဘယ်ကြောင့်ဆိုသော် Laravel သည် Session data များမှ Error များကို စစ်ဆေးပြီး view ဆီသို ့ အဆင်ပြေသည်နှင့် တပြိုင်နက် ပြသနိုင်ရန် ပြင်ဆင်ထားသည်ကို သတိပြုရမည်။ **ထိုကြောင့် အရေးကြီးသည့် အချက်မှာ`$errors` ဟုသည် variable မှာ သင့် view ၏ request တိုင်းတွင် ပြင်ဆင်ထားသောကြောင့် အမြဲတမ်း အဆင်သင့် ဖြစ်နေသည်ကို မှတ်ထားရန်လိုသည်။ `$errors` variable မှာ `MessageBag` ၏ instance ဖြစ်သည်။

ထိုကြောင့် redirect ပြုလုပ်ပြီးနောက် `$errors` variable နှင့် သင့် view မှာ အလိုအလျောက် ချည်နှောင်ပြီးသား ဖြစ်ပေသည်။

	<?php echo $errors->first('email'); ?>

### အမည်ပေးထားသော Error Bag များ

သင့်အနေဖြင့် Page တစ်ခုတည်းတွင် များပြားလှသော form များသည်ရှိသည် ဆိုပါစို ့။ ထိုအခါ သင့်အနေဖြင့် Error များ၏ `MessageBag` များကို ကွဲပြားခြားနား စေရန် အမည်နာမ ပေးလိုပေမည်။ ထိုအခါတွင် သင့်အနေဖြင့် `withErrors` ဟုသည့် method ၏ ဒုတိယ argument အနေဖြင့် မိမိပေးလိုသည့် အမည်ကို ထည့်သွင်းနိုင်သည်။

	return Redirect::to('register')->withErrors($validator, 'login');

ထိုနောက် သင့်အနေဖြင့် `$errors` variable မှ `MessageBag` instance ကို အောက်ပါအတိုင်း ဆွဲထုတ်နိုင်သည်။

	<?php echo $errors->login->first('email'); ?>

<a name="available-validation-rules"></a>
## အသုံးပြုနိုင်သည့် စိစစ်ခြင်း Rule များ

အောက်တွင် ဖော်ပြထားသည်မှာ အသုံးပြုနိုင်သော စိစစ်ရေး rule များနှင့် ၄င်းတို ့၏ function များဖြစ်ကြသည်။

- [Accepted](#rule-accepted)
- [Active URL](#rule-active-url)
- [After (Date)](#rule-after)
- [Alpha](#rule-alpha)
- [Alpha Dash](#rule-alpha-dash)
- [Alpha Numeric](#rule-alpha-num)
- [Array](#rule-array)
- [Before (Date)](#rule-before)
- [Between](#rule-between)
- [Confirmed](#rule-confirmed)
- [Date](#rule-date)
- [Date Format](#rule-date-format)
- [Different](#rule-different)
- [Digits](#rule-digits)
- [Digits Between](#rule-digits-between)
- [E-Mail](#rule-email)
- [Exists (Database)](#rule-exists)
- [Image (File)](#rule-image)
- [In](#rule-in)
- [Integer](#rule-integer)
- [IP Address](#rule-ip)
- [Max](#rule-max)
- [MIME Types](#rule-mimes)
- [Min](#rule-min)
- [Not In](#rule-not-in)
- [Numeric](#rule-numeric)
- [Regular Expression](#rule-regex)
- [Required](#rule-required)
- [Required If](#rule-required-if)
- [Required With](#rule-required-with)
- [Required With All](#rule-required-with-all)
- [Required Without](#rule-required-without)
- [Required Without All](#rule-required-without-all)
- [Same](#rule-same)
- [Size](#rule-size)
- [Unique (Database)](#rule-unique)
- [URL](#rule-url)

<a name="rule-accepted"></a>
#### accepted

အဆိုပါ field တွင် စိစစ်သည်မှာ  _yes_, _on_, သို ့မဟုတ်  _1_  တို ့ဖြစ်သည်။ "Terms of Service" ကဲ့သို ့ တခုသာ ရွေးမရွေး စိစစ်ရာနေရာများတွင် ၄င်းကို အသုံးပြုနိုင်သည်။


<a name="rule-active-url"></a>
#### active_url

အဆိုပါ field တွင် စိစစ်သည်မှာ `checkdnsrr` ဟုသည် PHP function ကို အသုံးပြု၍ အင်ထုထားသည့် URL ဟုတ်မဟုတ်ကို စစ်ဆေးသွားမည် ဖြစ်သည်။

<a name="rule-after"></a>
#### after:_date_

အဆိုပါ field တွင် စိစစ်သည်မှာ သတ်မှတ်ထားသော date အတွင်းတွင်သာ ထည့်သွင်းစေရန် ဖြစ်သည်။ date များကို  PHP ၏ `strtotime` function ကို အသုံးပြု၍ ပြောင်းလဲကာ စိစစ်သွားမည် ဖြစ်သည်။

<a name="rule-alpha"></a>
#### alpha
အဆိုပါ field တွင် ပါဝင်သော အချက်အလက်များသည် အက္ခရာ များသာ ဖြစ်ရမည် ဖြစ်သည်။ ဥပမာ ကိန်းဂဏန်းများကို လက်ခံသွားမည် မဟုတ်။

<a name="rule-alpha-dash"></a>
#### alpha_dash

အဆိုပါ field တွင် ပါဝင်သော အချက်အလက်များသည် အက္ခရာ နှင့် ကိန်းဂဏန်းများသာ မက dash နှင့် underscore ကိုပါ လက်ခံသွားမည် ဖြစ်သည်။

<a name="rule-alpha-num"></a>
#### alpha_num

အဆိုပါ field တွင် ပါဝင်သော အချက်အလက်များသည် အက္ခရာ နှင့် ကိန်းဂဏန်းများသာ လက်ခံသွားမည်။

<a name="rule-array"></a>
#### array

အဆိုပါ field တွင် ပါဝင်သော အချက်အလက်များသည် array အမျိုးအစားကိုသာ လက်ခံသွားမည်။

<a name="rule-before"></a>
#### before:_date_

အဆိုပါ field တွင်ပါဝင်သော အချက်အလက်များကို date ဖြင့် စိစစ်သတ်မှတ်ခြင်း ဖြစ်သည်။ dates များကို PHP မှ `strtotime` function ကို အသုံးပြု၍ passing ပေးသွားမည် ဖြစ်သည်။

<a name="rule-between"></a>
#### between:_min_,_max_

အဆိုပါ field တွင်ထည့်သွင်းသော အချက်အလက်များ ၏ အများဆုံးနှင့် အနည်းဆုံး တန်ဖိုးများကို သတ်မှတ်ခြင်း ဖြစ်ပြီး String ၊ numeric နှင့် file များကို `size` rule ကို အသုံးပြုသကဲ့သို ့ ဆင်တင်တင်ပင် ဖြစ်သည်။

<a name="rule-confirmed"></a>
#### confirmed

အဆိုပါ field ၏ အချက်အလက်သည် ရည်ညွန်း field ၏ အချက်အလက် ဥပမာ `foo_confirmation`  နှင့် တူညီရမည် ဖြစ်သည်။ ဥပမာ ပြုရသော် `password` field သည် `password_confirmation` field နှင့် ထပ်တူညီရမည် ဖြစ်သည်။

<a name="rule-date"></a>
#### date

တိကျ မှန်ကန်သော date ဖြစ်စေရန် စိစစ်ပေးပြီး `strtotime` ဟူသော PHP function ကို အသုံးပြုထားသည်။

<a name="rule-date-format"></a>
#### date_format:_format_

အဆိုပါ field မှ format နှင့် တူညီရမည် ဖြစ်ပြီး `date_parse_from_format` ဟူသည် PHP function ကို အသုံးပြုထားသည်။

<a name="rule-different"></a>
#### different:_field_

အဆိုပါ field သည် အခြား ရည်ညွန်း field နှင့် လုံးဝ ကွဲပြားခြားရမည် ဖြစ်သည်။
The given _field_ must be different than the field under validation.

<a name="rule-digits"></a>
#### digits:_value_

အဆိုပါ file တွင် numeric value ဖြစ်ပြီး တိကျသေချာသော ဂဏန်း အလုံးအရေအတွက် ကိုသာ ထည့်သွင်းရမည်ဖြစ်သည်။

<a name="rule-digits-between"></a>
#### digits_between:_min_,_max_

အဆို field တွင် _min_ and _max_ အကြား ထည့်သွင်းရသော ဂဏန်းအလုံးအရေအတွက်ကိုသာ ထည့်သွင်းခွင့်ရမည်ဖြစ်သည်။

<a name="rule-email"></a>
#### email

အဆိုပါ field တွင် email address format အတိုင်း ထည့်သွင်းထားခြင်း ရှိမရှိ စစ်ဆေးသွားမည် ဖြစ်သည်။

<a name="rule-exists"></a>
#### exists:_table_,_column_

The field under validation must exist on a given database table.

#### Basic Usage Of Exists Rule

	'state' => 'exists:states'

#### Specifying A Custom Column Name

	'state' => 'exists:states,abbreviation'

You may also specify more conditions that will be added as "where" clauses to the query:

	'email' => 'exists:staff,email,account_id,1'

Passing `NULL` as a "where" clause value will add a check for a `NULL` database value:

	'email' => 'exists:staff,email,deleted_at,NULL'

<a name="rule-image"></a>
#### image

The file under validation must be an image (jpeg, png, bmp, or gif)

<a name="rule-in"></a>
#### in:_foo_,_bar_,...

The field under validation must be included in the given list of values.

<a name="rule-integer"></a>
#### integer

The field under validation must have an integer value.

<a name="rule-ip"></a>
#### ip

The field under validation must be formatted as an IP address.

<a name="rule-max"></a>
#### max:_value_

The field under validation must be less than or equal to a maximum _value_. Strings, numerics, and files are evaluated in the same fashion as the `size` rule.

<a name="rule-mimes"></a>
#### mimes:_foo_,_bar_,...

The file under validation must have a MIME type corresponding to one of the listed extensions.

#### Basic Usage Of MIME Rule

	'photo' => 'mimes:jpeg,bmp,png'

<a name="rule-min"></a>
#### min:_value_

The field under validation must have a minimum _value_. Strings, numerics, and files are evaluated in the same fashion as the `size` rule.

<a name="rule-not-in"></a>
#### not_in:_foo_,_bar_,...

The field under validation must not be included in the given list of values.

<a name="rule-numeric"></a>
#### numeric

The field under validation must have a numeric value.

<a name="rule-regex"></a>
#### regex:_pattern_

The field under validation must match the given regular expression.

**Note:** When using the `regex` pattern, it may be necessary to specify rules in an array instead of using pipe delimiters, especially if the regular expression contains a pipe character.

<a name="rule-required"></a>
#### required

The field under validation must be present in the input data.

<a name="rule-required-if"></a>
#### required\_if:_field_,_value_

The field under validation must be present if the _field_ field is equal to _value_.

<a name="rule-required-with"></a>
#### required_with:_foo_,_bar_,...

The field under validation must be present _only if_ any of the other specified fields are present.

<a name="rule-required-with-all"></a>
#### required_with_all:_foo_,_bar_,...

The field under validation must be present _only if_ all of the other specified fields are present.

<a name="rule-required-without"></a>
#### required_without:_foo_,_bar_,...

The field under validation must be present _only when_ any of the other specified fields are not present.

<a name="rule-required-without-all"></a>
#### required_without_all:_foo_,_bar_,...

The field under validation must be present _only when_ the all of the other specified fields are not present.

<a name="rule-same"></a>
#### same:_field_

The given _field_ must match the field under validation.

<a name="rule-size"></a>
#### size:_value_

The field under validation must have a size matching the given _value_. For string data, _value_ corresponds to the number of characters. For numeric data, _value_ corresponds to a given integer value. For files, _size_ corresponds to the file size in kilobytes.

<a name="rule-unique"></a>
#### unique:_table_,_column_,_except_,_idColumn_

The field under validation must be unique on a given database table. If the `column` option is not specified, the field name will be used.

#### Basic Usage Of Unique Rule

	'email' => 'unique:users'

#### Specifying A Custom Column Name

	'email' => 'unique:users,email_address'

#### Forcing A Unique Rule To Ignore A Given ID

	'email' => 'unique:users,email_address,10'

#### Adding Additional Where Clauses

You may also specify more conditions that will be added as "where" clauses to the query:

	'email' => 'unique:users,email_address,NULL,id,account_id,1'

In the rule above, only rows with an `account_id` of `1` would be included in the unique check.

<a name="rule-url"></a>
#### url

The field under validation must be formatted as an URL.

> **Note:** This function uses PHP's `filter_var` method.

<a name="conditionally-adding-rules"></a>
## Conditionally Adding Rules

In some situations, you may wish to run validation checks against a field **only** if that field is present in the input array. To quickly accomplish this, add the `sometimes` rule to your rule list:

	$v = Validator::make($data, array(
		'email' => 'sometimes|required|email',
	));

In the example above, the `email` field will only be validated if it is present in the `$data` array.

#### Complex Conditional Validation

Sometimes you may wish to require a given field only if another field has a greater value than 100. Or you may need two fields to have a given value only when another field is present. Adding these validation rules doesn't have to be a pain. First, create a `Validator` instance with your _static rules_ that never change:

	$v = Validator::make($data, array(
		'email' => 'required|email',
		'games' => 'required|numeric',
	));

Let's assume our web application is for game collectors. If a game collector registers with our application and they own more than 100 games, we want them to explain why they own so many games. For example, perhaps they run a game re-sell shop, or maybe they just enjoy collecting. To conditionally add this requirement, we can use the `sometimes` method on the `Validator` instance.

	$v->sometimes('reason', 'required|max:500', function($input)
	{
		return $input->games >= 100;
	});

The first argument passed to the `sometimes` method is the name of the field we are conditionally validating. The second argument is the rules we want to add. If the `Closure` passed as the third argument returns `true`, the rules will be added. This method makes it a breeze to build complex conditional validations. You may even add conditional validations for several fields at once:

	$v->sometimes(array('reason', 'cost'), 'required', function($input)
	{
		return $input->games >= 100;
	});

> **Note:** The `$input` parameter passed to your `Closure` will be an instance of `Illuminate\Support\Fluent` and may be used as an object to access your input and files.

<a name="custom-error-messages"></a>
## Custom Error Messages

If needed, you may use custom error messages for validation instead of the defaults. There are several ways to specify custom messages.

#### Passing Custom Messages Into Validator

	$messages = array(
		'required' => 'The :attribute field is required.',
	);

	$validator = Validator::make($input, $rules, $messages);

> *Note:* The `:attribute` place-holder will be replaced by the actual name of the field under validation. You may also utilize other place-holders in validation messages.

#### Other Validation Place-Holders

	$messages = array(
		'same'    => 'The :attribute and :other must match.',
		'size'    => 'The :attribute must be exactly :size.',
		'between' => 'The :attribute must be between :min - :max.',
		'in'      => 'The :attribute must be one of the following types: :values',
	);

#### Specifying A Custom Message For A Given Attribute

Sometimes you may wish to specify a custom error messages only for a specific field:

	$messages = array(
		'email.required' => 'We need to know your e-mail address!',
	);

<a name="localization"></a>
#### Specifying Custom Messages In Language Files

In some cases, you may wish to specify your custom messages in a language file instead of passing them directly to the `Validator`. To do so, add your messages to `custom` array in the `app/lang/xx/validation.php` language file.

	'custom' => array(
		'email' => array(
			'required' => 'We need to know your e-mail address!',
		),
	),

<a name="custom-validation-rules"></a>
## Custom Validation Rules

#### Registering A Custom Validation Rule

Laravel provides a variety of helpful validation rules; however, you may wish to specify some of your own. One method of registering custom validation rules is using the `Validator::extend` method:

	Validator::extend('foo', function($attribute, $value, $parameters)
	{
		return $value == 'foo';
	});

The custom validator Closure receives three arguments: the name of the `$attribute` being validated, the `$value` of the attribute, and an array of `$parameters` passed to the rule.

You may also pass a class and method to the `extend` method instead of a Closure:

	Validator::extend('foo', 'FooValidator@validate');

Note that you will also need to define an error message for your custom rules. You can do so either using an inline custom message array or by adding an entry in the validation language file.

#### Extending The Validator Class

Instead of using Closure callbacks to extend the Validator, you may also extend the Validator class itself. To do so, write a Validator class that extends `Illuminate\Validation\Validator`. You may add validation methods to the class by prefixing them with `validate`:

	<?php

	class CustomValidator extends Illuminate\Validation\Validator {

		public function validateFoo($attribute, $value, $parameters)
		{
			return $value == 'foo';
		}

	}

#### Registering A Custom Validator Resolver

Next, you need to register your custom Validator extension:

	Validator::resolver(function($translator, $data, $rules, $messages)
	{
		return new CustomValidator($translator, $data, $rules, $messages);
	});

When creating a custom validation rule, you may sometimes need to define custom place-holder replacements for error messages. You may do so by creating a custom Validator as described above, and adding a `replaceXXX` function to the validator.

	protected function replaceFoo($message, $attribute, $rule, $parameters)
	{
		return str_replace(':foo', $parameters[0], $message);
	}

If you would like to add a custom message "replacer" without extending the `Validator` class, you may use the `Validator::replacer` method:

	Validator::replacer('rule', function($message, $attribute, $rule, $parameters)
	{
		//
	});