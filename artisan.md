## Artisan CLI

- [မိတ်ဆက်](#introduction)
- [အသုုံးပြုပုုံ](#အသုုံးပြုပုုံ)

<a name="introduction"></a>
## မိတ်ဆက်

Artisan သည် laravel တွင်အသုုံးပြုသည့် command line interface တစ်ခုုဖြစ်သည်။ ၄င်းတွင် application develop ပြုလုုပ်ရာတွင် အလွန်အသုုံးဝင်လှသော commands များစွာပါဝင်သည်။ ၄င်းမှာ Symfony ၏ console component မှဆင်းသက်လာခြင်း ဖြစ်သည်။


<a name="usage"></a>
## အသုုံးပြုပုုံ

#### အသုုံးပြုနုုိင်သည့် Command များကုုိ စီရီပြသခြင်း

Artisan တွင်ပါဝင်သည့် Command အကုုန်လုုံးကုုိ list အနေဖြင့် ပြသလုုိပါက `list` ဟုုသည့် command ကုုိ အသုုံးပြုနုုိင်သည်။

	php artisan list

#### Command  တစ်ခုုချင်း၏ အသုုံးပြုခြင်း လမ်းညွန်မှုကုုိ ကြည့်ရှုခြင်း 

Command တုုိင်းလုုိလုုိ တွင် “help” ဟုု အပုုိဆောင်း ရုုိက်ခြင်း ဖြင့် မိမိတုုိ ့ အသုုံးပြုမည့် Command တွင် ထည့်သွင်းရမည့် arguments များနှင့် ရွေးချယ်စရာများကုုိ သိရှိနုုိင်ပါသည်။

	php artisan help migrate

#### Configuration Environment ကုုိ သတ်မှတ်ခြင်း

မိမိတုုိ ့ အသုုံးပြုလုုိသည် Configuration Environment ကုုိ —env ဟုုသော  switch ဖြင့် ရွေးချယ်သတ်မှတ်နုုိင်သည်။ 


	php artisan migrate --env=local

#### လက်ရှိ အသုုံးပြုနေသည့် Laravel version ကိုုဖော်ပြခြင်း

မိမိတုုိ ့လက်ရှိအသုုံးပြုနေသည့် Laravel version ကုုိ သိရှိလုုိပါက —switch ကုုိ အသုုံးပြုပြီး  သိရှိနုုိင်ပါသည်။ 
 

	php artisan --version
