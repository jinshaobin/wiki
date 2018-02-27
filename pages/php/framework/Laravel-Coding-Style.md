<link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.12.0/styles/default.min.css" type="text/css">
<script type="text/javascript" src="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.12.0/highlight.min.js"></script>


# Laravel 框架编码规范

## 开发约定


- DRY –「Don't Repeat Yourself」不写重复的逻辑代码。

- COC -「Convention Over Configuration」优先选择框架提倡的做法，不过度配置。

### 1. 关于「能愿动词」的使用

- **必须（Must）** - 只能这样子做，请无条件遵循，没有别的选项；
- **绝不（Must Not)** - 严令禁止，在任何情况下都不能这样做；
- **应该（Should)** - 强烈建议这样做，但是不强求；
- **不应该（Should Not)** - 强烈建议不这样做，但是不强求；
- **可以（May）** - 选择性高一点，此词语使用较少；
    
```
参考：RFC 2119
```


### 2. 开发专用扩展包

Laravel 扩展包的注册会对应用造成消耗。有一些扩展包是开发环境中专用，**生产环境中并不会使用到，为了避免无用的负载， 必须严格控制其安装和加载**。

---

**安装开发专用扩展包时 ``必须`` 使用 --dev 参数，如：**

```
composer require laracasts/generators --dev
```

---

开发专用的 provider **``绝不``** 在 ``config/app.php``  里面注册，**``必须``** 在 ``app/Providers/AppServiceProvider.php`` 文件中使用如以下方式：

```php
public function register()
{
    if ($this->app->environment() == 'local') {
        $this->app->register(\Laracasts\Generators\GeneratorsServiceProvider::class);
    }
}
```

### 3 .配置信息与环境变量

假如我们有个『CDN 域名』的变量，在 Laravel 中有以下几种方法：

- 硬代码，直接写死。- ❌ 可维护性低
- 写死在 config/app.php 文件中。 - ❌ 无法区分环境进行配置
- 存储于 .env 文件中，使用 env() 方法直接读取。 - ✅  虽然解决了环境变量问题但是不推荐
- 存储在 .env 和 config/app.php 文件中，然后使用 config() 函数来读取。- ✅ 最佳实践

代码示例

``.env`` 文件中设置：

```
CDN_DOMAIN=cdndomain.com
```

 config/app.php 文件中设置：

```php
'cdn_domain' => env('CDN_DOMAIN', null),
```

程序中两种获取 相同配置 的方法：

```php
env('CDN_DOMAIN')

config('app.cdn_domain')
```

---

在此统一规定：
所有程序配置信息 **``必须``** 通过 config() 来读取，所有的 ``.env`` 配置信息 **``必须``** 通过 ``config()`` 来读取，
**``绝不``** 在配置文件以外的范围使用 env()。


> 这样做主要有以下几个优势：

