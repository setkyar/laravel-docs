# Templates

- [Controller Layouts](#controller-layouts)
- [Blade Templating](#blade-templating)
- [Other Blade Control Structures](#other-blade-control-structures)
- [Extending Blade](#extending-blade)

<a name="controller-layouts"></a>
## Controller Layouts

Laravel မှာအသုံးပြုသော templates ပုံစံများထဲကတစ်ခုကတော့ controller layouts ကနေအသုံးပြုတဲ့ပုံစံဖြစ်ပါတယ်။ `layout` property ကို controller မှာသတ်မှတ်လိုက်တာနဲ့ view ဖိုဒါထဲမှာ ကြိုတင်သတ်မှတ်ပြင်ဆင်ထားတဲ့ view ဖိုင်ကို သင့်အတွက်ယူဆောင်ပေးပါလိမ့်မယ်။ ပြီးရင်တော့ controller ကနေညွှန်ကြားလာတဲ့တဲ့ ညွှန်ကြားချက်တွကို လက်ခံဆောင်ရွက်ပေးမှာဖြစ်ပါတယ်။

#### Controller တွင် Layout ကိုသတ်မှတ်ခြင်း

	class UserController extends BaseController {

		/**
		 * The layout that should be used for responses.
		 */
		protected $layout = 'layouts.master';

		/**
		 * Show the user profile.
		 */
		public function showProfile()
		{
			$this->layout->content = View::make('user.profile');
		}

	}

<a name="blade-templating"></a>
## Blade Templating

Laravel မှာပါတဲ့ template ပုံစံနောက်တစ်ခုဖြစ်တဲ့ Blade ဆိုတာကတော့ ရိုးရှင်းပြီး၊ စွမ်းဆောင်ရည်ပြည့်ဝတဲ့ လုပ်ဆောင်ချက်တွေအများကြီးပါတဲ့ template engine တစ်ခုဖြစ်ပါတယ်။ Blade ရဲ့ပုံစံက ပင်မ _template_ မှာတည်ဆောက်ထားတဲ့ပုံစံကို ထပ်ပွားယူပြီး(_inheritance_) အပြောင်းအလဲလုပ်ချင်တဲ့နေရာတွေထဲကို (_section_) လိုအပ်သလို ပြုပြင်ပြောင်းလဲနိုင်တဲ့ ပုံစံဖြစ်ပါတယ်။ Blade template ကိုအသုံးပြုချင်ရင်တော့ `.blade.php` extension နဲ့အသုံးပြုရမှာပါ။ 

#### Blade ပုံစံသတ်မှတ်ခြင်း

	<!-- Stored in app/views/layouts/master.blade.php -->

	<html>
		<body>
			@section('sidebar')
				This is the master sidebar.
			@show

			<div class="container">
				@yield('content')
			</div>
		</body>
	</html>

#### Blade ပုံစံကို အသုံးပြုခြင်း

	@extends('layouts.master')

	@section('sidebar')
		@parent

		<p>This is appended to the master sidebar.</p>
	@stop

	@section('content')
		<p>This is my body content.</p>
	@stop

အပေါ်မှာပြထားတဲ့ဥပမာမှာ ပင်မ template ပုံစံကို `extend` လုပ်ယူပြီး ပင်မ layout ထဲက section နေရာကို ထပ်ထည့်ထားတာကို သတိပြုပါ။ ပင်မ layout ထဲမှာ ကြိုတင်သတ်မှတ်ထားတဲ့ အချက်အလက်တွေကို chile view ထဲမှာ ထပ်သုံးချင်ရင် `@parent` ဆိုတဲ့ ညွှန်ကြားချက်ကိုအသုံးပြုနိုင်ပါတယ်။ Sidebar နဲ့ footer ကဲ့သို့သော အပိုင်းတွေအတွက် လိုအပ်တဲ့ အချက်အလက်တွေကို ထပ်ထည့်နိုင်တဲ့ လုပ်ဆောင်ချက်တစ်ခုဖြစ်ပါတယ်။ 

တစ်ခါတစ်ရံ `@section` သတ်မှတ်ထား / မထား မသေချာဘူး `@yield` နဲ့ဆွဲယူထားတဲ့ နေရာထဲကိုလဲ default value တစ်ခု ထည့်ချင်တယ်ဆိုရင် ဒုတိယ argument အနေနဲ့ ထည့်ပေးလိုက်ရင် ရပါတယ်။ 

	@yield('section', 'Default Content');

<a name="other-blade-control-structures"></a>
## Blade တွင်အသုံးပြုနိုင်သော အခြား control structures များ

#### အချက်အလက်ထုတ်ပြခြင်း

	Hello, {{{ $name }}}.

	The current UNIX timestamp is {{{ time() }}}.

#### အချက်အလက် ရှိ/မရှိ စစ်ဆေးပြီးမှ ထုတ်ပြခြင်း 

တစ်ခါတစ်ရံမှာ အချက်အလက်တစ်ခုကိုထုတ်ပြချင်သော်လည်း အဲ့ဒီ အချက်အလက်ထည့်ထားတဲ့ variable ကို အသုံးပြုထားခြင်း ရှိ/မရှိ မသေချာတဲ့ အခြေအနေမျိုးမှာ ပုံမှန်ဆိုရင် အောက်ပါအတိုင်း အသုံးပြုကြပါတယ်။

	{{{ isset($name) ? $name : 'Default' }}}

အဲ့ဒီပုံစံကို Blade နဲ့လွယ်လွယ်ကူကူပဲရေးနိုင်ပါတယ်... အောက်မှာရေးထားတဲ့ပုံစံကိုကြည့်လိုက်ပါ။

	{{{ $name or 'Default' }}}

#### တွန့်ကွင်း (Curly Braces) နှင့်အုပ်ထားသော စာသားများအတိုင်း ထုတ်ပြခြင်း

တွန့်ကွင်း (curly braces) အုပ်ထားတဲ့ စာသားများကို ထုတ်ပြဖို့ လိုအပ်လျှင်တော့ blade ပုံစံကို ရှေ့မှာ `@` သင်္ကေတ နဲ့ခံပြီး အသုံးပြုနိုင်ပါတယ်။

	@{{ This will not be processed by Blade }}

အသုံးပြုသူဆီက ဝင်လာမဲ့ အချက်အလက်တွေကို escape သို့မဟုတ် purified လုပ်သင့်ပါတယ်။ အဲ့လိုပြုလုပ်ဖို့အတွက် တွန့်ကွင်းသုံးခု (triple curly brace) ကိုအသုံးပြုနိုင်ပါတယ်။ 

	Hello, {{{ $name }}}.

အကယ်၍ escape မလုပ်ချင်ဘူးဆိုရင်တော့ တွန့်ကွင်း နှစ်ခု (double curly braces) ကိုအသုံးပြုနိုင်ပါတယ်။

	Hello, {{ $name }}.

> **သတိပြုရန်:** Application ကိုအသုံးပြုုသူဆီကလာမဲ့ အချက်အလက်တွေကိုထုတ်ပြတဲ့ကိစ္စကို အထူးဂရုစိုက်ဖို့ လိုအပ်ပါတယ်။ အဲ့ဒါကြောင့် HTML entities တွေကို escape ပြုလုပ်ဖို့အတွက် တွန့်ကွင်းသုံးခု (triple curly brace) ကိုအမြဲတမ်းအသုံးပြုသင့်ပါတယ်။

#### If Statements

	@if (count($records) === 1)
		I have one record!
	@elseif (count($records) > 1)
		I have multiple records!
	@else
		I don't have any records!
	@endif

	@unless (Auth::check())
		You are not signed in.
	@endunless

#### Loops

	@for ($i = 0; $i < 10; $i++)
		The current value is {{ $i }}
	@endfor

	@foreach ($users as $user)
		<p>This is user {{ $user->id }}</p>
	@endforeach

	@while (true)
		<p>I'm looping forever.</p>
	@endwhile

#### Including Sub-Views

	@include('view.name')

Include လုပ်ထားတဲ့ view တွေဆီကိုလဲ အချက်အလက်တွေကို passing လုပ်လို့ရပါတယ်။

	@include('view.name', array('some'=>'data'))

#### Overwriting Sections

ပုံမှန်ဆိုရင် sections ဟာ ယခင်ရှိပီးသား အချက်အလက်တွေနဲ့အတူ နောက်ထပ် ထပ်ထည့်လာတဲ့ အချက်အလက်တွေကို ပေါင်းထည့်လိုက်တာဖြစ်ပါတယ်။ အကယ်၍ ယခင်အချက်အလက်တွေကို ဖျက်ပြစ်ပီး နောက်ထပ် ထပ်ထည့်လိုက်တဲ့ အချက်အလက်ကိုပဲ အသုံးပြုချင်ရင်တော့ `overwrite` ကိုအသုံးပြုနိုင်ပါတယ်။

	@extends('list.item.container')

	@section('list.item.content')
		<p>This is an item of type {{ $item->type }}</p>
	@overwrite

#### Displaying Language Lines

	@lang('language.line')

	@choice('language.line', 1);

#### Comments

	{{-- This comment will not be in the rendered HTML --}}

<a name="extending-blade"></a>
## Extending Blade

Blade ကိုအသုံးပြုပြီး စိတ်ကြိုက် control structure တွေကိုပြုလုပ်နိုင်ပါတယ်။ blade file ကို compile လုပ်ပီးတဲ့အခါ၊ သတ်မှတ်ထားတဲ့ စိတ်ကြိုက် control structure တွေကို view အတွက် အချက်အလက်တွေနဲ့အတူ ခေါ်ယူသုံးစွဲပါတယ်။ ရိုးရှင်းလွယ်ကူတဲ့ `str_replace` လိုကိစ္စတွေတင်မက ပိုပြီးရှုပ်ထွေးတဲ့ ကိစ္စတွေအထိ ကိုင်တွယ်ဖြေရှင်းနိုင်ပါတယ်။

Blade compiler မှာ `createMatcher` နဲ့ `create:lainMatcher` ဆိုပြီး helper methods နှစ်ခု ရှိပါတယ်။ အဲ့ဒီ methods တွေကနေ စိတ်ကြိုက် control structure တွေပြုလုပ်ဖို့ လိုအပ်တဲ့ အရာတွေကိုပြုလုပ်ပေးပါတယ်။ 

`createPlainMatcher` method ကို `@endif` တို့ `@stop` တို့လို arguments တွေမပါတာအတွက် အသုံးပြုပြီး၊ `createMatcher` method ကိုတော့ arguments ပါတာတွေပြုလုပ်ဖို့အတွက် အသုံးပြုပါတယ်။

အောက်ပါ ဥပမာကတော့ `@datatime($var)` ကို ပြုလုပ်ထားတာပါ။ အဲ့ဒီ directive မှာပါတဲ့ `$var` ရဲ့ တန်ဖိုးကို `->format()` အသုံးပြုပြီး အလွယ်တကူ ခေါ်သုံးနိုင်ပါတယ်။ 

	Blade::extend(function($view, $compiler)
	{
		$pattern = $compiler->createMatcher('datetime');

		return preg_replace($pattern, '$1<?php echo $2->format('m/d/Y H:i'); ?>', $view);
	});
