# Service Container

- [မိတ်ဆတ်](#introduction)
- [Binding](#binding)
    - [Binding Basics](#binding-basics)
    - [Binding Interfaces To Implementations](#binding-interfaces-to-implementations)
    - [Contextual Binding](#contextual-binding)
    - [Tagging](#tagging)
- [Resolving](#resolving)
    - [The Make Method](#the-make-method)
    - [Automatic Injection](#automatic-injection)
- [Container Events](#container-events)

<a name="introduction"></a>
## မိတ်ဆတ်

Class dependencies တွေနဲ့ dependency injection တိုကိုလုပ်ဆောင်ရာမှာ Laravel service container က powerful tool တစ်ခုဖြစ်ပါတယ်။ Dependency injection ဆိုတာက fancy စကားစုတစ်ခုပါ... တကယ်တမ်းအခြေခံကဘာကိုဆိုလိုတာလဲဆိုရင် this:class dependencies တွေက class ထဲကို constructor ကနေဒါမှမဟုတ် အချို့ကိစ္စများတွင် "setter" method တွေကိုသုံးပြီးတော့ injected လုပ်တာကိုပြောတာဖြစ်ပါတယ်။

ရိုးရှင်းတဲ့နမူနာတစ်ခုကိုကြည့်လိုက်ကြရအောင်...

    <?php

    namespace App\Http\Controllers;

    use App\User;
    use App\Repositories\UserRepository;
    use App\Http\Controllers\Controller;

    class UserController extends Controller
    {
        /**
         * The user repository implementation.
         *
         * @var UserRepository
         */
        protected $users;

        /**
         * Create a new controller instance.
         *
         * @param  UserRepository  $users
         * @return void
         */
        public function __construct(UserRepository $users)
        {
            $this->users = $users;
        }

        /**
         * Show the profile for the given user.
         *
         * @param  int  $id
         * @return Response
         */
        public function show($id)
        {
            $user = $this->users->find($id);

            return view('user.profile', ['user' => $user]);
        }
    }


အထက်မှာဖော်ပြထားတဲ့နမူနာမှာဆိုရင် `UserController` က data source တစ်ခုကနေပြီးတော့ users တွေကိုပြန်ယူဖို့လိုပါတယ်။ ဒါကြောင့်ကျွန်တော်တို့က serive တစ်ခု **inject** လုပ်ပြီးတော့ users တွေကိုပြန်ယူပါတယ်။ အထက်ဖော်ပြပါနမူနာမှာဆိုရင် `UserRepository` က [Eloquent](/docs/{{version}}/eloquent) သုံးပြီးတော့ user information တွေကို database ကနေပြန်လည်ရယူပါတယ်။ သို့သော်လည်း repository က injected ဖြစ်သွားတော့ကျွန်တော်တို့ကလွယ်ကူစွာတစ်ခြားတစ်ခုကိုပြောင်းလဲနိုင်ပါတယ်။ ကျွန်တော်တို့ application ကို test လုပ်တဲ့အချိန်မှာလည်းလွယ်ကူစွာ mock ဒါမှမဟုတ် dummy implementation တစ်ခုကိုလွယ်ကူစွာတည်ဆောက်နိုင်ပါတယ်။

Powerful/large application တွေတည်ဆောက်တဲ့အချိန်နဲ့ Laravel Core ကိုပြန်ပြီး contribute လုပ်ဖို့ရာအတွက် Laravel service container ကိုနက်နက်နဲနဲနားလည်မှဖြစ်ပါမယ်။

<a name="binding"></a>
## Binding

<a name="binding-basics"></a>
### Binding Basics

Almost all of your service container bindings will be registered within [service providers](/docs/{{version}}/providers), so most of these examples will demonstrate using the container in that context.

> {tip} There is no need to bind classes into the container if they do not depend on any interfaces. The container does not need to be instructed on how to build these objects, since it can automatically resolve these objects using reflection.

#### Simple Bindings

Within a service provider, you always have access to the container via the `$this->app` property. We can register a binding using the `bind` method, passing the class or interface name that we wish to register along with a `Closure` that returns an instance of the class:

    $this->app->bind('HelpSpot\API', function ($app) {
        return new HelpSpot\API($app->make('HttpClient'));
    });

Note that we receive the container itself as an argument to the resolver. We can then use the container to resolve sub-dependencies of the object we are building.

#### Binding A Singleton

The `singleton` method binds a class or interface into the container that should only be resolved one time. Once a singleton binding is resolved, the same object instance will be returned on subsequent calls into the container:

    $this->app->singleton('HelpSpot\API', function ($app) {
        return new HelpSpot\API($app->make('HttpClient'));
    });

#### Binding Instances

You may also bind an existing object instance into the container using the `instance` method. The given instance will always be returned on subsequent calls into the container:

    $api = new HelpSpot\API(new HttpClient);

    $this->app->instance('HelpSpot\Api', $api);

#### Binding Primitives

Sometimes you may have a class that receives some injected classes, but also needs an injected primitive value such as an integer. You may easily use contextual binding to inject any value your class may need:

    $this->app->when('App\Http\Controllers\UserController')
              ->needs('$variableName')
              ->give($value);

<a name="binding-interfaces-to-implementations"></a>
### Binding Interfaces To Implementations

A very powerful feature of the service container is its ability to bind an interface to a given implementation. For example, let's assume we have an `EventPusher` interface and a `RedisEventPusher` implementation. Once we have coded our `RedisEventPusher` implementation of this interface, we can register it with the service container like so:

    $this->app->bind(
        'App\Contracts\EventPusher',
        'App\Services\RedisEventPusher'
    );

This statement tells the container that it should inject the `RedisEventPusher` when a class needs an implementation of `EventPusher`. Now we can type-hint the `EventPusher` interface in a constructor, or any other location where dependencies are injected by the service container:

    use App\Contracts\EventPusher;

    /**
     * Create a new class instance.
     *
     * @param  EventPusher  $pusher
     * @return void
     */
    public function __construct(EventPusher $pusher)
    {
        $this->pusher = $pusher;
    }

<a name="contextual-binding"></a>
### Contextual Binding

Sometimes you may have two classes that utilize the same interface, but you wish to inject different implementations into each class. For example, two controllers may depend on different implementations of the `Illuminate\Contracts\Filesystem\Filesystem` [contract](/docs/{{version}}/contracts). Laravel provides a simple, fluent interface for defining this behavior:

    use Illuminate\Support\Facades\Storage;
    use App\Http\Controllers\PhotoController;
    use App\Http\Controllers\VideoController;
    use Illuminate\Contracts\Filesystem\Filesystem;

    $this->app->when(PhotoController::class)
              ->needs(Filesystem::class)
              ->give(function () {
                  return Storage::disk('local');
              });

    $this->app->when(VideoController::class)
              ->needs(Filesystem::class)
              ->give(function () {
                  return Storage::disk('s3');
              });

<a name="tagging"></a>
### Tagging

Occasionally, you may need to resolve all of a certain "category" of binding. For example, perhaps you are building a report aggregator that receives an array of many different `Report` interface implementations. After registering the `Report` implementations, you can assign them a tag using the `tag` method:

    $this->app->bind('SpeedReport', function () {
        //
    });

    $this->app->bind('MemoryReport', function () {
        //
    });

    $this->app->tag(['SpeedReport', 'MemoryReport'], 'reports');

Once the services have been tagged, you may easily resolve them all via the `tagged` method:

    $this->app->bind('ReportAggregator', function ($app) {
        return new ReportAggregator($app->tagged('reports'));
    });

<a name="resolving"></a>
## Resolving

<a name="the-make-method"></a>
#### The `make` Method

You may use the `make` method to resolve a class instance out of the container. The `make` method accepts the name of the class or interface you wish to resolve:

    $api = $this->app->make('HelpSpot\API');

If you are in a location of your code that does not have access to the `$app` variable, you may use the global `app` helper:

    $api = app('HelpSpot\API');

<a name="automatic-injection"></a>
#### Automatic Injection

Alternatively, and importantly, you may simply "type-hint" the dependency in the constructor of a class that is resolved by the container, including [controllers](/docs/{{version}}/controllers), [event listeners](/docs/{{version}}/events), [queue jobs](/docs/{{version}}/queues), [middleware](/docs/{{version}}/middleware), and more. In practice, this is how most of your objects should be resolved by the container.

For example, you may type-hint a repository defined by your application in a controller's constructor. The repository will automatically be resolved and injected into the class:

    <?php

    namespace App\Http\Controllers;

    use App\Users\Repository as UserRepository;

    class UserController extends Controller
    {
        /**
         * The user repository instance.
         */
        protected $users;

        /**
         * Create a new controller instance.
         *
         * @param  UserRepository  $users
         * @return void
         */
        public function __construct(UserRepository $users)
        {
            $this->users = $users;
        }

        /**
         * Show the user with the given ID.
         *
         * @param  int  $id
         * @return Response
         */
        public function show($id)
        {
            //
        }
    }

<a name="container-events"></a>
## Container Events

The service container fires an event each time it resolves an object. You may listen to this event using the `resolving` method:

    $this->app->resolving(function ($object, $app) {
        // Called when container resolves object of any type...
    });

    $this->app->resolving(HelpSpot\API::class, function ($api, $app) {
        // Called when container resolves objects of type "HelpSpot\API"...
    });

As you can see, the object being resolved will be passed to the callback, allowing you to set any additional properties on the object before it is given to its consumer.