- 定义分明，config() 是配置信息，env() 只是用来区分不同环境；
- 统一放置于 config 中还可以利用框架的 [配置信息缓存功能](https://laravel.com/docs/5.5/deployment#optimizing-configuration-loading) 来提高运行效率；
- 代码健壮性， config() 在 env() 之上多出来一个抽象层，会使代码更加健壮，更加灵活。


### 4. 辅助函数

Laravel 提供了很多 辅助函数，有时候我们也 **``可以``** 创建自己的辅助函数 (不推荐使用)。

**``必须``** 把所有的『自定义辅助函数』存放于 bootstrap 文件夹中。
并在 ``bootstrap/app.php`` 文件的最顶部进行加载：

```php
<?php

require __DIR__ . '/helpers.php';

...
```

### 5. 代码风格

代码风格 **``必须``** 严格遵循 [PSR-2](https://www.php-fig.org/psr/psr-2) 规范。


### 6. 类字面表示

PHP自5.6 版本之后使用 ``::class`` 完成该类的字面表示， **``必须``**在用到的地方，需给类的完整字面表示，例：

```php
Illuminate\Database\Contracts\Adapter::class
```



---


## 命名与书写规范：


### 1. 数据模型

#### 1.1 存放路径

所有的数据模型文件，都 **``必须``** 存放在：``app/Models/`` 文件夹中。

命名空间：

```php
namespace App\Models;
```

后期可根据 ``Domain、Module`` 来扩展, 例如:

```php
namespace App\Models\DomainName\SubModule;
```

Laravel 5.x 默认安装会把 ``User`` 模型存放在 ``app/User.php``，必须 移动到 ``app/Models`` 文件夹中，并修改命名空间声明为 ``App/Models``，同上。

为了不破坏原有的逻辑点，必须 全局搜索 ``App/User`` 并替换为 ``App/Models/User``。


- 使用 Artisan 创建时修改路径

```bash
php artisan make:model  App\\Models\\Order
```

#### 1.2 相关命名：


- 数据模型类名 必须 为「单数」, 如：App\Models\Order

- 类文件名 必须 为「单数」，如：app/Models/Order.php



( **待讨论** )
- 数据库表名字 必须 为「复数」，多个单词情况下使用「Snake Case」 如：photos, profile_photos

- 数据库表迁移名字 必须 为「复数」，如：2014_08_08_234417_create_profile_photos_table.php
-  数据填充文件名 必须 为「复数」，如：PhotosTableSeeder.php
- 数据库字段名 必须 为「Snake Case」，如：view_count, is_vip
- 数据库表主键 必须 为「id」
- 数据库表外键 必须 为「resource_id」，如：user_id, post_id
- 数据模型变量 必须 为「resource_id」，如：$user_id, $post_id

#### 1.3 利用 Trait 来扩展数据模型

有时候数据模型里的代码会变得很臃肿，应该 利用 Trait 来精简逻辑代码量，提高可读性


#### 1.4 Repository 

[暂未拟定]

#### 1.5 全局作用域

Laravel 的 Model 全局作用域 允许我们为给定模型的所有查询添加默认的条件约束。

所有的全局作用域都  **``必须``**  统一使用 闭包定义全局作用域，如下：


```php
<?php

namespace App\Scopes;

use Illuminate\Database\Eloquent\Scope;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Builder;

class AgeScope implements Scope
{
    /**
     * 应用作用域
     *
     * @param \Illuminate\Database\Eloquent\Builder  $builder
     * @param \Illuminate\Database\Eloquent\Model  $model
     * @return void
     */
    public function apply(Builder $builder, Model $model)
    {
        return $builder->where('age', '>', 200);
    }
}
```

要将全局作用域分配给模型，需要重写给定模型的 boot 方法并使用 addGlobalScope 方法：


```php
<?php

namespace App\Models;

use App\Scopes\AgeScope;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * 数据模型的启动方法
     *
     * @return void
     */
    protected static function boot()
    {
        parent::boot();

        static::addGlobalScope(new AgeScope);
    }
}
```



### 2. 控制器


**``应该``** 优先使用 Restful 资源控制器 。

---

**``必须``** 使用资源的复数形式，如：

```
- 类名：PhotosController
- 文件名：PhotosController.php
```

错误的范例：

```
- 类名：PhotoController
- 文件名：PhotoController.php
```

**``应该``** 保持控制器文件代码行数最小化，还有可读性。

- **``应该``** 为「方法」书写注释，这要求方法取名要足够合理，不需要过多注释；
- **``应该``**  为一些复杂的逻辑代码块书写注释，主要介绍产品逻辑 - 为什么要这么做；
- **``不应该``** 在控制器中书写「私有方法」，控制器里 应该 只存放「路由动作方法」；
- **``绝不``** 遗留「死方法」，就是没有用到的方法，控制器里的所有方法，都应该被使用到，否则应该删除；



### 3. 数据验证和授权策略 ( 拟定 )

3.1 数据验证

    (暂未拟定)


3.2 授权策略

``必须`` 使用 授权策略 类来做用户授权。

所有 Policy 授权策略类 **``必须``** 继承 app/Policies/Policy.php 基类。基类文件如下：

```php
<?php

namespace App\Policies;

use Illuminate\Auth\Access\HandlesAuthorization;

class Policy
{
    use HandlesAuthorization;

    public function __construct()
    {
        //
    }

    public function before($user, $ability)
    {
        // handle it , generate condition => $condition

        if ($condition) {
            return true;
        }
    }
}
```


Policy 授权策略类 **``必须``** 遵循资源路由方式进行命名，photos 对应 ``app/Policies/PhotoPolicy.php`` 。Policy 授权策略类文件内容请参考以下：

```php
<?php

namespace App\Policies;

use App\Models\User;
use App\Models\Photo;

class PhotoPolicy extends Policy
{
    public function update(User $user, Photo $photo)
    {
        return $user->isAuthorOf($photo);
    }

    public function destroy(User $user, Photo $photo)
    {
        return $user->isAuthorOf($photo);
    }
}
```


**``应该``** 使用自动判断授权策略方法，这样控制器和授权类的方法名可统一。

```php
/**
 * 更新指定的文章。
 *
 * @param  int  $id
 * @return Response
 */
public function update($id)
{
    $article = Article::find($id);
    // 会自动调用 `PhotoPolicy` 类中的 `update` 方法。
    $this->authorize($article);
    // 更新文章...
}

```




### 4. Artisan 命令行

所有的自定义命令，都 **``必须``** 有项目的命名空间。


正确范例:

```bash
php artisan gigahome:clear-token
php artisan gigahome:send-status-email
```

错误范例：

```bash
php artisan clear-token
php artisan send-status-email
```


### 5. 日期和时间

**``应该``** 使用 [Carbon](https://github.com/briannesbitt/Carbon) 来处理日期和时间相关的操作。

packagist.org 包名： ``Carbon/Carbon``


Laravel 5.5 版本的 diffForHumans，只需要在 ``config/app.php`` 文件中配置 locale 选项即可 ：

```php
'locale' => 'zh-CN',
```


### 6. 前端开发

开发非 SPA 应用，前端资源需要遵守以下规则

- ``必须`` 使用 Laravel 官方前端工具做前端开发自动化；
- ``必须`` 保证页面只加载一个 .css 文件；
- ``必须`` 保证页面只加载一个 .js 文件；
- ``必须`` 为 .css 和 .js 增加 版本控制；
- ``必须`` 使用 SASS 来书写 CSS 代码

### 7. 中间件

Laravel 提供了区别于其他PHP框架重要的组件即：中间件

存放位置在 ``app/Http/Middleware`` 目录下，命名以 Middleware 结尾， 范例：

```
app/Http/Middleware/PolicyHandleMiddleware.php 
```

来区别于框架原有的中间件

Auth 中间件 必须 书写在控制器的 ``__construct`` 方法中，并且 必须 使用 ``except`` 黑名单进行过滤，这样当你新增控制器方法时，默认是安全的。

```php
public function __construct()
{
    $this->middleware('auth', [            
        'except' => ['show', 'index']
    ]);
}
```


### 8. 路由

8.1 关于路由闭包

**``绝不``** 在路由配置文件里书写『闭包』或者其他业务逻辑代码，因为一旦使用将无法使用 ``路由缓存`` 。
路由要保持干净整洁，**``绝不``**放置除路由配置以外的其他程序逻辑。

``应该`` 优先使用 Restful 路由，配合资源控制器使用，见其他 RESTUL 文档。



8.2 resource 方法正确使用   (拟定)


一般资源路由定义：

```php
Route::resource('photos', 'PhotosController');
```

等于以下路由定义：

```php
Route::get('/photos', 'PhotosController@index')->name('photos.index');

Route::get('/photos/create', 'PhotosController@create')->name('photos.create');

Route::post('/photos', 'PhotosController@store')->name('photos.store');

Route::get('/photos/{photo}', 'PhotosController@show')->name('photos.show');

Route::get('/photos/{photo}/edit', 'PhotosController@edit')->name('photos.edit');

Route::put('/photos/{photo}', 'PhotosController@update')->name('photos.update');

Route::delete('/photos/{photo}', 'PhotosController@destroy')->name('photos.destroy');
```

使用 ``resource`` 方法时，如果仅使用到部分路由，**``必须``** 使用 only 列出所有可用路由： 不应该使用 ``except``，因为 ``only`` 相当于白名单，相对于 except 更加直观。路由使用白名单有利于安全配置。


8.3 路由词的复数/单数

资源路由路由 URI **``必须``** 使用复数形式，如：

```
/photos/create
/photos/{photo}
```

错误的例子如：
```
/photo/create
/photo/{photo}
```

8.4 路由命名

除了 resource 资源路由以外，其他所有路由都 **``必须``** 使用 ``name`` 方法进行命名。

**``必须``** 使用『资源前缀』作为命名规范，如下的 ``users.follow``，资源前缀的值是 ``users.``：

```php
Route::post('users/{id}/follow', 'UsersController@follow')->name('users.follow');
```


8.5 路由模型绑定

在允许使用路由``模型绑定``的地方 **``必须``** 模型绑定放置于 ``app/Providers/RouteServiceProvider.php`` 文件的 boot 方法中


```php
<?php

namespace App\Providers;

use ...

class RouteServiceProvider extends ServiceProvider
{
    /**
     * Define your route model bindings, pattern filters, etc.
     * @return void
     */
    public function boot()
    {
        //
        Route::bind('user_name', function ($value) {
            return User::where('name', $value)->first();
        });

        Route::bind('photo', function ($value) {
            return Photo::find($value);
        });

        parent::boot();
    }

    ...
}
```

8.6 路由参数绑定


出于安全考虑，**``应该``** 使用全局路由器参数限制。

必须 在 ``RouteServiceProvider`` 文件的 ``boot`` 方法里定义模式：

```php
/**
 * 定义路由模型绑定，模式过滤器等。
 *
 * @param  \Illuminate\Routing\Router  $router
 * @return void
 */
public function boot(Router $router)
{
    $router->pattern('id', '[0-9]+');

    parent::boot($router);
}
```

模式一旦被定义，便会自动应用到所有使用该参数名称的路由上：

```php
Route::get('users/{id}', 'UsersController@show');
Route::get('photos/{id}', 'PhotosController@show');
```

只有在 id 为数字时，才会路由到控制器方法中，否则 404 错误。



### 9.依赖注入：


Laravel 框架提供了三种依赖注入的方法， 包括构造函数注入，参数调用注入，接口(Interface/Contracts) 注入 

- 构造函数注入, 类内的成员名 **``必须``** 和注入的参数一致


```php
namespace App\Http\Controllers;

use ...
use App\Policies\Handlers\DevicePolicyHandler;

/**
 *
 */
class PhotosController extends Controller
{
    /**
     *@var DevicePolicyHandler 
     */
    protected $handler;

    /**
     * Constructor For PhototsController
     *@param DevicePolicyHandler $handler
     */
    public function __construct(DevicePolicyHandler $handler)
    {
        $this->handler = $handler;
    }
}
```


- 参数调用注入

```php

class PhotosRepository 
{
    /**
     *@param NexoMessageSender $sender
     */
    public function upload(DriveMessageUploader $uploader, $streamData)
    {
        $uploader->upload($streamData, config('storage.path'));
    }
}
```

- Interface/Contracts 注入

  Laravel 存在区别于其他PHP框架依赖注入，即 Interface/Contracts 注入

  参考案例：客户端的短信服务商之前是阿里云的SMS， 现在可能需要换成 Nexmo 国际代理， 但在指定的大陆地区内，仍然使用阿里云的SMS， 由于所用的方法不能统一，需要制定接口，Laravel 框架中称之为 Contract （``契约``）


step 1. 创建 ``app/Contracts`` 文件夹， 放置 ``app/Contracts/SmsSenderContract.php``


```php
namespace App\Contracts;

interface SmsSenderContract
{
    public function send($phoneNumber, $message);

    ...
} 
```

step 2. 构建 ``app/Libraries/NexmoSender.php`` ， ``app/Libraries/AliSmsSender.php`` 分别实现 ``SmsSenderContract`` 接口如下：

```php
namespace App\Libraries;

class NexmoSender implements SmsSenderContract
{
    public function send($phoneNumber, $message)
    {
        //
    }
}
```

```php
namespace App\Libraries;

class AliSmsSender implements SmsSenderContract 
{
    public function send($phoneNumber, $message)
    {
        //
    }
}

```

step 3. 构建 ``app/Providers/SmsServiceProvider.php``， 使用 ``Artisan`` 构建

```
$ php artisan make:provider SmsServiceProvider
```

依赖绑定某个具体的代理商到接口中，如下代码


```php
namespace App\Providers;

...
use App\Contracts\SmsSenderContracts;
use App\Libraries\NexmoSender;


class SmsServiceProvider extends ServiceProvider
{
    public function register()
    {
        $this->app->bind(
            SmsSenderContract::class, NexmoSender::class
        )
    }
}
```


step 4. 添加 ``Provider`` 至 ``config/app.php`` 中


```php

'providers' => [
    /*
     * Application Service Providers...
    */
    App\Providers\AppServiceProvider::class,
    App\Providers\AuthServiceProvider::class,
    App\Providers\EventServiceProvider::class,
    App\Providers\RouteServiceProvider::class,
    // Sms service provider
    App\Providers\SmsServiceProvider::class

],

```

step 5. 在使用的地方直接，注入接口：

```php
namespace App\Http\Controllers;

class UsersController extends Controller
{
    public function register(SmsSenderContract $smsSender)
    {
        .... 

        $smsSender->send('{phone}', '{message}');
    }
}

```


## Laravel 核心部分使用规范 (拟定)



### 1. IoC 容器使用规范

Laravel 的IoC 容器提供了强大的类依赖管理和依赖注入执行的能力， 使用 Laravel 的IoC容器 **``不应该``** 使用辅助函数 `` app() ``方法， **``应该``**使用 ``\Illuminate\Container\Container::getInstance() `` 来获取一个单例，如下：


```php
use Illuminate\Container\Container;

...

$container = Container::getInstance(); 

$container->make(Laracasts\Generator\Policy::class);
...

```




### 2. Service Provider 规范

Laravel 提供的 Service Provider, **``必须``** 使用 Artisan 完成构建，命名必须以 ``Provider`` 结尾， 范例：


```
php artisan make:provider BroadcastServiceProvider
```

- 如果是第三方提供的Provider, **``应该``** 对该 Provider 完成 ``vendor:publish``
操作。

- 应该将提供的服务添加至 ``config/app.php`` 列表中
- 每个自定义的 Provider **``必须``** 实现 register 方法
- 每个自定义的 Provider **``应该``** 实现 boot 方法



### 3. Facades 使用规范


暂未拟定 ...

### 4. Contracts 使用规范

暂未拟定 ...