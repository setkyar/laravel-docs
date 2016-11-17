# Laravel Valet

- [Introduction](#introduction)
    - [Valet Or Homestead](#valet-or-homestead)
- [Installation](#installation)
- [Release Notes](#release-notes)
- [Serving Sites](#serving-sites)
    - [The "Park" Command](#the-park-command)
    - [The "Link" Command](#the-link-command)
    - [Securing Sites With TLS](#securing-sites)
- [Sharing Sites](#sharing-sites)
- [Viewing Logs](#viewing-logs)
- [Custom Valet Drivers](#custom-valet-drivers)
- [Other Valet Commands](#other-valet-commands)

<a name="introduction"></a>
## Introduction

Valet က Mac အတွက်ရိုးရှင်းတဲ့ Laravel development environment တစ်ခုပါဘဲ။ Vagrant မလို၊ Apache မလို၊ Nginx မလို၊ `/etc/hosts` file မလိုပါဘူး။ သင့် site ကို local tunnels သုံးပြီးတော့ publicly share လုပ်လို့ရပါသေးတယ်။

Laravel Valet ကဘယ်လိုအလုပ်လုပ်သလဲဆိုရင်သင့်စက်ဖွင့်လိုက်တာနဲ့ [Caddy](https://caddyserver.com)  ကို background မှာအမြဲ run ထားပြီးတော့ [DnsMasq](https://en.wikipedia.org/wiki/Dnsmasq) ကိုသုံးပြီးတော့ Valet proxies တွေအကုန်လုံးက `*.dev` domain နဲ့ သင် install လုပ်ထားတဲ့ sites တွေနဲ့ point ပေးပါတယ်။

Valet က Vagrant တို့ Homestead တို့ replacement မဟုတ်ပါဘူး။ သင့်စက်ရဲ့ RAM ကို 7 MB လောက်သုံးပြီးတော့တကယ့်ကိုလျှင်မြန်တဲ့ Laravel development တစ်ခုပေါ့။နောက် Laravel valet က RAM အနည်းငယ်ပါတဲ့စက်တွေ… speed ပိုမြန်ချင်တဲ့သူတွေ အတွက်တော့အကောင်းဆုံးရွေးချယ်စရာတစ်ခုပါ။

Out of the box, Valet support includes, but is not limited to:

- [Laravel](https://laravel.com)
- [Lumen](https://lumen.laravel.com)
- [Symfony](https://symfony.com)
- [Zend](http://framework.zend.com)
- [CakePHP 3](http://cakephp.org)
- [WordPress](https://wordpress.org)
- [Bedrock](https://roots.io/bedrock)
- [Craft](https://craftcms.com)
- [Statamic](https://statamic.com)
- [Jigsaw](http://jigsaw.tighten.co)
- Static HTML

Valet ကိုသင့်  [custom drivers](#custom-valet-drivers) တွေနဲ့ extend လုပ်နိုင်ပါတယ်

<a name="valet-or-homestead"></a>
### Valet Or Homestead

Laravel Valet နဲ့ [Homestead](/docs/{{version}}/homestead) ဘာကွာလဲဆိုရင် Homestead က automated Nginx configuration နဲ့ Ubuntu virtual machine ကို offer လုပ်ပါတယ်… သင်က fully virtualized Linux development လိုချင်တယ်နောက်သင့်စက်က Window/linux ဆိုရင်တော့ Homestead ကသင့်အတွက်အကောင်းဆုံးပါ။ 

Valet only supports Mac, and requires you to install PHP and a database server directly onto your local machine. This is easily achieved by using [Homebrew](http://brew.sh/) with commands like `brew install php70` and `brew install mariadb`. Valet provides a blazing fast local development environment with minimal resource consumption, so it's great for developers who only require PHP / MySQL and do not need a fully virtualized development environment.

Both Valet and Homestead are great choices for configuring your Laravel development environment. Which one you choose will depend on your personal taste and your team's needs.

Valet က Mac ကိုဘဲ support လုပ်ပြီးတော့ PHP နဲ့ database server ကိုသင့်စက်ထဲမှာ install လုပ်ထားဖို့လိုပါတယ်။ This is easily achieved by using [Homebrew](http://brew.sh/) with commands like `brew install php70` and `brew install mariadb`

သင့် development က PHP/MySQL ဘဲလိုပြီးတော့ fully virtualized development environment လိုချင်တယ်ဆိုရင်တော့ Laravel Valet ကသင့်အတွက်ပါ။ တကယ်တော့ Homestead ကော Valet ကောနှစ်ခုလုံးသူ့ဟာနဲ့သူကောင်းပါတယ်… သင့်ကိုယ်ပိုင်အကြိုက်နဲ့ သင့် Team လိုအပ်ချက်ပေါ်မူတည်ပြီးတော့သင်ဘယ် development ကိုသုံးမလဲဆိုတာကိုယ်ပိုင်ဆုံးဖြစ်ရမှာပါ။

<a name="installation"></a>
## Installation

**Valet ကို install လုပ်ဖို့ macOS နဲ့ [Homebrew](http://brew.sh/) လိုပါတယ်။ Apache ဒါမှမဟုတ် Nginx တို့ကသင့်စက်ရဲ့ port 80 ကို binding မလုပ်ဖို့တော့လိုပါ့မယ်။**

- [Homebrew](http://brew.sh/) latest version ကို install သို့မဟုတ် update လုပ်ဖို့ `brew update` ကိုသုံးပါ
- PHP7.0 ကို Homebrew သုံးပြီး install လုပ်ဖို့ရာအတွက် `brew install homebrew/php/php70`
-  valet ကို composer သုံးပြီး `composer global require laravel/valet` command ကိုသုံးပြီးတော့ install လုပ်ပါ။ `~/.composer/vendor/bin` directory ကသင့် system PATH မှာရှိဖို့သေချာအောင်လုပ်ပါ။
- `valet install` command ကို run လိုက်ရင် သင့် system စလိုက်ပြီဆိုတာနဲ့ Valet daemon ကို lunch လုပ်ပြီးValet နဲ့ DnsMasq ကို install/configure လုပ်သွားမှာဖြစ်ပါတယ်။

Valet ကို install လုပ်ပြီးသွားတဲ့အခါမှာ *.dev domain ကို terminal ကနေ ping လုပ်ကြည့်လိုက်လို့ရှိရင် Valet မှန်ကန်စွာ install လုပ်ခဲ့တယ်ဆိုရင် 127.0.0.1 ကို respond ပြန်တာကိုတွေ့ရမှာပါ။ example အနေနဲ့ terminal ကနေ `ping foobar.dev` လို့ run ကြည့်လို့ရပါတယ်။

Valet ကသင့်စက် boots လုပ်လိုက်တာနဲ့ သူ့ daemon ကို automatically start လုပ်မှာပါ။ Valet ကို initial installation ပြီးသွားတာနဲ့ `valet start` ဒါမှမဟုတ် `valet install` ကိုနောက်တစ်ကြိမ်ပြန် run စရာမလိုတော့ပါဘူး။

#### Using Another Domain

Valet က default အနေနဲ့ `.dev` domain ကိုအသုံးပြုပါတယ်… သင့်အနေနဲ့တစ်ခြား domain ကိုအသုံးပြုချင်တယ်ဆိုရင် `valet domain tld-name` ဆိုပြီးပြောင်းနိုင်ပါတယ်။ 

ဥပမာအနေနဲ့ `.dev` domain ကို `.app` ဆိုပြီးပြောင်းချင်တယ်ဆိုရင်တော့ `valet domain app` ဆိုပြီး run လိုက်ရင် `.dev` domain တွေအကုန်လုံး `.app` ကိုအလိုအလျှောက်ပြောင်းသွားမှာပါ။

#### Database

If you need a database, try MariaDB by running `brew install mariadb` on your command line. You can connect to the database at `127.0.0.1` using the `root` username and an empty string for the password.

<a name="release-notes"></a>
##Release Notes

### Version 1.1.5
The 1.1.5 release of Valet brings a variety of internal improvements.

#### Upgrade Instructions
After updating your Valet installation using `composer global update`, you should run the `valet install` command in your terminal.

### Version 1.1.0
The 1.1.0 release of Valet brings a variety of great improvements. The built-in PHP server has been replaced with [Caddy](https://caddyserver.com/) for serving incoming HTTP requests. Introducing Caddy allows for a variety of future improvements and allows Valet sites to make HTTP requests to other Valet sites without blocking the built-in PHP server.

#### Upgrade Instructions
After updating your Valet installation using `composer global update`, you should run the `valet install` command in your terminal to create the new Caddy daemon file on your system.

<a name="serving-sites"></a>
##Serving Sites
Valet install လုပ်ပြီးတာနဲ့ sites တွေကို serve လုပ်လို့ရပါပြီ။ Valet က သင့် Laravel sites တွေကို serve လုပ်ဖို့ရာအတွက် `park` နဲ့ `link` ဆိုပြီး command နှစ်ခုရှိပါတယ်။

**The `park` Command**

- Create a new directory on your Mac by running something like `mkdir ~/Sites`. Next, `cd ~/Sites` and run `valet park`. This command will register your current working directory as a path that Valet should search for sites.
- Next, create a new Laravel site within this directory: `laravel new blog`.
- Open `http://blog.dev` in your browser.

**That's all there is to it.** Now, any Laravel project you create within your "parked" directory will automatically be served using the `http://folder-name.dev` convention.

<a name="the-link-command"></a>
**The `link` Command**

The `link` command may also be used to serve your Laravel sites. This command is useful if you want to serve a single site in a directory and not the entire directory.

<div class="content-list" markdown="1">
- To use the command, navigate to one of your projects and run `valet link app-name` in your terminal. Valet will create a symbolic link in `~/.valet/Sites` which points to your current working directory.
- After running the `link` command, you can access the site in your browser at `http://app-name.dev`.
</div>

To see a listing of all of your linked directories, run the `valet links` command. You may use `valet unlink app-name` to destroy the symbolic link.

<a name="securing-sites"></a>
**Securing Sites With TLS**

By default, Valet serves sites over plain HTTP. However, if you would like to serve a site over encrypted TLS using HTTP/2, use the `secure` command. For example, if your site is being served by Valet on the `laravel.dev` domain, you should run the following command to secure it:

    valet secure laravel

To "unsecure" a site and revert back to serving its traffic over plain HTTP, use the `unsecure` command. Like the `secure` command, this command accepts the host name that you wish to unsecure:

    valet unsecure laravel

<a name="sharing-sites"></a>
## Sharing Sites

Valet even includes a command to share your local sites with the world. No additional software installation is required once Valet is installed.

To share a site, navigate to the site's directory in your terminal and run the `valet share` command. A publicly accessible URL will be inserted into your clipboard and is ready to paste directly into your browser. That's it.

To stop sharing your site, hit `Control + C` to cancel the process.

<a name="viewing-logs"></a>
## Viewing Logs

If you would like to stream all of the logs for all of your sites to your terminal, run the `valet logs` command. New log entries will display in your terminal as they occur. This is a great way to stay on top of all of your log files without ever having to leave your terminal.

<a name="custom-valet-drivers"></a>
## Custom Valet Drivers

You can write your own Valet "driver" to serve PHP applications running on another framework or CMS that is not natively supported by Valet. When you install Valet, a `~/.valet/Drivers` directory is created which contains a `SampleValetDriver.php` file. This file contains a sample driver implementation to demonstrate how to write a custom driver. Writing a driver only requires you to implement three methods: `serves`, `isStaticFile`, and `frontControllerPath`.

All three methods receive the `$sitePath`, `$siteName`, and `$uri` values as their arguments. The `$sitePath` is the fully qualified path to the site being served on your machine, such as `/Users/Lisa/Sites/my-project`. The `$siteName` is the "host" / "site name" portion of the domain (`my-project`). The `$uri` is the incoming request URI (`/foo/bar`).

Once you have completed your custom Valet driver, place it in the `~/.valet/Drivers` directory using the `FrameworkValetDriver.php` naming convention. For example, if you are writing a custom valet driver for WordPress, your file name should be `WordPressValetDriver.php`.

Let's take at a sample implementation of each method your custom Valet driver should implement.

#### The `serves` Method

The `serves` method should return `true` if your driver should handle the incoming request. Otherwise, the method should return `false`. So, within this method you should attempt to determine if the given `$sitePath` contains a project of the type you are trying to serve.

For example, let's pretend we are writing a `WordPressValetDriver`. Our serve method might look something like this:

    /**
     * Determine if the driver serves the request.
     *
     * @param  string  $sitePath
     * @param  string  $siteName
     * @param  string  $uri
     * @return void
     */
    public function serves($sitePath, $siteName, $uri)
    {
        return is_dir($sitePath.'/wp-admin');
    }

#### The `isStaticFile` Method

The `isStaticFile` should determine if the incoming request is for a file that is "static", such as an image or a stylesheet. If the file is static, the method should return the fully qualified path to the static file on disk. If the incoming request is not for a static file, the method should return `false`:

    /**
     * Determine if the incoming request is for a static file.
     *
     * @param  string  $sitePath
     * @param  string  $siteName
     * @param  string  $uri
     * @return string|false
     */
    public function isStaticFile($sitePath, $siteName, $uri)
    {
        if (file_exists($staticFilePath = $sitePath.'/public/'.$uri)) {
            return $staticFilePath;
        }

        return false;
    }

> {note} The `isStaticFile` method will only be called if the `serves` method returns `true` for the incoming request and the request URI is not `/`.

#### The `frontControllerPath` Method

The `frontControllerPath` method should return the fully qualified path to your application's "front controller", which is typically your "index.php" file or equivalent:

    /**
     * Get the fully resolved path to the application's front controller.
     *
     * @param  string  $sitePath
     * @param  string  $siteName
     * @param  string  $uri
     * @return string
     */
    public function frontControllerPath($sitePath, $siteName, $uri)
    {
        return $sitePath.'/public/index.php';
    }

<a name="other-valet-commands"></a>
## အခြား Valet Command များ

Command  | Description
------------- | -------------
`valet forget` | Run this command from a "parked" directory to remove it from the parked directory list.
`valet paths` | View all of your "parked" paths.
`valet restart` | Restart the Valet daemon.
`valet start` | Start the Valet daemon.
`valet stop` | Stop the Valet daemon.
`valet uninstall` | Uninstall the Valet daemon entirely.
