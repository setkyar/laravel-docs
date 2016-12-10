# Localization

- [မိတ်ဆက်](#introduction)
- [Retrieving Language Lines](#retrieving-language-lines)
    - [Replacing Parameters In Language Lines](#replacing-parameters-in-language-lines)
    - [Pluralization](#pluralization)
- [Overriding Package Language Files](#overriding-package-language-files)

<a name="introduction"></a>
## မိတ်ဆက်

သင့်ရဲ့ Application အတွင်းမှာ ဘာသာစကားများစွာ လွယ်လွယ်ကူကူ ထည့်သွင်းလို့ရစေရန်၊ ဘာသာစကားအမျိုးမျိုးဖြင့် ရေးသားထားသော string တွေကို အလွယ်တကူ retrieve လုပ်နိုင်အောင် laravel ရဲ့ localization feature ကထောက်ပံ့ပေးထားပါတယ်။ `resources/lang` လမ်းကြောင်း အောက်မှာရှိတဲ့ file တွေထဲမှာ ဘာသာစကား စကားစုတွေကို သိမ်းဆည်းကြရပါတယ်။ ထိုလမ်းကြောင်းအောက်မှာ ကိုယ့် Application မှာထည့်သွင်းချင်တဲ့ ဘာသာစကားတစ်ခုချင်းစီအတွက် folder အသီးသီးဆောက်ပြီး ဆောက်ပြီး အသုံးပြုရမှာဖြစ်ပါတယ်။

    /resources
        /lang
            /en
                messages.php
            /es
                messages.php

ဘာသာစကားကြိုတင်သတ်မှတ်ထားသော ဖိုင်တွေဆီမှ keyed strings တွေပါတဲ့ array return ပြန်လာပါတယ်။ ဥပမာ -

    <?php

    return [
        'welcome' => 'မင်္ဂလာပါ'
    ];

### ဘာသာစကားညွှန်ကိန်း(locale) များကို အစီအစဉ်ချခြင်း

သင့် application အတွက် ပုံသေ ဘာသာစကားကို `config/app.php` configuration ဖိုင်ထဲမှာ သတ်မှတ်ထားပါတယ်။ ၎င်းကို သင့် application နဲ့ကိုက်ညီအောင် ပြောင်းလဲနိုင်ပါတယ်။ Runtime တွင်လဲ `setLocale` method ကို `App` facade နဲ့တွဲဖက်ပြီး ဘာသာစကားများ ပြောင်းလဲအသုံးပြုနိုင်ပါတယ်။

    Route::get('welcome/{locale}', function ($locale) {
        App::setLocale($locale);

        //
    });

လက်ရှိသတ်မှတ်ထားတဲ့ ဘာသာစကားမှာ မပါဝင်သော စကားစုများအတွက် အသုံးပြုနိုင်ရန် အရံဘာသာစကား (fallback language) ကိုလည်း သတ်မှတ်ပေးဖို့လိုပါလိမ့်မယ်။ "ပုံသေဘာသာစကား" သတ်မှတ်သကဲ့သို့ပင် "အရံဘာသာစကား" ကိုလဲ `config/app.php` configuration ဖိုင်ထဲမှာသတ်မှတ်နိုင်ပါတယ်။

    'fallback_locale' => 'en',

#### Determining The Current Locale

`getLocale` နှင့် `isLocale` method တို့ကို `App` facade တွင်အသုံးပြုပြီး လက်ရှိအသုံးပြုနေသော ဘာသာစကားကို ဆုံးဖြတ်ခြင်း (သို့) ထို ဘာသာစကားညွှန်းကိန်းသည် ကြိုတင်သတ်မှတ်ထားခြင်းရှိမရှိ စစ်ဆေးခြင်းများပြုလုပ်နိုင်ပါတယ်။

    $locale = App::getLocale();

    if (App::isLocale('en')) {
        //
    }

<a name="retrieving-language-lines"></a>
## Retrieving Language Lines

`trans` ဆိုတဲ့ helper function အသုံးပြုပြီးတော့ ဘာသာစကား files တွေမှ စကားစုများထုတ်ယူနိုင်ပါတယ်။ `trans` method က ဘာသာစကားစုတွေရဲ့ ဖိုင်နာမည် နဲ့ key တွေကို သူ့ရဲ့ ပထမ argument အနေနဲ့ လက်ခံပါတယ်။ ဥပမာ `resources/lang/messages.php` ဘာသာစကားဖိုင်အတွင်းမှ `welcome` စကားစုကို ရယူမည်ဆိုပါစို့ -

    echo trans('messages.welcome');

သင္က [Blade templating engine](/docs/{{version}}/blade) ကိုသံုးေနတာဆိုရင္ ဘာသာစကားစုေတြကို ေဖာ္ျပရန္(echo) အတြက္  `{{ }}` syntax သို႔မဟုတ္ `@lang` directive ကိုအသံုးျပဳရပါမယ္။

    {{ trans('messages.welcome') }}

    @lang('messages.welcome')

အကယ္လို႔ သတ္မွတ္လိုက္တဲ့ ဘာသာစကားစု မရွိခဲ့လ်ွင္ `trans` function က ဘာသာစကားစု၏ key ကိုပဲ return ျပန္ေပးပါတယ္။ ထို႔ေၾကာင့္ အထက္ကေဖာ္ျပထားေသာ ဥပမာတြင္ ဘာသာစကားစု မရွိခဲ့လ်ွင္ `trans` function က `messages.welcome` ကို return ျပန္ေပးပါလိမ့္မယ္။

<a name="replacing-parameters-in-language-lines"></a>
### Replacing Parameters In Language Lines

သင္ဆႏၵရွိလွ်င္ သင့္ရဲ႕ စကားစုေတြအတြင္းမွာ place-holder ေတြ သတ္မွတ္နုိုင္ပါတယ္။ place-holder အားလံုးက ေရွ႕မွာ `:` ခံထားပါတယ္။ ဥပမာအားျဖင့္ သင့္ရဲ႕ welcome message ကို place-holder name ထည့္သြင္းျပီး သတ္မွတ္မည္ဆိုလ်ွင္ - 

    'welcome' => 'Welcome, :name',

စကားစုေတြကိုထုတ္ယူေသာအခါ place-holder မ်ားေနရာတြင္အစားထိုးရန္အတြက္ `trans` function ၏ ဒုတိယ argument အေနနဲ႕ အစားထိုးခ်င္သည့္ array ကိုထည့္ေပးလိုက္ရပါမယ္။

    echo trans('messages.welcome', ['name' => 'dayle']);

အစားထိုး၀င္ေရာက္လာေသာတန္ဖိုးမ်ားသည္ place-holder ရဲ႕ စကားလံုး အၾကီးအေသး ထားသိုပံုအလုိက္ ေျပာင္းလဲသြားမွာျဖစ္ပါတယ္။

    'welcome' => 'Welcome, :NAME', // Welcome, DAYLE
    'goodbye' => 'Goodbye, :Name', // Goodbye, Dayle


<a name="pluralization"></a>
### Pluralization

Pluralization is a complex problem, as different languages have a variety of complex rules for pluralization. By using a "pipe" character, you may distinguish singular and plural forms of a string:

    'apples' => 'There is one apple|There are many apples',

After defining a language line that has pluralization options, you may use the `trans_choice` function to retrieve the line for a given "count". In this example, since the count is greater than one, the plural form of the language line is returned:

    echo trans_choice('messages.apples', 10);

Since the Laravel translator is powered by the Symfony Translation component, you may create even more complex pluralization rules which specify language lines for multiple number ranges:

    'apples' => '{0} There are none|[1,19] There are some|[20,Inf] There are many',

<a name="overriding-package-language-files"></a>
## Overriding Package Language Files

Some packages may ship with their own language files. Instead of changing the package's core files to tweak these lines, you may override them by placing files in the `resources/lang/vendor/{package}/{locale}` directory.

So, for example, if you need to override the English language lines in `messages.php` for a package named `skyrim/hearthfire`, you should place a language file at: `resources/lang/vendor/hearthfire/en/messages.php`. Within this file, you should only define the language lines you wish to override. Any language lines you don't override will still be loaded from the package's original language files.
