# Localization

- [မိတ်ဆက်](#introduction)
- [Language Files](#language-files)
- [အခြေခံအသုံးပြုခြင်း](#basic-usage)
- [အများကိန်းပြုခြင်း](#pluralization)
- [Validation Localization](#validation)
- [Overriding Package Language Files](#overriding-package-language-files)

<a name="introduction"></a>
## မိတ်ဆက်

Laravel မှာပါတဲ့ `Lang` class ဟာ languages ဖိုင်တွေထဲမှာသတ်မှတ်ထားတဲ့ စကားစုတွေကို လွယ်ကူ အဆင်ပြေသော နည်းလမ်းတွေနဲ့ လက်ခံဆောင်ရွက်ပေးနိုင်ပါတယ်။ သင့် application အတွက် ဘာသာစကားမျိုးစုံကို လွယ်ကူစွာ အသုံးပြုနိုင်အောင်အထောက်အပံ့ပေးထားပါတယ်။ 

<a name="language-files"></a>
## Language Files

`app/lang` လမ်းကြောင်းအောက်မှာ ဘာသာစကား စကားစုတွေကို သိမ်းဆည်းပါတယ်။ အဲ့ဒီလမ်းကြောင်းအောက်မှာတော့ သတ်မှတ်ချင်တဲ့ ဘာသာစကားတစ်ခုချင်းစီအတွက် ဖိုဒါတစ်ခုချင်းစီ ဆောက်ပြီးအသုံးပြုရမှာပါ။

	/app
		/lang
			/en
				messages.php
			/mm
				messages.php

#### Example Language File

ဘာသာစကားသတ်မှတ်ထားတဲ့ ဖိုင်ဆီကနေ keyed strings တွေပါတဲ့ array return ပြန်လာပါတယ်။ ဥပမာ -

	<?php

	return array(
		'welcome' => 'Welcome to our application'
	);

#### Changing The Default Language At Runtime

Application ရဲ့ ပုံမှန် ဘာသာစကားကိုတော့ `app/config/app.php` configuration ဖိုင်ထဲမှာ သတ်မှတ်ထားပါတယ်။ ဘာသာစကားများ တစ်ခုနဲ့တစ်ခု ပြောင်းလဲ အသုံးပြုချင်ရင်တော့ `App::setLocale` method ကိုအသုံးပြုနိုင်ပါတယ်။ 

	App::setLocale('mm');

#### Setting The Fallback Language

"fallback language" အတွက်လည်း ပြင်ဆင်ထားနိုင်ပါတယ်။ "fallback language" ဆိုတာကတော့ လက်ရှိ သတ်မှတ်ထားတဲ့ ဘာသာစကား (language) ဖိုင်မှာ လိုအပ်နေတဲ့ စကားစု (language line) မပါလာတဲ့ အခြေအနေမျိုးမှာ အသုံးပြုဖို့အတွက်ဖြစ်ပါတယ်။ ပုံမှန်သတ်မှတ်နေကျအတိုင်းပဲ "fallback language" ကို `app/config/app.php` configuration ဖိုင်ထဲမှာသတ်မှတ်နိုင်ပါတယ်။ 

	'fallback_locale' => 'en',

<a name="basic-usage"></a>
## အခြေခံအသုံးပြုနည်း

#### ဘာသာစကားသတ်မှတ်ထားသော ဖိုင်မှ စကားစုများ ရယူခြင်း

	echo Lang::get('messages.welcome');

`get`method ထဲကို passed လုပ်ထားတဲ့ string နှစ်ခုထဲမှ ပထမတစ်ခုကတော့ ဘာသာစကား (language) သတ်မှတ်ထားတဲ့ ဖိုင်ရဲ့ အမည်ဖြစ်ပြီး၊ ဒုတိယ တစ်ခုကတော့ array ထဲမှာသတ်မှတ်ထား စကားစုတွေရဲ့ key ဖြစ်ပါတယ်။ 

> **သတိပြုရန်**: အကယ်၍ `get` နဲ့ ယူထားတဲ့ key အတွက် စကားစုဟာ ရှိမနေဘူးဆိုရင်တော့ key တစ်ခုပဲ return ပြန်လာပါလိမ့်မယ်။

`trans` ဆိုတဲ့ helper function ကိုလည်း အသုံးပြုနိုင်ပါတယ်။ အဲ့ဒီ function ကတော့ `Lang::get` ဆိုတဲ့ method ကိုပဲ နာမည်ပြောင်းပြီးထပ်လုပ်ထားတာပါ။ 

	echo trans('messages.welcome');

#### စကားစုများ အစားထိုး ပြုလုပ်ခြင်း

စကားစုတွေမှာ အစားထိုးဖို့ စကားလုံးတွေအတွက် place-holders လဲသတ်မှတ်နိုင်ပါသေးတယ်။

	'welcome' => 'Welcome, :name',

ပြီးရင်တော့ `Lang::get` method ရဲ့ ဒုတိယ argument မှာ အစားထိုးချင်တဲ့ စကားလုံးကို passing ပေးလိုက်ပါ။ 

	echo Lang::get('messages.welcome', array('name' => 'Dayle'));

#### Determine If A Language File Contains A Line

	if (Lang::has('messages.welcome'))
	{
		//
	}

<a name="pluralization"></a>
## အများကိန်းပြုလုပ်ခြင်း

အများကိန်းပြုလုပ်ခြင်းကိစ္စ ဟာ နည်းနည်းတော့ ရှုပ်ထွေးပါတယ်။ မတူညီတဲ့ languages တွေအတွက် မတူညီတဲ့ အများကိန်းပြုလုပ်နည်းတွေ ရှိပါတယ်။ Laravel မှာတော့ အများကိန်းပြုလုပ်ဖို့အတွက် "pipe" character ကို အနည်းကိန်းအတွက် ပြုလုပ်ထားတဲ့ စကားစုနဲ့ အများကိန်းအတွက်သတ်မှတ်မဲ့ စကားစုကြားမှာ ခံပြီးအသုံးပြုနိုင်ပါတယ်။ အများကိန်းပြုလုပ်တာကိုနားလည်ဖို့အတွက် အောက်ပါ ဥပမာကိုကြည့်ပါ။ 

	'apples' => 'There is one apple|There are many apples',

စကားစုတွေကို ယူသုံးဖို့အတွက်တော့ `Lang::choice` mehtod ကိုအသုံးပြုနိုင်ပါတယ်။

	echo Lang::choice('messages.apples', 10);

Local အတွက်သတ်မှတ်ထားတဲ့ စကားလုံးကိုလဲ သတ်မှတ်ပေးလိုက်နိုင်ပါတယ်။ ဥပမာ - Russian (ru) language ကိုအသုံးပြုချင်တယ်ဆိုရင် -

	echo Lang::choice('товар|товара|товаров', $count, array(), 'ru');

Laravel translator ဟာ Symfony Translation component ကိုအသုံးပြုထားတဲ့အတွက်ကြောင့် သင့်အနေနဲ့ ပိုပြီး ရှင်းလင်းတိကျတဲ့ အများကိန်းပြုနည်း သတ်မှတ်ချက်ကို ပြုလုပ်နိုင်ပါတယ်။ 

	'apples' => '{0} There are none|[1,19] There are some|[20,Inf] There are many',


<a name="validation"></a>
## Validation

Localization အတွက် အသုံးပြုနိုင်တဲ့ validation errors နဲ့ messages တွေကိုတော့ အသုံးပြုနည်း လမ်းညွှန်ရဲ့<a href="/docs/validation#localization">Validation</a> မှာ ကြည့်နိုင်ပါတယ်။

<a name="overriding-package-language-files"></a>
## Overriding Package Language Files

Laravel နဲ့အတူ တွဲစပ်အသုံးပြုနိုင်တဲ့ packages တွေမှာ သူတို့ရဲ့ ကိုယ်ပိုင် ဘာသာစကားဖိုင်တွေတစ်ပါတည်းပါလာပါတယ်။ အဲ့ဒီဖိုင်တွေကို change ဖို့ packages တွေရဲ့ မူရင်းဖိုင်တွေကို သွားပြင်နေမဲ့အစား `app/lang/packages/{locale}/{package}` လမ်းကြောင်းအောက်ကနေတစ်ဆင့် override ပြုလုပ်နိုင်ပါတယ်။ ဥပမာ `skyrim/hearthfire` လို့ အမည်တွင်တဲ့ package အတွက် `messages.php` ဖိုင်ထဲမှာရှိတဲ့ English Language ကို override လုပ်ချင်တယ်ဆိုရင် `app/lang/packages/en/hearthfire/messages.php` ဖိုင်ကနေတစ်ဆင့် ပြုလုပ်နိုင်ပါတယ်။ Override လုပ်ဖို့လိုအပ်တဲ့ စကားစုတွေကိုပဲ အဲ့ဒီဖိုင်ထဲမှာသတ်မှတ်ထားဖို့လိုအပ်ပါတယ်။ ကျန်တဲ့စကားစုအားလုံးကိုတော့ package ရဲ့ language ဖိုင်ထဲက နေပဲ အလုပ်လုပ်သွားမှာဖြစ်ပါတယ်။ 
