# Eloquent ORM

- [အကြမ်းဖျင်း](#အကြမ်းဖျင်း)
- [အခြေခံအသုုံးပြုပုုံ](#အခြေခံအသုုံးပြုပုုံ)
- [Mass Assignment](#mass-assignment)
- [ထည့်သွင်း ၊ ပြင်ဆင် ၊ ဖျက်ပစ်](#insert-update-delete)
- [Soft Deleting](#soft-deleting)
- [Timestamps](#timestamps)
- [Query Scopes](#query-scopes)
- [Relationships](#relationships)
- [Querying Relations](#querying-relations)
- [Eager Loading](#eager-loading)
- [ဆက်စပ်နေသည့် Model များတွင် data ထည့်သွင်းခြင်း](#inserting-related-models)
- [Touching Parent Timestamps](#touching-parent-timestamps)
- [Working With Pivot Tables](#working-with-pivot-tables)
- [Collections](#collections)
- [Accessors & Mutators](#accessors-and-mutators)
- [Date Mutators](#date-mutators)
- [Model Events](#model-events)
- [Model Observers](#model-observers)
- [Arrays နှင့် JSON သို ့ပြောင်းလဲခြင်း](#converting-to-arrays-or-json)

<a name="introduction"></a>
## အကြမ်းဖျင်း

Laravel တွင်ပါဝင်သည့် ရုုိးရှင်းပြီး လှပသပ်ရပ်သော Eloquent ORM သည်  သင့် Database ကုုိ ActiveRecord ဖြင့် အခြေခံထား သဖြင့် အလွယ်တကူပင် အသုုံးပြုနုုိင်မည် ဖြစ်သည်။ Database မှ Table တစ်ခုုတုုိင်းကုုိ Model တစ်ခုု အနေဖြင့် သတ်မှတ်ကာ အသုုံးပြုရမည် ဖြစ်သည်။ 

အသုုံးမပြုခင် ပထမဆုုံး အနေဖြင့် `app/config/database.php` သုုိ ့သွားရောက်ကာ ကြိုတင် ပြင်ဆင်ရမည် ဖြစ်သည်။ 

<a name="basic-usage"></a>
## အခြေခံအသုုံးပြုပုုံ

စတင် အသုုံးပြုရန် Eloquent model တစ်ခုုကိုု တည်ဆောက်ရမည် ဖြစ်သည်။ ပုုံမှန်အားဖြင့် Models file များမှာ `app/models` အမည်ရှိသည့် Folder ထဲတွင် တည်ရှိမည် ဖြစ်သော်လည်း အလျဉ်းသင့်သလုုိ ပြင်ဆင်နုုိင်မည်ဖြစ်သည်။ ထုုိသုုိ ့ပြင်ဆင်နုုိင်ရန် အတွက် `composer.json` ထဲတွင် မိမိတုုိ ့  autoload လုုပ်ချင်သည့် file ၏ အမည်နှင့် တည်နေရာကိုု ထည့်သွင်းထားရမည် ဖြစ်သည်။ 


#### Eloquent Model တစ်ခုု Define ပြုလုုပ်ခြင်း

class User extends Eloquent {}

Eloquent Model တွင် မည့်သည့် table ကုုိ အသုုံးပြုမည်ကုုိ မကြေညာ ထားပါက Model အမည်၏ အများကိန်း ကုုိ အသုုံးပြုမည် ဖြစ်သည်။ ဥပမာ User.php ဟုု ကြေညာထားပါက Users table ဟုု အလုုိအလျောက် သတ်မှတ်မည် ဖြစ်သည်။ သုုိ ့မဟုုတ်ပဲ မိမိ စိတ်ကြိုက် အသုုံးပြုလုုိပါက အောက်ပါ အတုုိင်း သတ်မှတ်နုုိင်မည် ဖြစ်သည်။ 

class User extends Eloquent {

protected $table = 'my_users';

}

> **မှတ်ချက်:** Eloquent အနေဖြင့် Primary Key column ဟုု `id` ဟုု အလုုိအလျောက် သတ်မှတ်ဖြစ်သော်လည်း အထက်က ကဲ့သုုိ ့ပင် မိမိစိတ်ကြိုက် column ကုုိ သတ်မှတ်နုုိင်သည်။ ထုုိကဲ့သုုိ ့ Database Connection ကုုိ `connection` ဟုုသည် property ကုုိ အသုုံးပြု ထပ်မံ သတ်မှတ်နုုိင်မည် ဖြစ်သည်။ 

Model ကုုိ သတ်မှတ်ပြီးသည်နှင့် မိမိတုုိ ့ အလုုိရှိသည့် record များကုုိ စတင် တည်ဆောက်ခြင်း ၊ ထုုတ်ယူခြင်းများ ပြုလုုပ်နုုိင်ပြီ ဖြစ်သည်။  သတိပြုရမည့် တစ်ချက်မှာ `updated_at` နှင့် `created_at` ဆုုိသည့် columns များမှာ သင့် table တွင် အလုုိအလျောက် ပါဝင်မည်ဖြစ်သည်။ သင့် အနေဖြင့် အလုုိမရှိပါက model အတွင်းရှိ `$timestamps`ဟုုသည် property ကုုိ `false` ဟုု ပေးထားရန် လုုိပေမည်။

#### Model အတွင်းမှ record အားလုုံး ထုုတ်ယူခြင်း

$users = User::all();

#### Model အတွင်း record များအား primary key ကုုိ အသုုံးပြုကာ ထုုတ်ယူခြင်း

$user = User::find(1);

var_dump($user->name);

> **Note:**  [query builder] တွင် အသုုံးပြုနုုိင်သည့် method များ အားလုုံး (queries.md) Eloquent models တွင်လည်း ဆက်လက် အသုုံးပြုနုုိင်မည် ဖြစ်သည်။ 

#### Model အတွင်း record များအား primary key ကုုိ အသုုံးပြုကာ ထုုတ်ယူပြီး မရှိပါက Exception ဖြင့် ပြသခြင်း

တခါတရံ သင့် အနေဖြင့် Model မှ data များ ရှာမတွေ ့ပါက Exception အနေဖြင့် ပြသချင်မည် ဖြစ်သည်။ ထုုိသုုိ ့ ပြုလုုပ်နိုင်ရန် `App::error` ဟုုသော handler ကုုိ အသုုံးပြုပြီး 404 page ကုုိ လွဲပြောင်းပြသနုုိင်မည် ဖြစ်သည်။

$model = User::findOrFail(1);

$model = User::where('votes', '>', 100)->firstOrFail();

To register the error handler, listen for the `ModelNotFoundException`

use Illuminate\Database\Eloquent\ModelNotFoundException;

App::error(function(ModelNotFoundException $e)
{
return Response::make('Not Found', 404);
});

#### Querying Using Eloquent Models ကုုိ အသုုံးပြုကာ Query များရေးသားခြင်း

$users = User::where('votes', '>', 100)->take(10)->get();

foreach ($users as $user)
{
var_dump($user->name);
}

#### Eloquent ကို ပေါင်းစပ် အသုုံးပြုခြင်း 


သင့်အနေဖြင့်  Query Builder function များဖြင့် Eloquent ကို ပေါင်းစပ် အသုုံးပြုနုုိင်သည်။

$count = User::where('votes', '>', 100)->count();


If you are unable to generate the query you need via the fluent interface, feel free to use `whereRaw`:

$users = User::whereRaw('age > ? and votes = 100', array(25))->get();

#### Results များကို ခွဲထုတ်ခြင်း

သင့်အနေဖြင့် ထောင်ပေါင်းများစွာသော Eloquent records များကို ထုတ်ယူလိုပါက  `chunk` ဟုသော method ကို အသုံးပြု၍ Memory အသုံးပြုမှုကို လျော့ချနိုင်ပါသည်။ 


User::chunk(200, function($users)
{
foreach ($users as $user)
{
//
}
});

ပထမဆုံး argument   အနေဖြင့် မိမိတို ့ ပိုင်းထုတ်လိုသည့် အရေအတွက်ကို ထည့်သွင်း ပေးရမည် ဖြစ်သည်။ ဒုတိယ argument အနေဖြင့် closure ပါဝင်မည်ဖြစ်ပြီး ထိုထဲတွင် ထွက်ပေါ်လာသော record တိုင်းတွင် ဖြစ်ပေါ်စေလိုသည့်  Instruction များ ရေးသားနိုင်သည်။ 

#### Query Connection သတ်မှတ်အသုံးပြုခြင်း

မိမိတို ့ အသုံးပြုလိုသည် Database connection အပေါ်မူတည်၍ `on` method ကို အသုံးပြုကာ ပြောင်းလဲ အသုံးပြုနိုင်ပါသည်။

$user = User::on('connection-name')->find(1);

<a name="mass-assignment"></a>
## အမြောက်အများ ထည့်သွင်းခြင်း


Model အသစ်ကို တည်ဆောက်ပြီးပါက Constructor အနေဖြင့် ပါဝင်မည့် attribute များကို array အနေဖြင့် ထည့်သွင်းနိုင်ပါသည်။  
ထိုကဲ့သို ့ attribute များ ကို Model များမှ တဆင့် အမြောက်အများ ထည့်သွင်းနိုင်ခြင်းသည် အဆင်ပြေသော်လည်း User Input ကို မစစ်မဆေးပဲ အမြောက်အများထည့်သွင်းပါက လုံခြုံရေး ဆိုင်ရာ **ပြဿနာ** များရှိနိုင်ပါသည်။ ထိုကြောင့် default အနေဖြင့် Eloquent Model များအားလုံးတွင် ထိုသို ့
ပြုလုပ်ခြင်းကို ဖွင့်မပေးထားပါ။ သို ့သော် ထို ့သို ့ပြုလုပ်ချင်ပါက `fillable` သို ့မဟုတ် `guarded`  အစရှိသည့် attribute များကို သတ်မှတ်ထားရန် လိုပေမည်။

#### Defining Fillable Attributes On A Model

The `fillable` property specifies which attributes should be mass-assignable. This can be set at the class or instance level.

class User extends Eloquent {

protected $fillable = array('first_name', 'last_name', 'email');

}

အထက်က ဥပမာတွင် array တွင်ထည့်သွင်းထားသည့် attribute များသည် အမြောက်အများ ထည့်သွင်းနိုင်သည်။

#### Model တွင် တားမြစ် attribute များအား သတ်မှတ်ခြင်း


`fillable` ၏ ပြောင်းပြန်မှာ `guarded` ဖြစ်ပြီင်္း phone တစ်လုံး၏  "black-list" ကဲ့သို ့ အလုပ်လုပ်သည်။

class User extends Eloquent {

protected $guarded = array('id', 'password');

}

> **Note:** When using `guarded`, you should still never pass `Input::get()` or any raw array of user controlled input into a `save` or `update` method, as any column that is not guarded may be updated.

#### အမြောက်အများ ထည့်သွင်းခြင်းမှ တားမြစ်ခြင်း

အပေါ်မှ ဥပမာတွင် `id` နှင့် `password` field များမှာ အမြောက်အများ ထည့်သွင်းနိုင်မည် မဟုတ်ပေ။ အခြား attribute များမှာမူ အမြောက်အများ ထည့်သွင်းနိုင်မည် ဖြစ်သည်။ အကယ်၏ field အားလုံးကို တားမြစ်လိုပါက 

protected $guarded = array('*');

<a name="insert-update-delete"></a>
## ထည့်သွင်း ၊ ပြင်ဆင် ၊ ဖျက်ပစ်

Model မှ record အသစ်ကို တည်ဆောက်လိုပါက model instance တစ်ခုကို တည်ဆောက်ပြီး  `save` method ကို အသုံးပြုနိုင်သည်။


#### Model တွင် record များ ထည့်သွင်းခြင်း

$user = new User;

$user->name = 'John';

$user->save();

> **Note:** Typically, your Eloquent models will have auto-incrementing keys. However, if you wish to specify your own keys, set the `incrementing` property on your model to `false`.

You may also use the `create` method to save a new model in a single line. The inserted model instance will be returned to you from the method. However, before doing so, you will need to specify either a `fillable` or `guarded` attribute on the model, as all Eloquent models protect against mass-assignment.

After saving or creating a new model that uses auto-incrementing IDs, you may retrieve the ID by accessing the object's `id` attribute:

$insertedId = $user->id;

#### Setting The Guarded Attributes On The Model

class User extends Eloquent {

protected $guarded = array('id', 'account_id');

}

#### Using The Model Create Method

// Create a new user in the database...
$user = User::create(array('name' => 'John'));

// Retrieve the user by the attributes, or create it if it doesn't exist...
$user = User::firstOrCreate(array('name' => 'John'));

// Retrieve the user by the attributes, or instantiate a new instance...
$user = User::firstOrNew(array('name' => 'John'));

#### Model တစ်ခုအား Update ပြုလုပ်ခြင်း

Model အား update ပြုလုပ်လိုပါက ရှေးဦးစွာ ပြုလုပ်လိုသည် record ကို retrieve ပြုလုပ်ရပါမည်။ ထိုနောက် attribute အား မိမိတို ့ထည့်သွင်းလိုသည့်နှင့်
ပြောင်းလဲ သတ်မှတ်ပြီး `save` method ကို အသုံးပြုကာ update ပြုလုပ်နိုင်ပါသည်။

$user = User::find(1);

$user->email = 'john@foo.com';

$user->save();

#### Model နှင့် ၄င်း၏ Relationship များတွင် save ပြုလုပ်ခြင်း

တခါတရံ  သင့်အနေဖြင့် လက်ရှိ အသုံးပြုနေသည့် model တွင်သာ မက ၄င်းနှင့် relationship ပြုလုပ်ထားသော Model များတွင်ပါ save လုပ်လိုသည့် အခါများရှိပေမည်။ ထိုသို ့ ပြုလုပ်လိုပါက `push` method ကို အသုံးပြုနိုင်သည်။ 

$user->push();


You may also run updates as queries against a set of models:

$affectedRows = User::where('votes', '>', 100)->update(array('status' => 2));

> **Note:** No model events are fired when updating a set of models via the Eloquent query builder.

#### Model မှ record များအား ဖျက်ပစ်ခြင်း

record တစ်ခုအား ဖျက်ပစ်လိုပါက `delete` ဟုသော method ကို အသုံးပြုနိုင်သည်။

$user = User::find(1);

$user->delete();

#### Model မှ record များအား key အလိုက် ဖျက်ပစ်ခြင်း

User::destroy(1);

User::destroy(array(1, 2, 3));

User::destroy(1, 2, 3);

သင့်အနေဖြင့် query လုပ်ပြီးမှလည်း ဖျက်ပစ်နိုင်ပါသည်။

$affectedRows = User::where('votes', '>', 100)->delete();

####  Model ၏ Timestamps ကိုသာ update ပြုလုပ်ခြင်း

Model ၏ Timestamps ကိုသာ update ပြုလုပ်လိုပါက `touch` ကို အသုံးပြုနိုင်သည်။

$user->touch();

<a name="soft-deleting"></a>
## Soft Deleting  

Soft delete ပြုလုပ်ပါက တကယ်ဖျက်ပစ်လိုက်ခြင်း မဟုတ်ပဲ  သင့် record ထဲတွင်  `deleted_at` ဟု timestamp တွင် သင့်ဖျက်ပစ်လိုက်သည့် အချိန်ကို မှတ်သားထားမည် ဖြစ်သည်။ Soft delete ကို ထည့်သွင်းလိုပါက `SoftDeletingTrait` ကိုပါ ထည့်သွင်းရမည် ဖြစ်သည်။

use Illuminate\Database\Eloquent\SoftDeletingTrait;

class User extends Eloquent {

use SoftDeletingTrait;

protected $dates = ['deleted_at'];

}

`deleted_at` ဟုသည့် column ကို သင့်၏ table တွင် ထည့်သွင်းလိုပါက migration တွင် `softDeletes` ဟုသည့် method ကို အသုံးပြုနိုင်ပါသည်။

$table->softDeletes();

ထိုသို ့ပြုလုပ်ပြီး `delete` method ကို ခေါ်ယူပါက အမှန်တကယ် ဖျက်ပစ်မည် မဟုတ်ပဲ `deleted_at` ဟု column တွင် လက်ရှိ timestamp ကိုမှတ်သားထားမည် ဖြစ်သည်။ Model တစ်ခုတွင် soft delete ကို အသုံးပြုထားပါက query ပြုလုပ်သော အခါတိုင်းတွင် deleted record များမှာ ပါဝင်မည် မဟုတ်ပေ။

#### Soft Deleted record များပါ ရောစပ်ထုတ်ယူခြင်း


soft deleted record များပါ ပေါင်းစပ် ထုတ်ယူလိုပါက `withTrashed` ကို query တွင်ထည့်သွင်း အသုံးပြုရမည် ဖြစ်သည်။

$users = User::withTrashed()->where('account_id', 1)->get();

`withTrashed` method ကို relationship ပြုလုပ်ထားသော model တွင်လည်း အသုံးပြုနိုင်ပါသည်။

$user->posts()->withTrashed()->get();

ှSoft deleted ပြုလုပ်ထားသော results များသာ တွေ  ့မြင်လိုပါက `onlyTrashed` ဟုသော method ကို အသုံးပြုနိုင်သည်။

$users = User::onlyTrashed()->where('account_id', 1)->get();

soft deleted ပြုလုပ်ထားသော record များကို restore ပြုလုပ်လိုပါက `restore` method ကို အသုံးပြုနိုင်သည်။

$user->restore();

`restore` method ကို query ပြုလုပ်နေသည့် အတောအတွင်းလည်း အသုံးပြုနိုင်သည်။

User::withTrashed()->where('account_id', 1)->restore();


`withTrashed` ကဲ့သို ့ပင် `restore` method ကိုလည်း relationship များကြားထဲအတွင်း အသုံးပြုနိုင်သည်။

$user->posts()->restore();

Record တစ်ခုကို အမှန်တကယ် database ထဲမှ ဖျက်ပစ်လိုပါက `forceDelete` method ကို အသုံးပြုနိုင်သည်။

$user->forceDelete();

`forceDelete` method မှာလည်း relationship များအကြား အသုံးပြုနိုင်သည်။

$user->posts()->forceDelete();

soft delted ပြုလုပ်ထားခြင်း ဟုတ်မဟုတ် စစ်ဆေးနိုင်ရန်  `trashed` method ကို အသုံးပြုနိုင်သည်။ 

if ($user->trashed())
{
//
}

<a name="timestamps"></a>
## Timestamps

ပုံမှန်အားဖြင့် Eloquent အနေဖြင့် `timestamp` attribute ကိုထည့်သွင်းပေးသည်နှင့် `created_at` and `updated_at` ကို အလိုအလျောက် ကိုင်တွယ်ပေးမည် ဖြစ်သည်။ သင့်အနေနှင့် မလိုချင်ပါက အောက်ပါအတိုင်း ပြောင်းလဲ သတ်မှတ်နိုင်သည်။

#### အလိုအလျောက် Timestamps ပြုလုပ်ခြင်းမှ ဖယ်ရှားခြင်း

class User extends Eloquent {

protected $table = 'users';

public $timestamps = false;

}

#### စိတ်ကြိုက် Timestamp တစ်ခုသတ်မှတ်ခြင်း

Timestamp တစ်ခုကို စိတ်ကြိုက်သတ်မှတ်လိုပါက model အတွင်းရှိ `getDateFormat` ကို အသုံးပြု၍ သတ်မှတ်နိုင်သည်။

class User extends Eloquent {

protected function getDateFormat()
{
return 'U';
}

}

<a name="query-scopes"></a>
## Query Scopes

#### Query Scope တစ်ခုအား သတ်မှတ်ခြင်း

Scope များမှာ သင့်၏ query logic များကို ထပ်ခါထပ်ခါ အသုံးပြုနိုင်ခြင်း ဖြင့် သက်သာစေသည်။  Scope တစ်ခုကို ဖန်တီးနိုင်ရင် `scope` ဟုပါဝင်သည့် 
method တစ်ခုကို ဖန်တီးရန် လိုပေမည်။

class User extends Eloquent {

public function scopePopular($query)
{
return $query->where('votes', '>', 100);
}

public function scopeWomen($query)
{
return $query->whereGender('W');
}

}

#### Query Scope တစ်ခုအား အသုံးပြုခြင်း 

$users = User::popular()->women()->orderBy('created_at')->get();

#### Scopes အရှင်များ ဖန်တီးခြင်း

တခါတရံ သင့်အနေဖြင့် parameter များလက်ခံသော scope အရှင်များကို ဖန်တီးလိုပေမည်။ ထိုသို ့ပြုလုပ်နိုင်ရန် သင့်၏ scope function အတွင်းတွင်

class User extends Eloquent {

public function scopeOfType($query, $type)
{
return $query->whereType($type);
}

}

ထိုနောက် scope တွင် parameter ကို ထည့်သွင်း အသုံးပြုနိုင်သည်။

$users = User::ofType('member')->get();

<a name="relationships"></a>
## Relationships

Database table များမှာ တခါတရံ တစ်ခုနှင့် တစ်ခု ဆက်စပ်ပြီး တည်ရှိနိုင်ပေသည်။ ဥပမာ Blog post တစ်ခုတွင် comment များစွာ ပါဝင်သကဲ့သို ့  Order တစ်ခုတွင်လည်း User တစ်ယောက်နှင့် ဆက်စပ်နိုင် ပေသည်။ Laravel အနေဖြင့် ဆက်စပ်မှု မျိုးစုံကို ဆောင်ရွက်နိုင်အောင် ကူညီပေးထားပါသည်။

- [One To One](#one-to-one)
- [One To Many](#one-to-many)
- [Many To Many](#many-to-many)
- [Has Many Through](#has-many-through)
- [Polymorphic Relations](#polymorphic-relations)
- [Many To Many Polymorphic Relations](#many-to-many-polymorphic-relations)

<a name="one-to-one"></a>
### One To One

#### One To One Relation တစ်ခုကို တည်ဆောက်ခြင်း

one-to-one relationship မှာ အလွန်ပင် အခြေခံကျသော relation ဖြစ်သည်။  ဥပမာ User တစ်ယောက်တွင် Phone တစ်လုံး ရှိရမည့် အနေအထားမျိုး။ 
ထိုသို ့သော relation မျိုးကို Eloquent တွင် ဖန်တီးနိုင်ပေသည်။

class User extends Eloquent {

public function phone()
{
return $this->hasOne('Phone');
}

}


`hasOne` တွင်ထည့်သွင်းရမည့် argument မှာ ဆက်စပ်နေသည့် Model ၏ အမည်ပင်ဖြစ်သည်။ relationship တည်ဆောက်ပြီးသည်နှင့် Eloquent ၏ [dynamic properties](#dynamic-properties): ကို အသုံးပြုပြီး အချက်အလက်များကို ထုတ်ယူနိုင်သည်။

$phone = User::find(1)->phone;

အထက်ပါ statement အတွက် run သွားမည့် SQL မှာ အောက်ပါ အတိုင်းဖြစ်သည်။

select * from users where id = 1

select * from phones where user_id = 1

Eloquent အနေဖြင့် model name များကို အခြေခံပြီး Forigen key များကို သတ်မှတ်သွားမည်ကို သတိပြုရမည်။ အထက်က `Phone` model တွင် `user_id` ကို foreign key အနေဖြင့် အလိုအလျောက် သတ်မှတ်မည် ဖြစ်သည်။  မိမိတို ့ စိတ်ကြိုက်ပြောင်းလဲလိုပါက `hasOne` method တွင် ဒုတိယ argument အဖြစ်သွင်းပေးရန် လိုပေမည်။  ထိုထက်ပို၍ တတိယ argument အနေဖြင့် ထည့်သွင်းပါက မည်သည့် local column ကို ပူးပေါင်းမည်ကိုပါ သတ်မှတ်နိုင်သည်။

return $this->hasOne('Phone', 'foreign_key');

return $this->hasOne('Phone', 'foreign_key', 'local_key');

#### Relation ပြောင်းပြန်သတ်မှတ်ခြင်း

`Phone` model မှ Relation ကို ပြောင်းပြန်အနေဖြင့် သတ်မှတ်လိုပါက `belongsTo` ဟုသည့် method ကို အသုံးပြုနိုင်သည်။

class Phone extends Eloquent {

public function user()
{
return $this->belongsTo('User');
}

}

အထက်က ဥပမာတွင် Eloquent အနေဖြင့် `phones` table မှ `user_id` column ကို အသုံးပြုမည် ဖြစ်သည်။ `hasMany` ကဲ့သို ့ပင် Foriegn Key ကို သတ်မှတ်လိုပါက ဒုတိယ argument ကို ထည့်သွင်းပေးနိုင်သည်။


class Phone extends Eloquent {

public function user()
{
return $this->belongsTo('User', 'local_key');
}

}

ထိုအပြင် parent table နှင့်ဆက်စပ်နေသည့်  column ကို တတိယ parameter အဖြစ် ထည့်သွင်းနိုင်သည်။

class Phone extends Eloquent {

public function user()
{
return $this->belongsTo('User', 'local_key', 'parent_key');
}

}

<a name="one-to-many"></a>
### One To Many

one-to-many relation ၏ ဥပမာမှာ blog post တစ်ခုတွင် comment များစွာ ရှိသကဲ့သို ့ပင် ဖြစ်သည်။ ထိုသို ့ relation ကို အောက်ပါအတိုင်း model တွင်
သတ်မှတ်နိုင်သည်။

class Post extends Eloquent {

public function comments()
{
return $this->hasMany('Comment');
}

}

ထိုအခါ post comments များကို [dynamic property](#dynamic-properties) ကို အသုံးပြု၍ ထုတ်ယူနိုင်ပါပြီ။

$comments = Post::find(1)->comments;

ထိုထဲမှ  ထုတ်ယူလိုသည့် comment များကို စစ်ယူလိုပါက `comments` method နောက်တွင် method များကို စီတန်း အသုံးပြုနိုင်ပါသေးသည်။

$comments = Post::find(1)->comments()->where('title', '=', 'foo')->first();

၄င်းတွင်လည်း `hasOne` ကဲ့သို ့ foriegn key ကို  `hasMany` method  နောက်တွင် second argument အနေဖြင့်နှင့်  third argument ကို local key အနေဖြင့် ထည့်သွင်းနိုင်ပေသည်။

return $this->hasMany('Comment', 'foreign_key');

return $this->hasMany('Comment', 'foreign_key', 'local_key');

#### ပြောင်းပြန် relation သတ်မှတ်ခြင်း

`Comment` model ပြောင်းပြန် သတ်မှတ်နိုင်ရန်  `belongsTo` ဟုသည့် method ကို အသုံးပြုနိုင်သည်။

class Comment extends Eloquent {

public function post()
{
return $this->belongsTo('Post');
}

}

<a name="many-to-many"></a>
### Many To Many


Many-to-many relations မှာ ပိုမိုရှုပ်ထွေးသည့် relation ဖြစ်သည်။ ဥပမာ User တစ်ယောက်မှာ တာဝန်များစွာ ရှိပြီး တာဝန်တစ်ခုကိုလည်း User များစွာ ခွဲဝေပေးအပ်ထားသည် ဆိုပါစို ့။  User များစွာ  "Admin" တာဝန်ကို ယူထားနိုင်သည့် အခြေအနေတွင်ရှိပေမည်။  ထိုသို ့သော အခြေအနေတွင် Database 
Table သုံးခု လိုအပ်မည် ဖြစ်သည်။ ၄င်းတို ့မှာ `users` ၊ `roles` နှင့် `role_user` တို ့ဖြစ်ကြသည်။ `role_user` table မှာ ဆက်စပ်နေသည့် model အမည်များကို ဆက်စပ်ပေးမည် ဖြစ်ပြီး ၄င်းတွင် `user_id` နှင့် `role_id` ဟူသော columns နှစ်ခု ပါဝင်မည် ဖြစ်သည်။


many-to-many relation  ကို  `belongsToMany` method ကို အသုံးပြု၍ ရေးသားနိုင်သည်။

class User extends Eloquent {

public function roles()
{
return $this->belongsToMany('Role');
}

}

ထိုအခါ `User` မှ role ကို အောက်ပါ အတိုင်း ထုတ်ယူနိုင်မည် ဖြစ်သည်။

$roles = User::find(1)->roles;

မိမိ၏ ကြားခံ table ၏ အမည်ကို စိတ်ကြိုက် သတ်မှတ်လိုပါက `belongsToMany` method ၏ ဒုတိယ argument မှ သွင်း၍ စိတ်ကြိုက် သတ်မှတ်နိုင်သည်။

return $this->belongsToMany('Role', 'user_roles');

ထိုအပြင် ပါဝင်ပတ်သတ်နေသော Keys များကိုလည်း စိတ်ကြိုက် သတ်မှတ်နိုင်သည်။

return $this->belongsToMany('Role', 'user_roles', 'user_id', 'foo_id');

ထိုအပြင် `Role` model မှလည်း ပြောင်းပြန်သတ်မှတ် ၍လည်း ဖြစ်ပါသည်။

class Role extends Eloquent {

public function users()
{
return $this->belongsToMany('User');
}

}

<a name="has-many-through"></a>
### Has Many Through

 "has many through"  ဝေးကွာနေသည့် relation များမှ record များကို access လုပ်နိုင်ရန် အလွယ်တကူ ကြားဖြတ်ဆောင်ရွက်ပေးသော method ဇြစ်သည်။ ဥပမာ  `Country` model မှာ  `Posts` ဖြင့် ချိတ်ဆက်ထားခြင်း မရှိသော်လည်း `Users` model ဖြင့်မူ ချိတ်ဆက်ထားပါက တဆင့်ကျော်၍ access လုပ်နိုင်သည်။  ထို table များ၏  relationship မှာ အောက်ပါအတိုင်း ဆိုပါစို ့

countries
id - integer
name - string

users
id - integer
country_id - integer
name - string

posts
id - integer
user_id - integer
title - string

`posts` table တွင် `country_id` column မပါဝင်သော်လည်း `hasManyThrough` relation ဖြင့် `$country->posts` ဟု၍ accessible ဖြစ်အောင် စွမ်းဆောင်နိုင်ပေသည်။ 

class Country extends Eloquent {

public function posts()
{
return $this->hasManyThrough('Post', 'User');
}

}

relationship key များကို စိတ်ကြိုက်သတ်မှတ်လိုပါက တတိယနှင့် စတုတ္ထ parameter အများအဖြစ် ထည့်သွင်းနိုင်ပေသည်။

class Country extends Eloquent {

public function posts()
{
return $this->hasManyThrough('Post', 'User', 'country_id', 'user_id');
}

}

<a name="polymorphic-relations"></a>
### Polymorphic Relations

Polymorphic relations ကို အသုံးပြုခြင်းဖြင့် အခြား Model တစ်ခုထက်ပို၍ associate ပြုလုပ်ထားသော record များကို ထုတ်ယူနိုင်သည်။ ဥပမာ သင့်တွင် 
staff ဟုသော model နှင့် order ဟုသော model နှစ်ခုလုံးနှင့် ပတ်သတ်နေသည့် photo ဟုသော model ရှိသည် ဆိုပါစို ့။


class Photo extends Eloquent {

public function imageable()
{
return $this->morphTo();
}

}

class Staff extends Eloquent {

public function photos()
{
return $this->morphMany('Photo', 'imageable');
}

}

class Order extends Eloquent {

public function photos()
{
return $this->morphMany('Photo', 'imageable');
}

}

#### Polymorphic Relation ကို အသုံးပြုခြင်း

အောက်ပါ အတိုင်း photo များကို Staff member များမှသော်လည်းကောင်း  Order မှသော်လည်းကောင်း ထုတ်ယူနိုင်သည်။

$staff = Staff::find(1);

foreach ($staff->photos as $photo)
{
//
}

#### Polymorphic Relation မှ Owner ၏ record များကို ထုတ်ယူခြင်း

သို ့သော် တကယ့် "polymorphic" အလှတရားမှာ `Photo` model မှ staff ဖြစ်စေ ၊ order ဖြစ်စေ ထုတ်ယူနိုင်ခြင်း ဖြစ်သည်။


$photo = Photo::find(1);

$imageable = $photo->imageable;

`Photo` model မှ `imageable` relation  မှာ `Staff` မှဖြစ်စေ `Order` instance ဖြစ်စေ ပိုင်ဆိုင်သည့် model ပေါ်မူတည်၍ ထုတ်ပေးသွားမည် ဖြစ်သည်။

#### Polymorphic Relation Table Structure

မည်သို ့မည်ပုံ အလုပ်လုပ်ဆောင်သွားသည်ကို သိရှိနိုင်ရန် အောက်ပါ database structure ကို ကြည့်ရှုနိုင်ပါသည်။

staff
id - integer
name - string

orders
id - integer
price - integer

photos
id - integer
path - string
imageable_id - integer
imageable_type - string

Key field အနေဖြင့်  `photos` table မှ `imageable_id` နှင့် `imageable_type` တို ့ကို မှတ်သားထားရပါမည် ဖြစ်သည်။ ID မှာ Order သို ့မဟုတ် 
Staff တို ့၏  ID နှင့် ချိတ်ဆက်ထားမည် ဖြစ်သည်။  ORM အနေဖြင့် မည်သည့် model ကိုပြန်ရမည် ဆိုသည်ကို `imageable` ၏ relation ကို ထောက်ရှု၍ လုပ်ဆောင် သွားမည် ဖြစ်သည်။

<a name="many-to-many-polymorphic-relations"></a>
### Many To Many Polymorphic Relations

#### Polymorphic Many To Many Relation Table Structure  

သမရိုးကျ polymorphic relations တစ်ခုသာမက many-to-many polymorphic relations များကိုပါ တည်ဆောက်နိုင်သည်။ ဥပမာ blog တစ်ခု၏ database structure ဖြစ်သော  `Post` နှင့် `Video` model တို ့တွင် `Tag` model ကို တူညီစွာ polymorphic relation အနေဖြင့် ချိတ်ဆက်ရန် လိုပေမည်။ ရှေဦးစွာ table structure ကိုကြည့်လိုက်ပါ။

posts
id - integer
name - string

videos
id - integer
name - string

tags
id - integer
name - string

taggables
tag_id - integer
taggable_id - integer
taggable_type - string

အထက်ပါ table အတွက် relationship များကို model တွင် အောက်ပါအတိုင်း တည်ဆောက်ရမည်ဖြစ်သည်။ `Post` နှင့် `Video` model တို ့နှစ်ခုလုံးတွင်
`tags` method မှ `morphToMany` relationship ကို ကြေညာပေးရမည် ဖြစ်သည်။

class Post extends Eloquent {

public function tags()
{
return $this->morphToMany('Tag', 'taggable');
}

}

`Tag` model အနေဖြင့် ၄င်း၏ relationships ကို အောက်ပါ အတိုင်း သတ်မှတ်နိုင်သည်။ 

class Tag extends Eloquent {

public function posts()
{
return $this->morphedByMany('Post', 'taggable');
}

public function videos()
{
return $this->morphedByMany('Video', 'taggable');
}

}

<a name="querying-relations"></a>
## Relation များကို Query ပြုလုပ်ခြင်း

#### Relations များတွင် Select ကို အသုံးပြုခြင်း

Model များမှ record များကို access ပြုလုပ်ရာတွင် ၊  result များကို စစ်ဆေးပြီးမှ ထုတ်ယူလိုသည့် အနေအထားမျိုးတွင် ရှိပေနိုင်သည်။ ဥပမာ သင့်အနေဖြင့် Comment တစ်ခု အနည်းဆုံး ရှိသည့် blog post များကို ဆွဲထုတ်လိုသည် ဆိုပါစို ့။  သင့်အနေနဲ ့ `has` method ကို အသုံးပြုရမည် ဖြစ်သည်။

$posts = Post::has('comments')->get();

has method တွင် သင့်အနေဖြင့် operator များ နှင့်  ထုတ်ယူလိုသည့် အရေအတွက်ကို သတ်မှတ်နိုင်သည်။

$posts = Post::has('comments', '>=', 3)->get();

သင့်အနေဖြင့် ပို၍ အသေးစိတ်ကျပြီး "where" conditions များကို `has` queries အတွင်း စစ်ဆေးလိုပါက `whereHas` နှင့် `orWhereHas` method များကိုအသုံးပြုနိုင်သည်။

$posts = Post::whereHas('comments', function($q)
{
$q->where('content', 'like', 'foo%');

})->get();

<a name="dynamic-properties"></a>
### Dynamic Properties


Eloquent တွင် သင့်အနေဖြင့် relations များမှ properties များကို dynamic properties အနေဖြင့် ဆွဲယူနိုင်သည်။ Eloquent အနေဖြင့် သင့်၏ relationship အလိုအလျောက်အနေဖြင့် relations ကို အလိုအလျောက် load လုပ်ကာ ခေါ်ယူမည် ဖြစ်ပြီး  `get` ( one-to-many relationships) ပေလော၊ `first` (for one-to-one relationships) method ပေလော ကိုပင် ခွဲခြားလုပ်ဆောင်ပေးမည် ဖြစ်သည်။ ထိုနောက် တူညီသော အမည်မှ တဆင့်
dynamic property ကို အလွယ်တကူ ခေါ်ဆို နိုင်ပေမည်။ ဥပမာ `$phone` ဟုသည့် model မှ တဆင့်

class Phone extends Eloquent {

public function user()
{
return $this->belongsTo('User');
}

}

$phone = Phone::find(1);

Instead of echoing the user's email like this:

echo $phone->user()->first()->email;

It may be shortened to simply:

echo $phone->user->email;

> **Note:** Relationships များကို return ပြန်သော result များကို အလုပ်လုပ်သွားသော method မှာ `Illuminate\Database\Eloquent\Collection` class မှ instance များကို ပြန်ခြင်းဖြစ်သည်။

<a name="eager-loading"></a>
## Eager Loading


Eager loading exists to N+1 query ကဲ့သို ့သော Query များကို ပို ့၍ ပေါ့ပါးစွာ အသုံးပြုနိုင်ရန် ဖြစ်သည်။ ဥပမာ `Author` model နှင့် `Book` model တို ့ ဆက်စပ်နေသည် ဆိုပါစို ့။ ၄င်းတို ့၏ relationship ကို အောက်ပါ အတိုင်း သတ်မှတ်နိုင်ပါသည်။ 

class Book extends Eloquent {

public function author()
{
return $this->belongsTo('Author');
}

}

ထိုနောက် အောက်ပါ code ကို ကြည့်ကြည့်ပါ။

foreach (Book::all() as $book)
{
echo $book->author->name;
}

ထို loop မှာ Book မှ ရှိသမျှ စာအုပ်တိုင်းကို ခေါ်ယူမည် ဖြစ်သည်။ ထိုနောက် ထိုနောက် ထိုနောက် နောက် query တစ်ခုအနေဖြင့် စာအုပ်တစ်ခုချင်းဆီ၏ 
author ကို  ဖော်ပြသွားမည် ဖြစ်သည်။ အကယ်၍ စာအုပ် ၂၅ အုပ် ရှိသည် ဆိုပါစို ့ ၊ query ၂၆ ကြောင်း run ဖြစ်သည်။ သို ့ပင်သော်ညား eager loading ၏ အကျိုးကျေးဇူးကြောင့် မလိုအပ်သော query များကို လျှော့ချနိုင်သည်။ ထို relationship တွင် `with` method အသုံးပြု၍ eager load ပြုလုပ်နိုင်သည်။


foreach (Book::with('author')->get() as $book)
{
echo $book->author->name;
}

အထက်ပါ loop တွင်မူ query နှစ်ကြောင်းသာ execute ပြုလုပ်မည် ဖြစ်သည်။

select * from books

select * from authors where id in (1, 2, 3, 4, 5, ...)

Eager loading ကို အသုံးပြုခြင်း အားဖြင့် သင့် application ၏ performance ကို သိသိသာသာ မြင့်တက်စေမည် ဖြစ်သည်။

ထိုအပြင် တစ်ခုထက်ပိုသော relation များတွင် တချိန်တည်းတွင် eager load အသုံးပြုနိုင်မည် ဖြစ်သည်။

$books = Book::with('author', 'publisher')->get();

Nested relationship များတွင်လည်း eager load အသုံးပြုနိုင်သည်။

$books = Book::with('author.contacts')->get();

အထက် ဥပမာ တွင် `author` နှင့် ပတ်သတ်နေသည်များကို eager load ပြုလုပ်ပြီး author ၏ `contacts` relation ပါ load သွားမည် ဖြစ်သည်။

### Eager Load အကန် ့အသတ်များ 

ထခါတရံ condition များ စစ်ဆေးပြီးမှ relationship များကို eager load ပြုလုပ်လိုမည် အချိန်ကာလ လည်း ရှိပေမည်။ အောက်က ဥပမာတွင် ဆိုပါစို ့

$users = User::with(array('posts' => function($query)
{
$query->where('title', 'like', '%first%');

}))->get();

ထို ဥပမာ တွင် user's post တွင်းမှ  "first"  စကာလုံး နှင့်စတင်သည်များကိုသာ eager load လုပ်သွားမည် ဖြစ်သည်။ Closure များ အတွင်းတွင်မူ အကန့် အသတ်မရှိပေ။  သင့်အနေဖြင့် အောက်က အတိုင်း order အလိုက် စီရီနိုင်ပေဦးမည်။

$users = User::with(array('posts' => function($query)
{
$query->orderBy('created_at', 'desc');

}))->get();

### Lazy Eager Loading

တည်ရှိနေပြီးသော model collection များထဲမှ eager load နှင့် ဆက်စပ်နေသော model များကို တိုက်ရိုက် ခေါ်ယူ၍လည်း ဖြစ်နိုင်ပေသည်။ ထိုသို ့ပြုလုပ်ခြင်း Model များကို Load လုပ်ရာတွင် load လုပ်မည် မလုပ်မည်ကို dynamically စဉ်းစားဆုံးဖြတ်ရာတွင် သော်လည်းကောင်း ၊ caching ဖြင့် ပူးပေါင်း အသုံးပြုရာတွင်သော်လည်းကောင်း အသုံးဝင်သည်။

$books = Book::all();

$books->load('author', 'publisher');

<a name="inserting-related-models"></a>
## ဆက်စပ်နေသည့် Model များတွင် data ထည့်သွင်းခြင်း

#### ဆက်စပ်နေသည့် Model ဖြင့် ချိတ်ဆက်ခြင်း

တခါတရံ ဆက်စပ်နေသည့် model များအား insert ပြုလုပ်ရန်လည်း လိုပေမည်။ ဥပမာ သင့်အနေဖြင့် post တစ်ခုတွင် comment တစ်ခုကို ထည့်သွင်းမည် ဆိုပါစို ့။  Model တစ်ခု၏ `post_id` foreign key ကို manually ထည့်သွင်းနေမည့် အစား `Post` model ဖက်မှ တိုက်ရိုက်ထည့်သွင်း၍လည်း ရပေသည်။

$comment = new Comment(array('message' => 'A new comment.'));

$post = Post::find(1);

$comment = $post->comments()->save($comment);

အထက်က ဥပမာတွင် `post_id` field ကို အလိုအလျောက် ထည့်သွင်းသွားမည် ဖြစ်သည်။

### Models များ ဆက်စပ်ခြင်း(Belongs To)

`belongsTo` relationship ဖြင့် Method များကို data များ update လုပ်စေလိုပါက `associate` method ကို အသုံးပြုနိုင်သည်။ ထို method သည် လက်အောက်ခံ model ၏ foregin key ကိုပါ ထည့်သွင်းပေးသွားမည် ဖြစ်သည်။

$account = Account::find(10);

$user->account()->associate($account);

$user->save();

### ဆက်စပ်နေသည့် Model တွင် data ထည့်သွင်းခြင်း (Many To Many)

သင့်အနေဖြင့် many-to-many relations ရှိနေသည့် model များတွင်လည်း data ဖြည့်သွင်းလိုပေမည်။  ထုံးစံ ဥပမာ တစ်ခုဖြစ်သည့် `User` နှင့် `Role` model များဖြင့် ရှေ  ့ဆက်ကြစို ့။ user ၏ roles များကို `attach` method ဖြင့် ချိတ်ဆက်နိုင်သည်။

#### Attaching Many To Many Models

$user = User::find(1);

$user->roles()->attach(1);

You may also pass an array of attributes that should be stored on the pivot table for the relation:

$user->roles()->attach(1, array('expires' => $expires));

Of course, the opposite of `attach` is `detach`:

$user->roles()->detach(1);

#### Using Sync To Attach Many To Many Models

You may also use the `sync` method to attach related models. The `sync` method accepts an array of IDs to place on the pivot table. After this operation is complete, only the IDs in the array will be on the intermediate table for the model:

$user->roles()->sync(array(1, 2, 3));

#### Adding Pivot Data When Syncing

You may also associate other pivot table values with the given IDs:

$user->roles()->sync(array(1 => array('expires' => true)));

Sometimes you may wish to create a new related model and attach it in a single command. For this operation, you may use the `save` method:

$role = new Role(array('name' => 'Editor'));

User::find(1)->roles()->save($role);

In this example, the new `Role` model will be saved and attached to the user model. You may also pass an array of attributes to place on the joining table for this operation:

User::find(1)->roles()->save($role, array('expires' => $expires));

<a name="touching-parent-timestamps"></a>
## Touching Parent Timestamps

When a model `belongsTo` another model, such as a `Comment` which belongs to a `Post`, it is often helpful to update the parent's timestamp when the child model is updated. For example, when a `Comment` model is updated, you may want to automatically touch the `updated_at` timestamp of the owning `Post`. Eloquent makes it easy. Just add a `touches` property containing the names of the relationships to the child model:

class Comment extends Eloquent {

protected $touches = array('post');

public function post()
{
return $this->belongsTo('Post');
}

}

Now, when you update a `Comment`, the owning `Post` will have its `updated_at` column updated:

$comment = Comment::find(1);

$comment->text = 'Edit to this comment!';

$comment->save();

<a name="working-with-pivot-tables"></a>
## Working With Pivot Tables

As you have already learned, working with many-to-many relations requires the presence of an intermediate table. Eloquent provides some very helpful ways of interacting with this table. For example, let's assume our `User` object has many `Role` objects that it is related to. After accessing this relationship, we may access the `pivot` table on the models:

$user = User::find(1);

foreach ($user->roles as $role)
{
echo $role->pivot->created_at;
}

Notice that each `Role` model we retrieve is automatically assigned a `pivot` attribute. This attribute contains a model representing the intermediate table, and may be used as any other Eloquent model.

By default, only the keys will be present on the `pivot` object. If your pivot table contains extra attributes, you must specify them when defining the relationship:

return $this->belongsToMany('Role')->withPivot('foo', 'bar');

Now the `foo` and `bar` attributes will be accessible on our `pivot` object for the `Role` model.

If you want your pivot table to have automatically maintained `created_at` and `updated_at` timestamps, use the `withTimestamps` method on the relationship definition:

return $this->belongsToMany('Role')->withTimestamps();

#### Deleting Records On A Pivot Table

To delete all records on the pivot table for a model, you may use the `detach` method:

User::find(1)->roles()->detach();

Note that this operation does not delete records from the `roles` table, but only from the pivot table.

#### Updating A Record On A Pivot Table

Sometimes you may need to update your pivot table, but not detach it. If you wish to update your pivot table in place you may use `updateExistingPivot` method like so:

User::find(1)->roles()->updateExistingPivot($roleId, $attributes);

#### Defining A Custom Pivot Model

Laravel also allows you to define a custom Pivot model. To define a custom model, first create your own "Base" model class that extends `Eloquent`. In your other Eloquent models, extend this custom base model instead of the default `Eloquent` base. In your base model, add the following function that returns an instance of your custom Pivot model:

public function newPivot(Model $parent, array $attributes, $table, $exists)
{
return new YourCustomPivot($parent, $attributes, $table, $exists);
}

<a name="collections"></a>
## Collections

All multi-result sets returned by Eloquent, either via the `get` method or a `relationship`, will return a collection object. This object implements the `IteratorAggregate` PHP interface so it can be iterated over like an array. However, this object also has a variety of other helpful methods for working with result sets.

#### Checking If A Collection Contains A Key

For example, we may determine if a result set contains a given primary key using the `contains` method:

$roles = User::find(1)->roles;

if ($roles->contains(2))
{
//
}

Collections may also be converted to an array or JSON:

$roles = User::find(1)->roles->toArray();

$roles = User::find(1)->roles->toJson();

If a collection is cast to a string, it will be returned as JSON:

$roles = (string) User::find(1)->roles;

#### Iterating Collections

Eloquent collections also contain a few helpful methods for looping and filtering the items they contain:

$roles = $user->roles->each(function($role)
{
//
});

#### Filtering Collections

When filtering collections, the callback provided will be used as callback for [array_filter](http://php.net/manual/en/function.array-filter.php).

$users = $users->filter(function($user)
{
return $user->isAdmin();
});

> **Note:** When filtering a collection and converting it to JSON, try calling the `values` function first to reset the array's keys.

#### Applying A Callback To Each Collection Object

$roles = User::find(1)->roles;

$roles->each(function($role)
{
//
});

#### Sorting A Collection By A Value

$roles = $roles->sortBy(function($role)
{
return $role->created_at;
});

#### Sorting A Collection By A Value

$roles = $roles->sortBy('created_at');

#### Returning A Custom Collection Type

Sometimes, you may wish to return a custom Collection object with your own added methods. You may specify this on your Eloquent model by overriding the `newCollection` method:

class User extends Eloquent {

public function newCollection(array $models = array())
{
return new CustomCollection($models);
}

}

<a name="accessors-and-mutators"></a>
## Accessors & Mutators

#### Defining An Accessor

Eloquent provides a convenient way to transform your model attributes when getting or setting them. Simply define a `getFooAttribute` method on your model to declare an accessor. Keep in mind that the methods should follow camel-casing, even though your database columns are snake-case:

class User extends Eloquent {

public function getFirstNameAttribute($value)
{
return ucfirst($value);
}

}

In the example above, the `first_name` column has an accessor. Note that the value of the attribute is passed to the accessor.

#### Defining A Mutator

Mutators are declared in a similar fashion:

class User extends Eloquent {

public function setFirstNameAttribute($value)
{
$this->attributes['first_name'] = strtolower($value);
}

}

<a name="date-mutators"></a>
## Date Mutators

By default, Eloquent will convert the `created_at`, `updated_at`, and `deleted_at` columns to instances of [Carbon](https://github.com/briannesbitt/Carbon), which provides an assortment of helpful methods, and extends the native PHP `DateTime` class.

You may customize which fields are automatically mutated, and even completely disable this mutation, by overriding the `getDates` method of the model:

public function getDates()
{
return array('created_at');
}

When a column is considered a date, you may set its value to a UNIX timestamp, date string (`Y-m-d`), date-time string, and of course a `DateTime` / `Carbon` instance.

To totally disable date mutations, simply return an empty array from the `getDates` method:

public function getDates()
{
return array();
}

<a name="model-events"></a>
## Model Events

Eloquent models fire several events, allowing you to hook into various points in the model's lifecycle using the following methods: `creating`, `created`, `updating`, `updated`, `saving`, `saved`, `deleting`, `deleted`, `restoring`, `restored`.

Whenever a new item is saved for the first time, the `creating` and `created` events will fire. If an item is not new and the `save` method is called, the `updating` / `updated` events will fire. In both cases, the `saving` / `saved` events will fire.

#### Cancelling Save Operations Via Events

If `false` is returned from the `creating`, `updating`, `saving`, or `deleting` events, the action will be cancelled:

User::creating(function($user)
{
if ( ! $user->isValid()) return false;
});

#### Setting A Model Boot Method

Eloquent models also contain a static `boot` method, which may provide a convenient place to register your event bindings.

class User extends Eloquent {

public static function boot()
{
parent::boot();

// Setup event bindings...
}

}

<a name="model-observers"></a>
## Model Observers

To consolidate the handling of model events, you may register a model observer. An observer class may have methods that correspond to the various model events. For example, `creating`, `updating`, `saving` methods may be on an observer, in addition to any other model event name.

So, for example, a model observer might look like this:

class UserObserver {

public function saving($model)
{
//
}

public function saved($model)
{
//
}

}

You may register an observer instance using the `observe` method:

User::observe(new UserObserver);

<a name="converting-to-arrays-or-json"></a>
## Arrays နှင့် JSON သို ့ပြောင်းလဲခြင်း

#### Model တစ်ခုကို Array သို ့ပြောင်းလဲခြင်း

JSON APIs များ တည်ဆောက်ရာတွင် ၊ သင့် အနေဖြင့် model နှင့် ဆက်စပ်ပတ်သတ်သည်များကို array အနေဖြင့်သော လည်းကောင်း JSON အနေဖြင့်သော် လည်းကောင်း ထုတ်ပေးလိုသည့် အချိန်ကာလ များ ရှိပေမည်။ Eloquent အနေဖြင့် ထိုသို ့ပြုလုပ်နိုင်ရန် ထောက်ပံ့ပေးသော method များလည်း ရှိပေသည်။ ထိုသို ့ ၄င်းနှင့် တကွ ဆက်စပ်ပတ်သတ်နေသည်များကိုပါက array အဖြစ်ပြောင်းလဲ နိုင်ရန် `toArray` method ကို အသုံးပြုနိုင်သည်။

$user = User::with('roles')->first();

return $user->toArray();

Models connection တစ်ခုလုံးပါ array အဖြစ် ပြောင်းလဲသွားသည်ကို သတိပြုရမည်။ 

return User::all()->toArray();

#### Model တစ်ခုကို JSON သို ့ ပြောင်းလဲခြင်း

Model တစ်ခုကို JSON အနေဖြင့် ပြောင်းလဲလိုပါက `toJson` method ကို အသုံးပြုနိုင်သည်။

return User::find(1)->toJson();

#### Route တစ်ခုမှ Model ကို return ပြန်ခြင်း  

Model သို ့မဟုတ် collection တစ်ခုသည် string အဖြစ်သို ့ cast အလုပ်ခံရပါက အလိုအလျောက် JSON အဖြစ်သို ့ပြောင်းလဲသွားမည် ဖြစ်သည်။ 
ထို ့ကြောင့် သင့် application route မှ တိုက်ရိုက် return ပြန်၍လည်း ရနိုင်သည်။

Route::get('users', function()
{
return User::all();
});

#### Array သို ့မဟုတ် JSON သို ့ပြောင်းလဲရာတွင် Attribute တစ်ချို  ့ကို ဖျောက်ထားခြင်း

တခါတရံ သင့်အနေဖြင့် တချို  ့သော attribute များကို array သို ့မဟုတ် JSON သို ့ပြောင်းလဲရာတွင် ပါမလာ စေချင်သည့် attribute များ (ဥပမာ password များကဲ့သို ့သော ) ရှိပေမည်။ ထိုသို ့ပြုလုပ်နိုင်ရန် model အတွင်းတွင် `hidden` ဟူသော property တစ်ခုအဖြစ် ကြေညာပေးရန် လိုပေမည်။

class User extends Eloquent {

protected $hidden = array('password');

}

> **Note:** When hiding relationships, use the relationship's **method** name, not the dynamic accessor name.

အပြန်အလှန်အားဖြင့် သင့်အနေဖြင့် ထုတ်ချင်သည်များကိုသာ ဖော်ပြနိုင်ရန် `visible` ဟုသည် property တစ်ခု ကြေညာနိုင်ပေသည်။

protected $visible = array('first_name', 'last_name');

<a name="array-appends"></a>
Occasionally, you may need to add array attributes that do not have a corresponding column in your database. To do so, simply define an accessor for the value:

public function getIsAdminAttribute()
{
return $this->attributes['admin'] == 'yes';
}

Once you have created the accessor, just add the value to the `appends` property on the model:

protected $appends = array('is_admin');

Once the attribute has been added to the `appends` list, it will be included in both the model's array and JSON forms.