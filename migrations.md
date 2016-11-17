# Migrations & Seeding

- [အစပျိုး](#introduction)
- [Migrations ဖန်တီးခြင်း](#creating-migrations)
- [Migrations ပြုလုပ်ခြင်း](#running-migrations)
- [Migrations နောက်ပြန်ခြင်း](#rolling-back-migrations)
- [Database Seeding](#database-seeding)

<a name="introduction"></a>
## အစပျိုး

Migration မှာ Database အတွက် Version Control ပြုလုပ်နိုင်ရန် ဖန်တီးထားသည်။ ၄င်းကို အသုံးပြုခြင်းဖြင့် database schema များကို အလွယ်တကူ ပြင်ဆင်နိုင်ပြီး team တစ်ခုလုံး တူညီသည့် database schema ကို အသုံးပြုနိုင်ရန် ပံပိုးထားသည်။ Migrations မှာ ပှံမှန်အားဖြင့် [Schema Builder](schema.md) ဖြင့် တွဲဖက် အသုံးပြုကြသည်။

<a name="creating-migrations"></a>
## Migrations ဖန်တီးခြင်း

migration တစ်ခု ဖန်တီးနိုင်ရန် Artisan CLI တွင် `migrate:make` ဟူသည် command ကို အသုံးပြုနိုင်သည်။

	php artisan migrate:make create_users_table

Migration file များသည် `app/database/migrations` ဆိုသည့် folder တွင်တည်ရှိမည် ဖြစ်ပြီး Migrations များကို အစဉ်အတိုင်း စီရီထားမည့် timestamp ဖြင့် သတ်မှတ်ထားမည် ဖြစ်သည်။

Migration တစ်ခု ဖန်တီးနေစဉ် `--path` ဟု attribute ကို အသုံးပြုနိုင်သည်။ အဆိုပါ path မှာ သင် install လုပ်ထားသော root directory မှ အလိုအလျောက် သိရှိမည် ဖြစ်ပါသည်။

	php artisan migrate:make foo --path=app/migrations

	
`--table` နှင့် `--create` options များကို အသုံးပြု၍  table အမည်ကို သတ်မှတ်ခြင်း ၊  table အသစ်ကို ဖန်တီးခြင်း များ ပြုလုပ်နိုင်ပါမည်။

	php artisan migrate:make add_votes_to_user_table --table=users

	php artisan migrate:make create_users_table --create=users

<a name="running-migrations"></a>
## Migrations ပြုလုပ်ခြင်း

#### Migration ကို အပြည့်အဝ ပြုလုပ်ခြင်း

	php artisan migrate

#### Path  လမ်းကြောင်းတစ်ခုတွင်သာ Migration ပြုလုပ်ခြင်း

	php artisan migrate --path=app/foo/migrations

#### Package တစ်ခုအတွက် Migration ပြုလုပ်ခြင်း

	php artisan migrate --package=vendor/package

> **သတိပြုရန်:**  migrations run နေစဉ် "class not found" ဟု error တွေ ့ရှိပါက `composer dump-autoload` ဆိုသည့် command ကို run ကြည့်ပါ။

### Production တွင် Migration ပြုလုပ်ခြင်း

အချို  ့သော migration operations များမှာ အန္တာရယ်များလှပေသည်။ တနည်းအားဖြင့် သင့်၏ အချက်အလက်များကို စက္ကန် ့ပိုင်းအတွင် ဆုံးရှုံးသွားစေနိုင်သည်။ အဆိုပါ အန္တာရယ်မှ ကာကွယ်နိုင်ရန်  production အခြေအနေတွင် migration ပြုလုပ်ရန် confirmation တောင်းခံပါသည်။ ထိုတောင်းခံမှုကို ကျော်လွှားလိုပါက `--force` flag ကို အသုံးပြုနိုင်သည်။

	php artisan migrate --force

<a name="rolling-back-migrations"></a>
## Rolling Back Migrations

#### Migrations နောက်ပြန်ခြင်း

	php artisan migrate:rollback

#### Migrations ပထမဆုံး အခြေအနေသို ့ နောက်ပြန်ခြင်း

	php artisan migrate:reset

#### အစမှ အဆုံး နောက်ပြန်ပြီးနောက် တဖန် Migration ပြုလုပ်ခြင်း

	php artisan migrate:refresh

	php artisan migrate:refresh --seed

<a name="database-seeding"></a>
## Database Seeding

Laravel အနေဖြင့် database ကို အလွယ်တကူ seed ပြုလုပ်နိုင်ရင် seed classes များပါရှိပါသည်။ Seed class များမှာ `app/database/seeds` တွင်တည်ရှိမည် ဖြစ်သည်။ Seed class များကို အလိုရှိသလို အမည်ပေးနိုင်သော်လည်း အဓိပ္ပါယ်ရှိသော နာမည်မျိုး ဥပမာ `UserTableSeeder` စသဖြင့်သာ ပေးသင့်သည်။ ပုံမှန်အားဖြင့် `DatabaseSeeder` class မှာ ဖန်တီးပေးထားသည်။ ထို class မှ သင့်အနေဖြင့် `call` method ကို အသုံးပြုကာ
အစီအစဉ်အလိုက် အခြားသော seed classes များကို run နိုင်သည်။

#### Example Database Seed Class

	class DatabaseSeeder extends Seeder {

		public function run()
		{
			$this->call('UserTableSeeder');

			$this->command->info('User table seeded!');
		}

	}

	class UserTableSeeder extends Seeder {

		public function run()
		{
			DB::table('users')->delete();

			User::create(array('email' => 'foo@bar.com'));
		}

	}

Database ကို seed ပြုလုပ်ရန် Artisan CLI မှ `db:seed` command ကို အသုံးပြုနိုင်သည်။

	php artisan db:seed

ပုံမှန်အားဖြင့် `db:seed` command မှာ `DatabaseSeeder` class ကို run မည်ဖြစ်ပြီး ထိုမှတဆင့် အခြား seed class များကို ခေါ်ယူမည် ဖြစ်သည်။ သို ့ပင်သော်ညား `--class` option ကို အသုံးပြုကာ သီးသန် ့ seeder class တစ်ခုချင်းစီလည်း run နိုင်ပါသေးသည်။

	php artisan db:seed --class=UserTableSeeder

ထိုအပြင် `migrate:refresh` ကိုအသုံးပြုကာ, rollback ပြုလုပ်ပြီး  migrations ကို အစမှ တဖန်ပြန်၍ run ခြင်းကိုလည်း ပြုလုပ်နိုင်မည် ဖြစ်သည်။

	php artisan migrate:refresh --seed
