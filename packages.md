# Package Development

- [Introduction](#introduction)
- [Creating A Package](#creating-a-package)
- [Package Structure](#package-structure)
- [Service Providers](#service-providers)
- [Deferred Providers](#deferred-providers)
- [Package Conventions](#package-conventions)
- [Development Workflow](#development-workflow)
- [Package Routing](#package-routing)
- [Package Configuration](#package-configuration)
- [Package Views](#package-views)
- [Package Migrations](#package-migrations)
- [Package Assets](#package-assets)
- [Publishing Packages](#publishing-packages)

<a name="introduction"></a>
## Introduction

Laravelတွင် တခြား functions တွေ အသစ်ထည့်တဲ့အခါမှာ Packages တွေခွဲပြီးအသစ်ထပ်ထည့်တဲ့နည်းက သမရိုးကျ နည်းလမ်းကောင်းတခုဖြစ်ပါတယ်။ လူအများစုဆောင်းပြီး project တွေကို ဖန်တီးရာတဲ့အခါမှာ အရမ်းအသုံးတည့်တဲ့နည်းလမ်းဖြစ်ပါတယ်။ ဥပမာ [Carbon](https://github.com/briannesbitt/Carbon), or  [Behat](https://github.com/Behat/Behat).

သေချာတာပေါ့ဗျာ ၊ Packages တွေကို အသုံးပြုရာမှာ ပုံစံမျိုးစုံရှိပါတယ်။ တချိုဟာတွေက Laravel တစ်ခုတည်းမဟုတ်ပဲ အခြားခြားသော Framework တွေမှာပါ အလုပ်လုပ်တဲ့ stand-alone packages တွေဖြစ်တယ်။ အပေါ်က CarBon နဲ့ Behat လို packages တွေကတော့ Stand-alon တွေဖြစ်ပါတယ် ။ အဲဒီလိုဖန်တီးထားတဲ့ packages တွေကို Laravel မှာသုံးမယ်ဆိုရင်တော့ ထုံးစံတိုင်း "composer.json" ဖိုင်မှာ သွားထည့့်ပေးလိုက်တာနဲ့ သုံးပြုနိုင်မှာပါ။

တခြားတချက်ကတော့ တခြား packages တွေက Laravel အတွက်ပဲလို့ အသေသတ်မှတ်ပီး ထုတ်လုပ်ထားတဲ့ packages တွေလဲ ရှိပါတယ် ။ ဥပမာ အရင် laravel version တွေမှာ တုန်းက "bundles" လို packages တွေမျိုးပါ။ အဲဒီ packages တွေမှာ  routes, controllers, views, configuration, နဲ့ migrations ဖွဲ့စည်းထားပြီး laravel ရဲ့  လုပ်ဆောင်နိုင်မူတွေကို တိုးချဲ့ အသုံးပြုနိုင်ပါတယ်။ Stan-alone packages တစ်ခု ဖန်တီးဖို့ဆိုတာ အရမ်းခက်တဲ့ ကိစ္စတော့မဟုတ်ပါဘူး ၊ အခုအောက်မှာ ထပ်ဖော်ပြမယ့် နည်းလမ်းတွေအတိုင်း ဖန်တီးကြည့်နိုင်ပါတယ်။

Laravel အတွက် Packages တွေကို  [Packagist](http://packagist.org)မှာတင်ပြီး ဖြန့်ချီနိုင်ပြီး [Composer](http://getcomposer.org) လို အရမ်းမိုက်တဲ့ Package destributuin tool တွေသုံးပြု ပြီး ဖန်တီးရမှာပါ။

<a name="creating-a-package"></a>
## Creating A Package

Laravel မှာသုံးပြုဖို့အတွက် packages တစ်ခုတည်ဆောက်ဖို့အတွက်ကတော့ 'workbench' Artisan command ကို အသုံးပြုပြီးလွယ်လွယ်ကူကူကိုဖန်တီးနိုင်ပါတယ်။ အဲလိုလုပ်ဖို့အတွက် ပထမဆုံး 'app/config/workbench.php' မှာ name နဲ့ email လေးအရင်သွားဖြည့်ပေးရပါတယ်။ အဲဒီ name နဲ့ email ကို အသစ်ဆောက်မယ့် packages တွေ က 'composer.json' မှာ ပြန်အသုံးပြုဖို့အတွက်ဖြစ်ပါတယ်။ကဲ့ ဒီလောက်ပြင်ဆင်ပြီးရင် package တစ်ခု တည်ဆောက်ဖို့ အဆင်သင့်ဖြစ်နေပါပြီ။အောက်က ကွန်မန်းကို Terminal(cmd) မှာ ထည့် run လိုက်ပါ။

#### Issuing The Workbench Artisan Command

	php artisan workbench vendor/package --resources

အပေါ်က command ထဲမှာ vendor ဆိုတာက package တစ်ခုကို authors တွေခွဲရေးတဲ့အခါမှာ package name ကို ခွဲခွဲခြားခြားသိနိုင်အောင်ပေးထားတဲ့နာမည်ဖြစ်ပါတယ်။ Vendor ဆိုတာက အဲဒီ package ကို ဖန်တီးတဲ့လူဖြစ်ပြီး package ဆိုတာကတော့ ကိုယ်လုပ်တဲ့ package name ဖြစ်ပါတယ်။ ဥပမာ ကျွန်တော် Taylar Otwell က "Zapper" ဆိုတဲ့ package တစ်ခုတည်ဆောက်လိုက်ရင် Package name က 'Zapper' ဖြစ်ပြး Vendor name က Taylar ဖြစ်ပါတယ်။ပုံမှန်အားဖြင့်တော့ workbench က framework package တခုတည်ဆောက်ပါတယ်။ "--resources" command က workbench ကို `migrations`, `views`, `config`, စသဖြင့်လိုအပ်တဲ့ ဖိုင်တွေကို ဖန်တီးပေးဖို ့ပြောပါတယ်။

အပေါ်က 'Workbench' ကို run ပြီးပြီဆိုရင်တော့ ၊ ကိုယ်ပေးထားတဲ့ နာမည်အတိုင်းပဲ 'workbench' ဆိုတဲ့ဖိုဒါထဲမှာ vendor name နဲ့ ဖိုဒါတွေရောက်လာပြီး အထဲမှာ package နာမည်နဲ့ လိုအပ်တဲ့ဖိုင်တွေအကုန် အလိုလျောက်ရှိနေပါလိမ့်မယ်။ ပြီးရင်တော့ တည်ဆောက်လိုက်တဲ့ package ကို laravel ကနေ သုံးပြုနိုင်ရန်အတွက် 'ServiceProvider' ကြော်ငြာပေးရပါတယ်။ Service Provider ကို 'app/config/app.php' မှာ သွားထည့်ပေးရပါတယ်။အဲဒီမှာ သွားထည့်ပေးလိုက်ရင် workbench ထဲက package တွေကို laravel ကနေ အသုံးပြုနိုင်ပါပြီ။ Service Provider က '[Package]ServiceProvider' ကိုအသုံးပြုပါတယ်။ဥပမာအရဆိုရင် 'app/config/app.php' က Provider မှာ 'Taylor\Zapper\ZapperServiceProvider' ဆိုပြီး array ထဲမှာ သွားထည့်ပေးရမှာပါ။

အခုလို Provider မှာ သွားထည့်ပေးပြီးရင်တော့ packages ကို လိုအပ်သလိုမျိုး စတင် အသုံးပြုနိုပ်ပါပြီ။ ပထမဆုံး package structure နဲ ့ development workflow ကို အရင်လေ့လာသင့်ပါတယ်။

> **Note:** Service Provider cannot be found ဆိုပြီး error ပြနေရင် `php artisan dump-autoload` ကို root directory မှာ terminal(cmd) မှ တစ်ဆင့် run ပြီး ပြန်စမ်းကြည့်ပါ။

<a name="package-structure"></a>
## Package Structure

'workbench' command ကို အသုံးပြုပြီးတဲ့အခါမှာ အဲဒီ command က ကိုယ်ဖန်တီးလိုက်တဲ့ packages ကို laravel နှင့် တွဲဖက်အသုံးပြုနိုင်အောင် အကုန်အလိုလျောက်ပြုလုပ်ပေးပါတယ်။

#### Basic Package Directory Structure

	/src
		/Vendor
			/Package
				PackageServiceProvider.php
		/config
		/lang
		/migrations
		/views
	/tests
	/public

အပေါ်က file structure ကိုအရင်လေ့လာကြည့်ရအောင်။ 'src/Vendor/Package' က တော့ 'ServiceProvider' ပါဝင်တဲ့အတွက် package's classes တွေရဲ့  အဓိကနေရာလို့ပြောရမှာပါ။ `config`, `lang`, `migrations`, နဲ့ `views' တွေကတော့ packages အတွက် လိုအပ်တဲ့ resources တွေပါဝင်မယ့်ဖိုင်တွေဖြစ်ပါတယ်။
Packages တစ်ခုမှာလဲ Laravel မှာရှိတဲ့ resources တွေ အတိုင်း တည်ရှိနေမှာပါ။

<a name="service-providers"></a>
## Service Providers

Service providers ဖိုင်တွေကတော့ packages တွေရဲ့ အသက်ဖိုင်လို့ပြောရမှာပါ။ပုံမှန်အားဖြင့် Service Provider မှာ 'boot' နဲ့ 'register' ဆိုတဲ့ methodsနှစ်ခုပါဝင်ပါတယ်။
ဒီ methods နှစ်ခုမှာပဲ အကုန်လုံးပြုလုပ်နိုင်ပါတယ်။ ဥပမာ routes ဖိုင်ချိတ်ဖို့ ၊IoC Container တွေ register bindings လုပ်ဖို့ ၊ events တွေထည့်ဖို့ ၊ အကုန်လုံးနည်းပါးကို ဒီ method နှစ်ခုတစ်ဆင့် အလုပ်လုပ်သွားမှာပါ။

"register" method က Service Provider ကို register ပြုလုပ်ပြီးတာနဲ့ အလုပ်လုပ်မယ့် method ဖြစ်ပါတယ်။ 'boot' method ကတော့ request အသက်မဝင်ခင်အချိန်ထိပဲ အလုပ်လုပ်မှာဖြစ်ပါတယ်။ ဒါဆိုရင်တော့ service provider ထဲက actions တွေ registe လုပ်ပြီးတဲ့အချိန် (သို ့) တခြား provider တစ်ခုရဲ့  service ကို ကျော်လွန်(override)အသုံးပြုလိုပါက 'boot' method ကို အသုံးပြုသင့်ပါတယ်။

'workbench' command နှင့် package တစ်ခုတည်ဆောက်လိုက်တာနဲ့ 'boot' method မှာ အောက်ဖော်ပြပါအတိုင်း action တစ်ခု ပါဝင်နေပါတယ်။

	$this->package('vendor/package');

ဒီ method က laravel ကို packages ထဲက views,config, other resource တွေကို အသုံးပြုနိုင်အောင်လုပ်ပေးပါတယ်။ ပုံမှန်အားဖြင့်တော့ အဲဒီ ကုတ်ကို ပြုပြင်ဖို့မလိုအပ်ပါဘူး။

ပုံမှန်အားဖြင့် package တစ်ခုတည်ဆောက်ပြီးတဲ့အခါ အဲဒီ packages ရဲ့  resource တွေက 'vendor/package' အောက်မှာရှိပါတယ်။ဘယ်လိုဖြစ်ဖြစ်  package method ကို argument နောက်တစ်ခု ထပ်ထည့်ပြီး package resource နေရာတွေကို လိုအပ်သလို အောက်ကပုံစံအတိုင်း ပြောင်းလဲနိုင်ပါသေးတယ်။

	// Passing custom namespace to package method
	$this->package('vendor/package', 'custom-namespace');

	// Package resources now accessed via custom-namespace
	$view = View::make('custom-namespace::foo');

Service provider classes တွေအတွက် app directory ထဲမှာ နေရာအတည်တစ်ကျ သတ်မှတ်ထားတာမျိုးလဲမရှိပါဘူး။ 'app' ထဲမှ  'Providers' namespace ပေးပြီး ထားချင်တဲ့နေရာမှာ ထားနိုင်ပါတယ်။ ဒဲဒီ class ဖိုင်တွေကို Composer's [auto-loading facilities](http://getcomposer.org/doc/01-basic-usage.md#autoloading) က သိမှတ်ပြုနေသ၍ အဲဒီ class ဖိုင်ထဲက class တွေကို app က ယူသုံးနိုင်မှာပါ။

'Package ထဲက resources ( ဥပမာ Configuration ၊ Views ) နေရာတွေကို ပြောင်းလိုက်ပြီဆိုရင် ပြောင်းလိုက်တဲ့နေရာကို 'package' methord မှာ တတိယမြောက် argument တစ်ခုအဖြစ် အောက်ပါအတိုင်းထည့်သင့်ပေးသင့်ပါတယ်။

	$this->package('vendor/package', null, '/path/to/resources');

<a name="deferred-providers"></a>
## Deferred Providers


If you are writing a service provider that does not register any resources such as configuration or views, you may choose to make your provider "deferred". A deferred service provider is only loaded and registered when one of the services it provides is actually needed by the application IoC container. If none of the provider's services are needed for a given request cycle, the provider is never loaded.

To defer the execution of your service provider, set the `defer` property on the provider to `true`:

	protected $defer = true;

Next you should override the `provides` method from the base `Illuminate\Support\ServiceProvider` class and return an array of all of the bindings that your provider adds to the IoC container. For example, if your provider registers `package.service` and `package.another-service` in the IoC container, your `provides` method should look like this:

	public function provides()
	{
		return array('package.service', 'package.another-service');
	}

<a name="package-conventions"></a>
## Package Conventions


When utilizing resources from a package, such as configuration items or views, a double-colon syntax will generally be used:

#### Loading A View From A Package

	return View::make('package::view.name');

#### Retrieving A Package Configuration Item

	return Config::get('package::group.option');

> **Note:** If your package contains migrations, consider prefixing the migration name with your package name to avoid potential class name conflicts with other packages.

<a name="development-workflow"></a>
## Development Workflow


When developing a package, it is useful to be able to develop within the context of an application, allowing you to easily view and experiment with your templates, etc. So, to get started, install a fresh copy of the Laravel framework, then use the `workbench` command to create your package structure.

After the `workbench` command has created your package. You may `git init` from the `workbench/[vendor]/[package]` directory and `git push` your package straight from the workbench! This will allow you to conveniently develop the package in an application context without being bogged down by constant `composer update` commands.

Since your packages are in the `workbench` directory, you may be wondering how Composer knows to autoload your package's files. When the `workbench` directory exists, Laravel will intelligently scan it for packages, loading their Composer autoload files when the application starts!

If you need to regenerate your package's autoload files, you may use the `php artisan dump-autoload` command. This command will regenerate the autoload files for your root project, as well as any workbenches you have created.

#### Running The Artisan Autoload Command

	php artisan dump-autoload

<a name="package-routing"></a>
## Package Routing

In prior versions of Laravel, a `handles` clause was used to specify which URIs a package could respond to. However, in Laravel 4, a package may respond to any URI. To load a routes file for your package, simply `include` it from within your service provider's `boot` method.

#### Including A Routes File From A Service Provider

	public function boot()
	{
		$this->package('vendor/package');

		include __DIR__.'/../../routes.php';
	}

> **Note:** If your package is using controllers, you will need to make sure they are properly configured in your `composer.json` file's auto-load section.

<a name="package-configuration"></a>
## Package Configuration

#### Accessing Package Configuration Files

Some packages may require configuration files. These files should be defined in the same way as typical application configuration files. And, when using the default `$this->package` method of registering resources in your service provider, may be accessed using the usual "double-colon" syntax:

	Config::get('package::file.option');

#### Accessing Single File Package Configuration

However, if your package contains a single configuration file, you may simply name the file `config.php`. When this is done, you may access the options directly, without specifying the file name:

	Config::get('package::option');

#### Registering A Resource Namespace Manually

Sometimes, you may wish to register package resources such as views outside of the typical `$this->package` method. Typically, this would only be done if the resources were not in a conventional location. To register the resources manually, you may use the `addNamespace` method of the `View`, `Lang`, and `Config` classes:

	View::addNamespace('package', __DIR__.'/path/to/views');

Once the namespace has been registered, you may use the namespace name and the "double colon" syntax to access the resources:

	return View::make('package::view.name');

The method signature for `addNamespace` is identical on the `View`, `Lang`, and `Config` classes.

### Cascading Configuration Files

When other developers install your package, they may wish to override some of the configuration options. However, if they change the values in your package source code, they will be overwritten the next time Composer updates the package. Instead, the `config:publish` artisan command should be used:

	php artisan config:publish vendor/package

When this command is executed, the configuration files for your application will be copied to `app/config/packages/vendor/package` where they can be safely modified by the developer!

> **Note:** The developer may also create environment specific configuration files for your package by placing them in `app/config/packages/vendor/package/environment`.

<a name="package-views"></a>
## Package Views

If you are using a package in your application, you may occasionally wish to customize the package's views. You can easily export the package views to your own `app/views` directory using the `view:publish` Artisan command:

	php artisan view:publish vendor/package

This command will move the package's views into the `app/views/packages` directory. If this directory doesn't already exist, it will be created when you run the command. Once the views have been published, you may tweak them to your liking! The exported views will automatically take precedence over the package's own view files.

<a name="package-migrations"></a>
## Package Migrations

#### Creating Migrations For Workbench Packages

You may easily create and run migrations for any of your packages. To create a migration for a package in the workbench, use the `--bench` option:

	php artisan migrate:make create_users_table --bench="vendor/package"

#### Running Migrations For Workbench Packages

	php artisan migrate --bench="vendor/package"

#### Running Migrations For An Installed Package

Packages ထဲမှာ database migrate လုပ်ဖို့အတွက် workbench ထဲမှာ 
To run migrations for a finished package that was installed via Composer into the `vendor` directory, you may use the `--package` directive:

	php artisan migrate --package="vendor/package"

<a name="package-assets"></a>
## Package Assets

#### Moving Package Assets To Public
'packages' တွေမှာ 'Javascript, Css, images လို assets တွေပါကောင်းပါနိုင်ပါတယ်။ အဲဒီ assets တွေကို app မှ တဆင့်တန်းဆွဲခေါ်သုံးဖို့မဖြစ်နိုင်ပါဘူး  ။ အဲဒီအတွက် 'package' ထဲက assets တွေကို public အောက်ကို ပြောင်းထည့်ပေးဖို့လိုအပ်ပါတယ်။ အဲဒီအတွက် `asset:publish`  ကွန်မန်း ကို  အောက်ကအတိုင်း အသုံးပြုပြီး ပြောင်းထည့်ပေးနိုင်ပါတယ်။

	php artisan asset:publish

	php artisan asset:publish vendor/package

တကယ်လို တည်ဆောက်ထားတဲ့ 'package' က 'workbench' အောက်မှာပဲရှိသေးရင်တော့ ' --bench ' ကိုအောက်ပါအတိုင်းထပ်ထည့်ပြီးရေးပေးရပါတယ်။

	php artisan asset:publish --bench="vendor/package"

ဒီကွန်မန်းက package ထဲမှ assets တွေကို 'public/packages' ထဲကို သက်ဆိုင်ရင် package နဲ့ vendor နာမည်တွေအလိုက်ဖိုဒါတွေ အလိုလျောက်ဆောက်ပြီး သိမ်းဆည်းပေးသွားမှာပါ။ ဥပမာ 'workbench' အောက်မှာ 'usersape/kusod' ဆိုပြီး packages ဆောက်ထားရင် 'public/packages/userscape/kudos' ဆိုပြီး ရောက်သွားမှာပါ။ ဒီလိုလုပ်ခြင်းအားဖြင့် asset တွေနဲ့ပက်သက်ပြီး  လုံခြုံရေးဆိုင်ရာ အားသာချက်များ ရရှိနိုင်ပါတယ်။

<a name="publishing-packages"></a>
## Publishing Packages

ကိုယ်တည်ဆောက်ထားတဲ့'Package' က အသုံးပြုဖို ့အားလုံးပြင်ဆင်ပြီးသွားရင်တော့ [Packagist](http://packagist.org) ကို တခြားသူတွေပါသုံးပြုနိုင်အောင် တင်ထားပေးသင့်ပါတယ်။ တကယ်လို့ ကိုယ် တည်ဆောက်လိုက်တဲ့ 'package' က laravel အတွက်ပဲ သီးသန့်တည်ဆောက်ထားရင်တော့ 'composer.json' မှာ 'laravel' ဆိုပြီး tag ထည့်ပေးဖို့လိုအပ်ပါတယ်။


Also, it is courteous and helpful to tag your releases so that developers can depend on stable versions when requesting your package in their `composer.json` files. If a stable version is not ready, consider using the `branch-alias` Composer directive.

Once your package has been published, feel free to continue developing it within the application context created by `workbench`. This is a great way to continue to conveniently develop the package even after it has been published.

Some organizations choose to host their own private repository of packages for their own developers. If you are interested in doing this, review the documentation for the [Satis](http://github.com/composer/satis) project provided by the Composer team.
