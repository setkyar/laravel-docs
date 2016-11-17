# Laravel Cashier

- [Introduction](#introduction)
- [Configuration](#configuration)
- [Subscribing To A Plan](#subscribing-to-a-plan)
- [No Card Up Front](#no-card-up-front)
- [Swapping Subscriptions](#swapping-subscriptions)
- [Subscription Quantity](#subscription-quantity)
- [Cancelling A Subscription](#cancelling-a-subscription)
- [Resuming A Subscription](#resuming-a-subscription)
- [Checking Subscription Status](#checking-subscription-status)
- [Handling Failed Payments](#handling-failed-payments)
- [Invoices](#invoices)

<a name="introduction"></a>
## Introduction

Laravel Cashier က Subscription Billing Service တစ်ခုဖြစ်တဲ့ Stripe သုံးတဲ့အခါ ပိုပြီးလွယ်ကူစေအောင်လို့ လုပ်ပေးထားပါတယ်။ Stripe သုံးဖို့အတွက် အစအဆုံး ပြန်ရေးနေစရာမလိုတော့အောင်လို့ အခြေခံ Code တွေ ရေးထားပြီးသားဖြစ်ပါတယ်။ အခြေခံ Subscription Management အပြင် , Coupons, Subscription ကို Upgrade လုပ်တဲ့ Feature (Swap), Subscription Quantities, Subscription ကို သတ်မှတ်ထားတဲ့ ကာလအတွင်း Subscription ကို Cancel လုပ်လို့ရမယ့် Feature လည်းပါဝင်ပါတယ်။ နောက် Invoice ကို PDF ထုတ်လို့ရအောင်လဲ ကူညီပေးပါတယ်။

<a name="configuration"></a>
## Configuration

#### Composer

ပထမဆုံး သင့်ရဲ့ Composer File မှာ Casher package ကိုထည့်ပေးပါ၊

	"laravel/cashier": "~2.0"

#### Service Provider

နောက်... သင်ရဲ့ `app` configuration file ထဲမှာ `Laravel\Cashier\CashierServiceProvider` ကို regiter လုပ်ပါ၊

#### Migration

Chashier ကိုမသုံးခင်မှာ... columns တစ်ချို့ကို သင့်ရဲ့ database ထဲကို add လုပ်ဖို့လိုပါမယ်။ မစိုးရိမ်ပါနဲ့... လိုအပ်တဲ့ column တွေကိုထက်ထည့်ဖို့  `cashier:table` Artisan command ကိုသုံးနိုင်ပါတယ်။

#### Model Setup

နောက်... သင့်ရဲ့ model definition မှာ BillableTrait နဲ့ appropriate date mutators တွေကို add လိုက်ပါ:

	use Laravel\Cashier\BillableTrait;
	use Laravel\Cashier\BillableInterface;

	class User extends Eloquent implements BillableInterface {

		use BillableTrait;

		protected $dates = ['trial_ends_at', 'subscription_ends_at'];

	}

#### Stripe Key

နောက်ဆုံးမှာတော့ သင့်ရဲ့ Stripe key ကိုသင့်ရဲ့ bootstrap files တစ်ခုထဲမှာ set လုပ်လိုက်ပါ

	User::setStripeKey('stripe-key');

<a name="subscribing-to-a-plan"></a>
## Subscribing To A Plan

user ကို Stripe plan တစ်ခုပေးဖို့ သင့်မှာ model instance တစ်ခုရှိတယ်ဆိုရင် လွယ်လွယ်ကူကူ subscribe လုပ်နိုင်ပါတယ်
	$user = User::find(1);

	$user->subscription('monthly')->create($creditCardToken);

subscription ကို create လုပ်ပြီးသွားပြီဆိုရင် သင့်အနေနဲ့ cupon ကို apply လုပ်ဖို့ `withCoupon` ကိုသုံးနိုင်ပါတယ်

	$user->subscription('monthly')
	     ->withCoupon('code')
	     ->create($creditCardToken);

Stripe subscription ကို `subscription` method က automatically create လုပ်သွားလိမ့်မယ်... သင့်ရဲ့ Strip customer ID နဲ့ အခြား billing information နဲ့ပတ်သတ်တဲ့ database တွေကော update လုပ်သွားပါလိမ့်မယ်။

သင့်မှာ trail period ရှိတယ်ဆိုရင် သင့်ရဲ့ model မှာ trial end date ကို subscribing လုပ်ပြီးမှာ set လုပ်ထားရဲ့လားဆိုတာကိုသေချာ make sure လုပ်ပါ။

	$user->trial_ends_at = Carbon::now()->addDays(14);

	$user->save();

<a name="no-card-up-front"></a>
## No Card Up Front
သင့်ရဲ့ application က ပထမဆုံး free-trial တစ်ခုကို credit-card မပါဘဲ လက်ခံမယ်ဆိုရင် `cardUpFront` ကိုသင့်ရဲ့ modle မှာ `false` ဆိုပြီး set လုပ်ပါ...

	protected $cardUpFront = false;

Account creation မှာ trial နောက်ဆုံးရက်ကို model မှာ set လုပ်ထားရဲ့လားဆိုတာကို make sure လုပ်ပါ...

	$user->trial_ends_at = Carbon::now()->addDays(14);

	$user->save();

<a name="swapping-subscriptions"></a>
## Swapping Subscriptions

Subscription အသစ်တစ်ခုမှာ user တစ်ယောက် ကို swap လုပ်ချင်တယ်ဆိုရင်  `swap` method ကိုသုံးပါ...

	$user->subscription('premium')->swap();

တကယ်လို့ user က trial မှာဘဲရှိနေတယ် ဆိုရင် trial က ပုံမှန် maintained လုပ်သွားပါ့မယ်။ နောက် subscription အတွက် "quantity" တစ်ခုရှိတယ်ဆိုရင် အဲ့ဒီ့ quantity ကိုလည်း maintain လုပ်သွားပါ့မယ်။

<a name="subscription-quantity"></a>
## Subscription Quantity

တစ်ခါတစ်လေမှာ subscriptions တွေက "quantity" ကနေပြီးတော့ affect ဖြစ်တယ်။ ဥပမာ... သင့်ရဲ့ application က user account တစ်ခုအတွက် တစ်လ ကို $10 charge လုပ်တယ်ဆိုပါတော့။ သင့်ရဲ့ subscription quantity ကို တိုးချင်တာဘဲဖြစ်ဖြစ်၊ လျော့ချင်တာဘဲဖြစ်ဖြစ် လွယ်လွယ်ကူကူ လုပ်ချင်တယ်ဆိုရင် `increment` နဲ့ `decrement` methods ကိုသုံးနိုင်ပါတယ်

	$user = User::find(1);

	$user->subscription()->increment();

	// Add five to the subscription's current quantity...
	$user->subscription()->increment(5)

	$user->subscription->decrement();

	// Subtract five to the subscription's current quantity...
	$user->subscription()->decrement(5)

<a name="cancelling-a-subscription"></a>
## Cancelling A Subscription

Subscription တစ်ခုကို Cancel လုပ်တာ ပန်ခြံထဲမှာ လမ်းလျှောက်ရသလိုပါဘဲ...

	$user->subscription()->cancel();

Subscription တစ်ခု cancel လုပ်သွားတဲ့အချိန်မှာ Casher က  `subscription_ends_at` column ကို သင့်ရဲ့ database မှာ အလိုလို set လုပ်သွားပါ့မယ်။  ဥပမာ၊ customer က March လတစ်ရက်နေ့မှာ subscription ကို Cancel လုပ်သွားတယ် နောက် March 5 ရက်နေ့မှာ subscription end ဖြစ်မယ်လို့ schedule လည်းမရှိဘူးဆိုရင် `subscribed` method က March လ 5 ရက်နေ့အထိ return `true` ပြန်နေမှာပါ။

<a name="resuming-a-subscription"></a>
## Resuming A Subscription

User တစ်ယောက်ကသူရဲ့ subscription ကို cancelled လုပ်သွားတဲ့အချိန်မှာ သင့်အနေနဲ့ သူတို့ resume ပြန်လုပ်ဖို့ဆုတောင်းနေမှာပေါ့၊ ဒါဆိုရင် `resume` method ကိုသုံးလိုက်ပါ:

	$user->subscription('monthly')->resume($creditCardToken);

တကယ်လို့ user က subscription တစ်ခုကို cancels လုပ်လိုက်တယ်၊ နောက် subscription က fully expired မဖြစ်ခင်မှာ user က resume ပြန်လုပ်လိုက်တယ်ဆိုရင် သူတို့က bill တွေကိုချက်ချင်းမဖြတ်ပါဘူး။ သူတို့ရဲ့ subscription တွေကို ရိုးရှင်းစွာပဲ re-activated လုပ်သွားပါတယ် နောက် သူတို့ရဲ့ မူလ    billing cycle အတိုင်း  billed လုပ်ပါလိမ့်မယ်။

<a name="checking-subscription-status"></a>
## Checking Subscription Status

User တစ်ယောက်က သင့်ရဲ့ application ကို subscribed လုပ်သွားတာကို verify လုပ်ရန်အတွက် `subscribed` command: ကိုသုံးပါ-

	if ($user->subscribed())
	{
		//
	}

`subscribed` method က Route filter အတွက် အကောင်းဆုံး အသင့်တော်ဆုံး လုပ်ဆောင်ပေးထားပါတယ်:


	Route::filter('subscribed', function()
	{
		if (Auth::user() && ! Auth::user()->subscribed())
		{
			return Redirect::to('billing');
		}
	});

သင့်အနေနဲ့ user က trial ကာလမှာဟုတ်မဟုတ်ကို `onTrial` method ကိုအသုံးပြုပြီးတော့ ဆုံးဖြတ်ပေးနိုင်ပါတယ်:

	if ($user->onTrial())
	{
		//
	}

သင့်အနေနဲ့ user က active subscriber လား ဒါမှမဟုတ် cancel လုပ်လိုက်ပြီလားဆိုတာကို `cancelled` method ကိုသုံးပြီးတော့စစ်နိုင်ပါတယ်:


	if ($user->cancelled())
	{
		//
	}

သင့်အနေနဲ့ User ကသူ့ရဲ့ subscription ကို cancel လုပ်လိုက်ပြီ ဒါပေမယ့် subscription ကလည်း fully expires မဖြစ်သေးဘူး... တစ်နည်းအားဖြင့် "grace period" လည်းမကုန်သေးဘူးဆိုတာကို ဆုံးဖြတ်နိုင်ပါတယ်။ ဥပမာ၊ user က subscription ကို March လ 5 ရက်နေ့မှာ cancel လုပ်လိုက်တယ်... တကယ်တမ်း scheduled မှာက March လ 10 ရက်နေ့မှပြီးမယ်ဆိုရင် အဲ့ဒီ့ user က "grace period" မှာဘဲရှိသေးပါတယ်။ မှတ်ထားရမှာက `subscribed` method ကအဲ့ဒီ့အချိန်မှာ `true` return ဘဲပြန်နေဦးမှာပါ။

	if ($user->onGracePeriod())
	{
		//
	}

User က သင့် application ရဲ့ plan တစ်ခုကိုအမြဲတမ်း subscribed လုပ်ပြီးပြီလား မလုပ်ရသေးဘူးလားဆိုတာကို `everSubscribed` method နဲ့ စစ်ဆေးနိုင်ပါတယ်:

	if ($user->everSubscribed())
	{
		//
	}

<a name="handling-failed-payments"></a>
## Handling Failed Payments

ကတယ်လို့ customer ရဲ့ credit card expires ဖြစ်နေရင်လား၊ မစိုးရိမ်ပါနဲ့ Cashuer က Webhook controller တစ်ခုပါဝင်ပါတယ်... အဲဒါကဘာလုပ်နိုင်လဲဆိုရင်  customer ရဲ့ subscriotion ကို သင့်အတွက် cancel လုပ်ပေးပါလိမ့်မယ်:

	Route::post('stripe/webhook', 'Laravel\Cashier\WebhookController@handleWebhook');

ဒါဘဲလေ။ Payment Fail ဖြစ်တာတွေ capture လုပ်တာတွေကိုလည်း controller ကဖြေရှင်းပေးပါလိမ့်မယ်။ controller က payment သုံးကြိမ်ကြိုးစားလို့မှမရဘူးဆိုရင် customer subscription ကို cancel လုပ်ပါလိမ့်မယ်။ ဒီဥပမာမှာ `stripe/webbhook` URI က ဥပမာအတွက်ပါ။ သင့်အနေနဲ့အဲ့ဒီ့ URI  ကို Stripe Setting မှာ configure လုပ်ဖို့လိုမှာပါ။

သင်ထက်ပေါင်းထည့်ထားတဲ့ Stripe webhook event ကိုဖြေရှင်းချင်တယ်ဆိုရင် Webhook controller ကို ရိုးရှင်းစွာဘဲ extend လုပ်လိုက်ပါ :

	class WebhookController extends Laravel\Cashier\WebhookController {

		public function handleWebhook()
		{
			// Handle other events...

			// Fallback to failed payment check...
			return parent::handleWebhook();
		}

	}

> **Note:** In addition to updating the subscription information in your database, the Webhook controller will also cancel the subscription via the Stripe API.

<a name="invoices"></a>
## Invoices

သင့်အနေနဲ့ user invoices  ရဲ့ array ကို `invoices` method ကိုသုံးပြီးတော့ လွယ်လွယ်ကူကူ retrieve လုပ်နိုင်ပါတယ်:

	$invoices = $user->invoices();

Customer တွေရဲ့ invoices တွေကို List လုပ်တဲ့အချိန်မှာ သင့်အနေနဲ့ invoice information နဲ့ပတ်သတ်တာတွေကို ပြသဖို့ရာအတွက် ဒီ helper တွေကို သုံးနိုင်ပါတယ်:

	{{ $invoice->id }}

	{{ $invoice->dateString() }}

	{{ $invoice->dollars() }}

Invoice PDF download ကို generate ထုတ်ဖို့ရာအတွက် `downloadInvoice` method ကိုသုံးပါ။  ဟုတ်တယ်...ဒါကတကယ်ကိုလွယ်ပါတယ်:

	return $user->downloadInvoice($invoice->id, [
		'vendor'  => 'Your Company',
		'product' => 'Your Product',
	]);