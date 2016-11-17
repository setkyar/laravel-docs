# Pagination

- [ပြင်ဆင်ခြင်း](#configuration)
- [အသုံးအနှုန်း](#usage)
- [Appending To Pagination Links](#appending-to-pagination-links)
- [Converting To JSON](#converting-to-json)
- [Custom Presenters](#custom-presenters)

<a name="configuration"></a>
## ပြင်ဆင်ခြင်း

Frameworks တော်တော်များများအတွက်တော့ pagination ပြုလုပ်ဖို့အတွက် စိတ်ပျက်စရာ ကိစ္စတွေ ကြုံတွေ့နိုင်ပါတယ်။ Laravel ကတော့ ဒီကိစ္စကို လွယ်လွယ်ကူကူ ပဲပြုလုပ်နိုင်အောင် အဆင်သင့် ပြင်ဆင်ထားပေးပါတယ်။`app/config/view.php` ဖိုင်ထဲမှာ pagination အတွက် option တစ်ခု ပါရှိပါတယ်။ အဲ့ဒီ `pagination` option မှာ pagination links တွေအတွက် ဘယ် view ကိုအသုံးပြုသင့်တယ်ဆိုတာကို သတ်မှတ်ပေးထားနိုင်ပါတယ်။ ပုံမှန်အတိုင်းဆိုရင်တော့ Laravel မှာ pagination အတွက် view နှစ်ခုကို သတ်မှတ်ပေးထားပါတယ်။ 

`pagination::slider` ကိုအသုံးပြုမယ်ဆိုရင် လက်ရှိ view မှာဖော်ပြထားတဲ့ items အရေအတွက်ကိုအခြေခံပီးတော့ links တွေထုတ်ပေးပါတယ်။ `pagination::simple` view ကတော့ "previous" နဲ့ "next" button နှစ်ခုထုတ်ပေးပါတယ်။ အဲ့ဒီ view နှစ်ခုစလုံးဟာ Twitter Bootstrap နဲ့ အဆင်ပြေပြေတွဲဖက်အသုံးပြုနိုင်ပါတယ်။ 

<a name="usage"></a>
## အသုံးအနှုန်း

အချက်အလက်တွေကို paginate လုပ်လုပ်ဖို့အတွက် နည်းနည်းတွေ အများကြီးရှိပါတယ်။ အဲ့ဒီအထဲကမှ `paginate` method ကို Laravel ရဲ့ Query Builder သို့မဟုတ် Eloquent Model တွေနဲ့တဲသုံးတဲ့နည်းကတော့ အရိုးရှင်းဆုံးနည်းလမ်းဖြစ်ပါတယ်။ 

#### Paginating Database Results

	$users = DB::table('users')->paginate(15);

#### Paginating An Eloquent Model

[Eloquent](eloquent.md) models တွေကိုလည်း paginate လုပ်နိုင်ပါတယ် -

	$allUsers = User::paginate(15);

	$someUsers = User::where('votes', '>', 100)->paginate(15);

`paginate` method ကို passing ပေးလိုက်တဲ့ argument(number) ဟာ စာမျက်နှာတစ်ခုပေါ်မှာ အချက်အလက် ဘယ်လောက်ပေါ်မယ်ဆိုတဲ့ အရေအတွက်ဖြစ်ပါတယ်။ Pagination links တွေကို view မှာပြန်ပြဖို့အတွက်တော့ `links` method ကိုအသုံးပြုနိုင်ပါတယ်။ 

	<div class="container">
		<?php foreach ($users as $user): ?>
			<?php echo $user->name; ?>
		<?php endforeach; ?>
	</div>

	<?php echo $users->links(); ?>

လက်ရှိ စာမျက်နှာနဲ့ပတ်သက်ပြီး framework ကို ဘာပြင်ဆင်မှုမှ မလုပ်ခဲ့တာကို သတိပြုမိမှာပါ။ အဲ့ဒီအတွက် laravel က အလိုအလျောက်ဆုံးဖြတ်ပေးပါတယ်။ 

Pagination အတွက် custom view ကိုအသုံးပြုချင်ရင်တော့ `links` method ထဲမှာ view ကို passing ပေးလိုက်ရုံပါပဲ။

	<?php echo $users->links('view.name'); ?>

Pagination information တွေကိုလဲ အောက်ပါ methods တွေကိုအသုံးပြုပြီး ရယူနိုင်ပါတယ်။ 

- `getCurrentPage`
- `getLastPage`
- `getPerPage`
- `getTotal`
- `getFrom`
- `getTo`
- `count`


#### "Simple Pagination"

အကယ်၍ pagination view မှာ "next" နဲ့ "previous" links တွေကိုပဲပြချင်ရင်တော့ ပိုပြီးအဆင်ပြေတဲ့ query ကိုပြုလုပ်ပေးနိုင်တဲ့ `simplePaginate` method ကိုအသုံးပြုနိုင်ပါတယ်။ view မှာ page numbers တွေအတိအကျဖော်ပြစရာမလိုတဲ့အတွက် data တွေအများကြီးကို paginate လုပ်ရာမှာ ပိုမို အဆင်ပြေစေပါတယ်။ 

	$someUsers = User::where('votes', '>', 100)->simplePaginate(15);

#### Creating A Paginator Manually

အကယ်၍ pagination ကို manually ပြုလုပ်ချင်ရင် `Paginator::make` method ကိုအသုံးပြုနိုင်ပါတယ်။ 

	$paginator = Paginator::make($items, $totalItems, $perPage);

#### Customizing The Paginator URI

Paginator ကအသုံးပြုတဲ့ URI ကိုလဲ `setBaseUrl` method ကိုအသုံးပြုပြီး ပြင်ဆင်နိုင်ပါတယ်။ 

	$users = User::paginate();

	$users->setBaseUrl('custom/url');

အပေါ်မှာပြထားတဲ့ ဥပမာအရဆိုရင် pagination URLs ဟာ http://example.com/custom/url?page=2 ပုံစံဖြစ်သွားမှာပါ။

<a name="appending-to-pagination-links"></a>
## Appending To Pagination Links

သင့်အနေနဲ့ `appends` method ကိုအသုံးပြုပြီး query string တွေကို pagination links တွေဆီကို ထပ်ပေါင်းထည့်လို့ရပါတယ်။

	<?php echo $users->appends(array('sort' => 'votes'))->links(); ?>

အပေါ်မှာရေးထားတဲ့ အတိုင်းဆိုရင် URLs ဟာ အောက်ပါပုံစံနဲ့ထွက်လာမှာပါ။

	http://example.com/something?page=2&sort=votes

Paginator's URLs မှာ "hash fragment" ထပ်ပေါင်းထည့်ချင်ရင်တော့ `fragment` method ကိုအသုံးပြုနိုင်ပါတယ်။ 

	<?php echo $users->fragment('foo')->links(); ?>

အပေါ်က mehtod call ဟာ URLs ကို အောက်ပါအတိုင်းထုတ်ပေးပါလိမ့်မယ်။

	http://example.com/something?page=2#foo

<a name="converting-to-json"></a>
## Converting To JSON

The `Paginator` class implements the `Illuminate\Support\Contracts\JsonableInterface` contract and exposes the `toJson` method. You can may also convert a `Paginator` instance to JSON by returning it from a route. The JSON'd form of the instance will include some "meta" information such as `total`, `current_page`, `last_page`, `from`, and `to`. The instance's data will be available via the `data` key in the JSON array.

<a name="custom-presenters"></a>
## Custom Presenters

Pagination ရဲ့ UI style ဟာ default အနေအထားမှာ Bootstrap Frontend Framework က pagination ပုံစံအတိုင်းပြုလုပ်ပေးထားပါတယ်။ သင့်အနေနဲ့ customize presenter နဲ့ အသုံးပြုချင်တယ်ဆိုရင်လဲ အသုံးပြုလို့ရနိုင်ပါတယ်။ 

### Extending The Abstract Presenter

`Illuminate\Pagination\Presenter` class ကို extend လုပ်ပြီး အဲ့ဒီ class ရဲ့ abstract methods တွေကို implement ပြုလုပ်ပြီးပြောငး်လဲ အသုံးပြုနိုင်ပါတယ်။ အောက်မှာ ပြထားတဲ့ ဥပမာကတော့ Zurb Foundation ရဲ့ ပုံစံကိုပြောင်းလဲ အသုံးပြုထားတာဖြစ်ပါတယ်။ 

    class ZurbPresenter extends Illuminate\Pagination\Presenter {

        public function getActivePageWrapper($text)
        {
            return '<li class="current"><a href="">'.$text.'</a></li>';
        }

        public function getDisabledTextWrapper($text)
        {
            return '<li class="unavailable">'.$text.'</li>';
        }

        public function getPageLinkWrapper($url, $page)
        {
            return '<li><a href="'.$url.'">'.$page.'</a></li>';
        }

    }

### Using The Custom Presenter

ပထမဦးဆုံး custom presenter ပြုလုပ်လို့တဲ့ view ဖိုင်ကို `app/views` အောက်မှာ ပြုလုပ်ပေးလိုက်ပါ။ ပြီးရင် `app/config/view.php` အောက်မှာရှိတဲ့ `pagination` `pagination::slider-3` နေရာမှာ အသစ်လုပ်ထားတဲ့ view file ရဲ့ name နဲ့အစားထိုးလိုက်ပါ။ အပေါ်မှာပြထားတဲ့ Zurb Foundation အတိုင်းဆိုရင် သင့်ရဲ့ view ဖိုင်အသစ်ဟာ အောက်ပါ ပုံစံအတိုင်းဖြစ်ရမှာပါ။ 

    <ul class="pagination">
        <?php echo with(new ZurbPresenter($paginator))->render(); ?>
    </ul>
