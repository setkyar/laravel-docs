# Artisan Development

- [Introduction](#introduction)
- [Building A Command](#building-a-command)
- [Registering Commands](#registering-commands)
- [Calling Other Commands](#calling-other-commands)

<a name="introduction"></a>
## Introduction

သင့် application အတွက် ကိုယ်ပိုင် commands တွေကို Artisan နဲ့ ထပ်ပြီးပေါင်းထည့်နိုင်ဖို့စီစဉ်ထားပါတယ်။ သင့်ရဲ့ ကိုယ်ပိုင် command တွေကို `app/commands` မှာထက်ထည့်နိုင်ပါတယ်၊ သို့သော်လည်းသင့်ရဲ့ကိုယ်ပိုင် command တွေကို  သင်ကြိုက်တဲ့ storage location မှာ ထည့်နိုင်ပါတယ် သင့်ရဲ့ commands တွေကို သင့်ရဲ့ `composer.json` settings မှာအခြေခံပြီး autoload လုပ်နိုင်ပါတယ်။

<a name="building-a-command"></a>
## Building A Command

### Generating The Class

command တစ်ခုအသစ် create လုပ်ရန်အတွက် - သင့်အနေနဲ့  `command:make` Artisan command ကိုသုံးနိုင်ပါတယ်၊  အဲ့ဒါကသင်စတင်ဖို့ command stub တစ်ခုကို generate ထုတ်ပေးပါလိမ့်မယ်:

#### Generate A New Command Class

	php artisan command:make FooCommand

Default အရ generate လုပ်လိုက်တဲ့ commands တွေက `app/commands` မှာ  သိမ်းဆည်းထားမှာပါ... သို့သော်လည်း သင့်ကိုယ်ပိုင် path ဒါမှမဟုတ် namespace တစ်ခု သတ်မှတ်ထားလို့လည်းရပါတယ်:

	php artisan command:make FooCommand --path=app/classes --namespace=Classes

command create လုပ်တဲ့အချိန်မှာ `--command` option ကို terminal command name အဖြစ် assign လုပ်ရန်အသုံးပြုပါလိမ့်မယ်:

	php artisan command:make AssignUsers --command=users:assign

### Writing The Command

သင့်ရဲ့  command generate လုပ်ပြီးသွားတဲ့အချိန်မှာ သင့်အနေနဲ့ `name` နဲ့ `description` တွေရဲ့ class properties တွေကို ဖြည့်စွတ်သင့်ပါတယ်၊ အဲဒါတွေက သင့်ရဲ့ command တွေကို `list` နဲ့ screen မှာထုတ်ပြတဲ့အချိန်မှာ အသုံးပြုမှာပါ။

သင့် command excute ဖြစ်သွားပြီဆိုရင် `fire` method ကိုခေါ်ပါ့မယ်။ ဒီ method မှာသင်ကြိုက်တဲ့ command logic ကိုထည့်နိုင်တယ်။

### Arguments & Options

The `getArguments` and `getOptions` methods are where you may define any arguments or options your command receives. 

`getArguments` နဲ့ `getOptions` methods တွေကို သင့် command ကနေလက်ခံရရှိတဲ့ မည်သည့် arguments ဒါမှမဟုတ် options မဆို  သတ်မှတ်နိုင်ပါတယ်။ ဒီ methods နှစ်ခုက commands တွေကို array တစ်ခု return ပြန်ပါတယ်၊ အဲ့ဒီ့ array က array options တွေကို list တစ်ခုပုံစံနဲ့ ဖော်ပြထားပါတယ်။

`arguments` တွေကို defining လုပ်တဲ့အချိန်မှာ array definition values တွေကို အောက်မှာပြထားသလိုကိုယ်စားပြုပါတယ် -

	array($name, $mode, $description, $defaultValue)

argument `mode` တွေက `InputArgument::REQUIRED` or `InputArgument::OPTIONAL` တစ်ခုခုဖြစ်လိမ့်မယ်။

`options` တွေကိုသတ်မှတ်တဲ့အချိန်မှာ array definition values တွေကို အောက်မှာပြထားသလိုကိုယ်စားပြုပါတယ် -

	array($name, $shortcut, $mode, $description, $defaultValue)

options အတွက်... argument `mode` က `InputOption::VALUE_REQUIRED`, `InputOption::VALUE_OPTIONAL`, `InputOption::VALUE_IS_ARRAY`, `InputOption::VALUE_NONE` တွေဖြစ်လိမ့်မယ်။

`VALUES_IS_ARRAY` mode ကဘာကိုပြောတာလဲဆိုရင် command ကိုခေါ်တဲ့အချိန်မှာ နှစ်ကြိမ်သုံးလို့ရတယ်ဆိုတာကိုပြတာပါ -

	php artisan foo --option=bar --option=baz

The `VALUE_NONE` option indicates that the option is simply used as a "switch":
`VALUE_NONE` ကဘာကိုပြောတာလဲဆိုရင် သင့်ရဲ့ option ကို "switch" အဖြစ်ရိုးရှင်းစွာ သုံးလို့ရတယ်ဆိုတာကိုပြတာပါ -

	php artisan foo --option

### Retrieving Input

ဘာလို့ သင့်ရဲ့ command က execute ဖြစ်တာလည်း၊ သင်သေချာပေါက် arguments နဲ့ options တွေကို  application က accept လုပ်လိုက် တဲ့ values access လုပ်ဖို့လိုပါမယ် လို့ပါမယ် ဒါကိုလုပ်ဖို့ဆိုရင် သင့်အနေနဲ့ `argument` နဲ့ `option` method  တွေကိုသုံးဖို့လိုပါလိမ့်မယ်။

#### Retrieving The Value Of A Command Argument

	$value = $this->argument('name');

#### Retrieving All Arguments

	$arguments = $this->argument();

#### Retrieving The Value Of A Command Option

	$value = $this->option('name');

#### Retrieving All Options

	$options = $this->option();

### Writing Output

To send output to the console, you may use the `info`, `comment`, `question` and `error` methods. Each of these methods will use the appropriate ANSI colors for their purpose.

Console ဆီကို output send ဖို့ရာအတွက်  သင့်အနေနဲ့  `info`, `comment`, `question` နဲ့ `error` methods တွေကိုအသုံးပြုဖို့လိုပါလိမ့်မယ်။ ဒီ methods တစ်ခုချင်းဆီက သူတို့ရည်ရွယ်ချက်နဲ့သင့်လျော်တဲ့ ANSI colors တွေကို အသုံးပြုပါလိမ့်မယ်။

#### Sending Information To The Console

	$this->info('Display this on the screen');

#### Sending An Error Message To The Console

	$this->error('Something went wrong!');

### Asking Questions

user input prompt အတွက် သင့်အနေနဲ့ `ask` နဲ့ `confirm` methods တွေကို အသုံးပြုနိုင်ပါတယ် -

#### Asking The User For Input

	$name = $this->ask('What is your name?');

#### Asking The User For Secret Input

	$password = $this->secret('What is the password?');

#### Asking The User For Confirmation

	if ($this->confirm('Do you wish to continue? [yes|no]'))
	{
		//
	}

သင့်အနေနဲ့ default value ကိုု `confirm` method အဖြစ် သတ်မှတ်ထားနိုင်ပါတယ်၊ ဒါက `true` or `false` ဖြစ်သင့်ပါတယ်:

	$this->confirm($question, true);

<a name="registering-commands"></a>
## Registering Commands

#### Registering An Artisan Command

သင်ရဲ့ command ကပြီးသွားပြီ ဆိုရင် သင့်အနေနဲ့ Artisan နဲ့ register လုပ်ရပါမယ် ဒါမှ အသုံးပြုလို့ရမှာပါ။ ဒါကိုလည်း ထုံးစံအတိုင်းဘဲ `app/start/artisan.php` file မှာ လုပ်ရမှာပါ။ ဒီ file ထဲမှာ command ကို register လုပ်ဖို့ရာအတွက် `Artisan::add` method ကို အသုံးပြုသင့်ပါတယ် -

	Artisan::add(new CustomCommand);

#### Registering A Command That Is In The IoC Container

သင့် ရဲ့ command က application ရဲ့  [IoC container](ioc.md) ထဲမှာ Register လုပ်ထားတယ်ဆိုရင်... Arisan ကနေခေါ်နိုင်အောင် သင့်အနေဲ့ `Artisan::resolve` method ကို အသုံးပြုရပါ့မယ် - 

	Artisan::resolve('binding.name');

<a name="calling-other-commands"></a>
## Calling Other Commands

တစ်ခါတစ်လေသင့် command ကနေအခြား command တစ်ခုခုကိုခေါ်ချင်မှာပေါ့... ဒါလည်းရပါတယ်  `call` method နဲ့ခေါ်လိုက်ရုံပါဘဲ -

	$this->call('command:name', array('argument' => 'foo', '--option' => 'bar'));